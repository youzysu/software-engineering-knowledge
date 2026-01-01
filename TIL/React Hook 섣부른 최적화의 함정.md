# 문제 상황
- useStableCallback을 useEventCallback으로 교체한 후 useStep hook에서 버그 발생
## `useStableCallback` vs `useCallbackEvent`

| 항목     | useStableCallback                     | useEventCallback                                          |
| ------ | ------------------------------------- | --------------------------------------------------------- |
| 렌더링 비용 | 매 렌더링마다 실행, 매번 ref 할당이 렌더링 함수 내부에서 발생 | callback 변경 시만 Effect                                     |
| 문제     | **렌더링 중 즉시 업데이트**                     | [해결] 렌더링 함수가 side effect(렌더링 시 값 변경)를 실행하지 않고 순수 함수에 가까워짐 |
- React 공식 문서 [Ref로 값 참조하기](https://ko.react.dev/learn/referencing-values-with-refs#differences-between-refs-and-state): "렌더링 중에는 current 값을 읽거나 쓰면 안 됩니다."

> 렌더링 중에 ref.current를 읽거나 쓰지 마세요. 렌더링 중에 일부 정보가 필요한 경우 State를 대신 사용하세요. ref.current가 언제 변하는지 React는 모르기 때문에 렌더링할 때 읽어도 컴포넌트의 동작을 예측하기 어렵습니다.

```tsx
import { useCallback, useEffect, useInsertionEffect, useRef } from 'react';

const useEventEffect = typeof window !== 'undefined' ? useInsertionEffect : useEffect;

/**
 * 항상 참조를 유지하는 이벤트 핸들러 함수를 생성합니다.
 *
 * 이 훅은 이벤트 핸들러를 props로 전달함으로 인해 발생하는 불필요한 리렌더링을 방지합니다.
 * Radix UI의 `useCallbackRef`와 유사하게 동작합니다.
 */
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const useEventCallback = <T extends (...args: any[]) => any>(callback: T): T => {
  const ref = useRef(callback);

  useEventEffect(() => {
    ref.current = callback;
  }, [callback]);

  return useCallback(((...args) => ref.current(...args)) as T, []);
};

export default useEventCallback;

```

```tsx
import { useCallback, useRef } from 'react';

/**
 * 항상 참조를 유지하는 콜백 함수를 생성합니다.
 *
 * 개발자의 멘탈 모델에 따라 참조가 유지될 것이라 예상하는 API를 작성할 때 사용합니다. (e.g., `useState`의 dispatch 함수)
 * DX의 향상이 목적이지만 오류를 유발할 수 있으니 신중하게 사용하고, 특별한 이유가 없다면 의존성 배열과 함께 `useCallback`을 사용하세요.
 */
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const useStableCallback = <T extends (...args: any[]) => any>(callback: T): T => {
  const ref = useRef(callback);
  ref.current = callback;

  return useCallback(((...args) => ref.current(...args)) as T, []);
};

export default useStableCallback;
```

# 해결 과정

## 문제 원인
- checkCompletedStep에서 lastCompletedStep 이전 클로저 참조로 인해 UI 버그 발생

```tsx
  const checkCompletedStep = useEventCallback((input: StepIndex | TName) => {
    const index = typeof input === 'string' ? steps.indexOf(input) : input;
    return lastCompletedStep >= index;
  });
```
### useEventCallback 동작
- ref.current 업데이트가 렌더링 단계가 아닌 Effect 단계에서 발생
- 이전 렌더링 단계에서는 아직 업데이트가 되지 않아 이전 클로저를 참조

```tsx
const { step, checkCompletedStep, checkDisabledStep, nextStep, prevStep, goToStep, resetStep } = useStep(steps);

<StepNav.Root step={step} onStepChange={goToStep}>
  <StepNav.Item completed={checkCompletedStep('1')}>Step 1</StepNav.Item>
  <StepNav.Item completed={checkCompletedStep('2')} disabled={checkDisabledStep('2')}>
    Step 2
  </StepNav.Item>
  <StepNav.Item disabled={checkDisabledStep('3')}>Step 3</StepNav.Item>
</StepNav.Root>
```

1. 컴포넌트가 리렌더링 되어 checkCompletedStep 호출
2. useEventCallback으로 감싼 함수는 이전 클로저를 참조하여 최신 lastCompletedStep 값을 사용하지 않아 **completed 상태가 올바르게 표현되지 않음**

## useEventCallback을 useCallback으로 교체

```tsx
const [lastCompletedStep, setLastCompletedStep] = useState<StepIndex>(currentStepIndex - 1);

const checkCompletedStep = useEventCallback((input: StepIndex | TName) => {
  const index = typeof input === 'string' ? steps.indexOf(input) : input;
  return lastCompletedStep >= index;
});
```

```tsx
const [lastCompletedStep, setLastCompletedStep] = useState<StepIndex>(currentStepIndex - 1);
 
const checkCompletedStep = useCallback(
  (input: StepIndex | TName) => {
    const index = typeof input === 'string' ? steps.indexOf(input) : input;
    return lastCompletedStep >= index;
  },
  [lastCompletedStep],
);
```

```tsx
const goToStep = useCallback(
  (input: StepIndex | TName) => {
    const index = typeof input === 'string' ? steps.indexOf(input) : input;

    if (index < 0 || index >= steps.length) {
      // eslint-disable-next-line no-console
      console.error(`Invalid step "${input}".`);
      return;
    }

    if (index > lastCompletedStep + 1) {
      // eslint-disable-next-line no-console
      console.error(`Cannot go to step "${input}" before completing the previous step.`);
      return;
    }

    setCurrentStepIndex(index);
  },
  [lastCompletedStep, **setCurrentStepIndex**],
);
```

# 의존성 배열의 딜레마

- 의존성 누락 시 버그 발생 (클로저 참조 문제)
- 그러나 의존성이 자주 변경되면 메모이제이션 무의미
### 최종적으로 useCallback 제거 결정

```tsx
const goToStep = (input: StepIndex | TName) => {
  const index = typeof input === 'string' ? steps.indexOf(input) : input;

  if (index < 0 || index >= steps.length) {
    // eslint-disable-next-line no-console
    console.error(`Invalid step "${input}".`);
    return;
  }

  if (index > lastCompletedStep + 1) {
    // eslint-disable-next-line no-console
    console.error(`Cannot go to step "${input}" before completing the previous step.`);
    return;
  }

  setCurrentStepIndex(index);
};
```

- 메모이제이션 오버헤드 > 재생성 비용이라는 판단
    - 의존성 배열이 자주 변경되어 메모이제이션 효과 미미
    - 단순한 함수들의 재생성 비용이 실제로 낮음
- 단순한 함수 재생성이 더 효율적, 코드가 더 명확하고 간단해짐
- **useEventCallback 이전 최초 useStableCallback 사용은 의미 없는 최적화**

# 정리

> 측정 우선 (Measure First): 실제 성능 문제 확인 후 최적화 "최적화는 마지막에, 측정 후에, 필요할 때만"

“React Hook의 최적화는 양날의 검입니다. 성능 향상을 위한 시도가 오히려 버그를 만들거나 가독성을 해칠 수 있습니다. 항상 동작하는 코드를 먼저 만들고, 실제 성능 문제가 확인된 후에 신중하게 최적화해야 합니다.”

지금은 자식 컴포넌트의 불필요한 리렌더링을 유발하지 않고, 단계 변경 시에만 렌더링되는 단순한 상황이기 때문에 불필요하다고 판단했지만, 메모이제이션을 하지 않고 자식에게 drilling 되어 전달되거나, 다른 상태들이 추가되는 등 렌더링 빈도가 늘어나면 잠재적으로 문제가 발생할 수 있습니다.

### 🔍 최적화 체크리스트
1. 실제 성능 문제가 있는가?
2. 의존성 배열이 안정적인가?
3. 메모이제이션 비용 < 재계산 비용인가?
4. 자식 컴포넌트 리렌더링을 방지해야 하는가?