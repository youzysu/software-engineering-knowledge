# 들어가며

지금까지 진행한 프로젝트에서는 유저에게 정보를 입력받을 때 필요한 모든 필드를 한 페이지 내부에서 진행하는 경우가 많았어요.
이슈트래커 프로젝트에서도 이슈 콘텐츠, 담당자, 마일스톤, 레이블 모두 한 페이지에서 작성하거나 설정할 수 있었고, 가지마켓 프로젝트에서도 중고 상품 등록 시 필요한 모든 정보를 한 페이지에서 입력받는 구조였어요.

물론 상황에 따라 적합한 방식이 다르겠지만, 최근 UX/UI 트렌드를 보면 한 페이지에서 모든 정보를 입력받는 것이 아니라, 여러 페이지로 나누어서 정보를 입력받는 방식이 많이 사용되고 있어요.
한번에 많은 정보를 입력받는 것은 유저에게 피로감을 유발하고, 유저가 입력한 정보를 한번에 확인하기 어렵기 때문이죠.

그러나 페이지가 많아지면 유저가 어떤 페이지를 거쳐서 어떤 정보를 입력했는지 파악하기 어려워지고, 유지보수가 어려워지는 문제가 있어요.

저는 현재 진행중인 북크북크 프로젝트에서 새로운 북클럽 생성 시, 여러 페이지로 나누어서 단계별로 정보를 입력받는 방식을 적용하고자 계획하고 있는데요,
이러한 문제를 어떻게 해결할 수 있을지 고민하다가, 토스에서 발표한 "토스 SLASH 퍼널: 쏟아지는 페이지 한 방에 관리하기"라는 영상을 보게 되었어요.

![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/d4e9aace-7c19-41d0-863a-44f7dead71dc)

# 퍼널 : 쏟아지는 페이지 한 방에 관리하기

<iframe
  width="100%"
  height="350"
  src="https://www.youtube.com/embed/NwLWX2RNVcw?si=eemsN07yXxzAbbsO"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

프론트엔드 패턴 중 여러 페이지들을 통해 상태를 수집하고, 최종적으로 결과 페이지를 보여주는 일반적인 설문조사 패턴을 응집도, 추상화, 시각화 측면에서 개선한 패턴을 소개하고 있어요.

여러 페이지에 걸쳐서 상태를 수집해야 하는 디자인 요구사항이 주어졌을 때 가장 기본적인 설계 방식은 다음과 같아요.

![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/80d78a60-ae84-4084-9ddf-c86f0f332428)
![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/606f69b8-2ddb-4d33-8f0d-6085d429f1db)

각 페이지에서 다음 단계에 해당하는 페이지로 라우팅을 통해 이동하는 방식이죠.
최종 페이지에서는 이전 페이지들에서 수집한 상태들을 통합해서 API를 호출하고, 결과를 보여주게 됩니다.
모든 페이지의 상위에 전역 상태를 두고, 각 페이지에서 필요한 상태를 전역 상태에서 수집하는 방식으로 구현하고 있어요.

## 문제 인식

유지보수 측면에서 위의 구조는 다음과 같은 문제가 있어요.

**1. 페이지의 흐름이 흩어져 있다.**

모든 페이지 파일을 넘나들며 페이지 이동 흐름을 파악해야 합니다.

**2. 한가지의 목적을 위한 상태가 흩어져 있다.**

각 페이지에서 전역 상태를 통해 필요한 상태를 각각 수집하고 있고, 상태를 사용하는 위치와 상태를 정의하는 위치가 달라요.
따라서 상태 흐름을 추적하기 어렵게 만듭니다.

이렇게 흩어져 있는 페이지 흐름과 상태를 한곳에 모아서 관리하기 위한 방법으로 퍼널 패턴을 소개하고 있어요.

## 응집도: 페이지의 흐름과 상태의 응집도 향상시키기

### 퍼널 패턴

퍼널은 유저가 서비스에 진입해서 최종 목표지점에 이르기까지 조금씩 이탈하는 과정을 단계별로 나눈 것을 말합니다.

![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/d46c686c-5492-40a9-8589-fc01e68aecd8)

1. 한 페이지에서 지역 상태로 form data, step 만들기
2. 여러 페이지 대신 한 흐름으로 컴포넌트 퍼널 구성하기
3. step 상태를 통해 컴포넌트를 조건부 렌더링하기
4. step 상태를 변경하면서 다음 단계의 컴포넌트를 렌더링하기

### 개선 포인트

1. UI 세부 사항은 하위 컴포넌트의 역할로 두되, step 상태는 상위에서 관리하여 UI 흐름을 한곳에서 관리할 수 있다.
2. API 호출에 필요한 form data는 한 페이지에서 관리할 수 있고, 어떤 상태가 어떤 UI에서 수집되는지 한눈에 파악할 수 있다.
3. 디자인 스펙 변경 시 유연한 대응이 가능하다.

## 추상화: useFunnel hook 라이브러리 만들기

퍼널 흐름과 관련된 로직을 추상화하여 다른 퍼널에서도 재사용할 수 있도록 개선할 수 있어요.

![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/6457f277-ddee-42ee-862f-71b8b6060651)
![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/3484f977-272b-413a-8f03-29a9b31420c4)

1. 퍼널 흐름과 관련된 로직 묶어내기
   2. step 지역 상태
   3. step 상태에 따라 컴포넌트를 조건부 렌더링하는 로직
4. 컴포넌트 내부에서 조건부 렌더링 추상화하기

### 개선 포인트

1. 퍼널 관련 기능을 사용처의 복잡도를 증가시키지 않고 가능을 추가할 수 있다.

- 브라우저 히스토리 관리 기능 추가하기
  ![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/6ebc70e1-7872-473b-bdfd-c85e134d692b)

## 시각화: 개발자 도구로 dx 향상시키기

물론 로직 상 이전보다 퍼널 흐름을 파악하기 쉬워졌지만, 여전히 개발 과정에서 퍼널의 흐름을 단번에 파악하기 어렵다는 문제가 있어요.
또한, 다음 단계 개발로 넘어가기 위해서는 url path에서 step을 변경해줘야 하는데, 이는 DX를 저해하는 요소가 됩니다.

![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/fba65b90-bac2-48ff-a905-f206d9138c7c)
퍼널의 흐름을 파악하고 Step을 넘나들어야 하는 DX를 개선하기 위해 흐름을 시각화한 개발자 도구로 디버깅을 편하게 만들 수 있어요.

다이어그램 라이브러리 mermaid를 사용하여 퍼널 흐름을 시각화한 개발자 도구를 만드는 방법을 소개하고 있어요.

![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/51cfc90c-c5eb-44f6-a7e3-5d265e21699f)
![image](https://github.com/bookkbookk/bookkbookk/assets/111998760/b967cae8-a08f-4063-8d2a-ba0a1971f562)

### 개선 포인트

1. 코드를 보지 않고도 퍼널 흐름이 맞는지 파악할 수 있다.
2. 개발 및 디버깅 속도가 향상된다.
3. 퍼널 흐름을 비개발자 동료에게 공유할 수 있다.

# hook 구현하고 실제 프로젝트에 적용하기

토스에서는 위의 패턴을 추상화하여 slash 라이브러리에서 useFunnel hook을 제공하고 있지만, next.js 프로젝트에서 사용할 수 있도록 useRouter를 기반으로 구현되어 있어 라이브러리를 사용할 수 없었어요.
react-router-dom에서 제공하는 `useSearchParams`를 활용하여 searchParams를 조작하여 퍼널을 구현할 수 있지만, 이번 프로젝트의 경우 페이지 전체가 아닌, 동일한 URL로 페이지 일부만 단계에 따라 변경되도록 디자인했기 때문에, path가 아닌 state 기반으로 구현하고자 했어요.

## useFunnel hook 구현

### 컴포넌트 분리

#### 타입 정의

- 해당 Step의 name으로 사용할 수 있는 문자열을 제한하기 위해 NonEmptyArray 타입을 정의했어요.

```ts
import { ReactElement, ReactNode } from 'react'

export type NonEmptyArray<T> = readonly [T, ...T[]]

export interface FunnelProps<Steps extends NonEmptyArray<string>> {
  steps: Steps
  step: Steps[number]
  children: Array<ReactElement<StepProps<Steps>>> | ReactElement<StepProps<Steps>>
}

export interface StepProps<Steps extends NonEmptyArray<string>> {
  name: Steps[number]
  children: ReactNode
}
```

#### Funnel, Step 컴포넌트

```tsx
import { Children, ReactElement, isValidElement } from 'react'
import { FunnelProps, NonEmptyArray, StepProps } from './type'

export const Step = <T extends NonEmptyArray<string>>({ children }: StepProps<T>) => {
  return <>{children}</>
}

export const Funnel = <Steps extends NonEmptyArray<string>>({
  steps,
  step,
  children,
}: FunnelProps<Steps>) => {
  const validChildren = Children.toArray(children)
    .filter(isValidElement)
    .filter((i) => steps.includes((i.props as Partial<StepProps<Steps>>).name ?? '')) as Array<
    ReactElement<StepProps<Steps>>
  >

  const targetStep = validChildren.find((child) => child.props.name === step)

  if (targetStep == null) {
    throw new Error(`${step} 스텝 컴포넌트를 찾지 못했습니다.`)
  }

  return <>{targetStep}</>
}
```

### hook 구현

```tsx
import { useMemo, useState } from 'react'
import { Funnel, Step } from './Funnel'
import { FunnelProps, NonEmptyArray, StepProps } from './type'

type RouteFunnelProps<Steps extends NonEmptyArray<string>> = Omit<
  FunnelProps<Steps>,
  'steps' | 'step'
>

type FunnelComponent<Steps extends NonEmptyArray<string>> = ((
  props: RouteFunnelProps<Steps>
) => JSX.Element) & {
  Step: (props: StepProps<Steps>) => JSX.Element
}

export const useFunnel = <Steps extends NonEmptyArray<string>>(
  steps: Steps,
  options?: { initialStep?: Steps[number] }
): readonly [FunnelComponent<Steps>, activeStepIndex: number, (step: Steps[number]) => void] => {
  const [step, setStep] = useState(options?.initialStep ?? steps[0])
  const activeStepIndex = steps.findIndex((s) => s === step)

  const FunnelComponent = useMemo(() => {
    return Object.assign(
      function RouteFunnel(props: RouteFunnelProps<Steps>) {
        return <Funnel<Steps> steps={steps} step={step} {...props} />
      },
      {
        Step,
      }
    )
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [step])

  return [FunnelComponent, activeStepIndex, setStep] as const
}
```

## 사용 예시

### 북클럽 생성 퍼널

```tsx
export default function NewBookClubFunnel() {
  const { profile, member, congratulation } = NEW_BOOK_CLUB_FUNNEL
  const funnelSteps = [profile, member, congratulation] as const
  const navigate = useNavigate()

  const [Funnel, activeStepIndex, setStep] = useFunnel(funnelSteps, {
    initialStep: profile,
  })

  return (
    <BoxContent>
      <Stepper activeStep={activeStepIndex} funnel={funnelSteps} />
      <Funnel>
        <Funnel.Step name={profile}>
          <BookClubProfile onPrev={() => navigate(-1)} onNext={() => setStep(member)} />
        </Funnel.Step>
        <Funnel.Step name={member}>
          <BookClubMember onPrev={() => setStep(profile)} onNext={() => setStep(congratulation)} />
        </Funnel.Step>
        <Funnel.Step name={congratulation}>
          <BookClubCongratulation />
        </Funnel.Step>
      </Funnel>
    </BoxContent>
  )
}
```

이외에도 책 추가 등 여러 단계에 걸쳐 상태를 수집해야 하는 상황에서 적용하여 사용하고 있습니다!

# 마치며

> 고심해서 만들어낸 해결책을 다른 사람들도 쓸 수 있게 공통 로직만 뽑아서 추상화하는 것

영상에서 위와 같이 언급을 하는 부분이 인상적이었는데요,
추상화를 통해 다른 퍼널에서도 재사용할 수 있도록 hook을 구현하고 개발자 도구도 만들어서 시각화해서 디버깅을 편하게 만드는 것을 보면서 현업에서는 UX와 DX를 이렇게 고민하고 점진적으로 코드를 개선하고, 생산성을 높이기 위한 고민들을 많이 하고 있구나를 느꼈어요.

실제 slash 라이브러리 useFunnel 코드를 참고하여 제 프로젝트에 맞게 직접 구현해본 과정이 재밌었고,
또 추상화를 통해 재사용성을 높이는 방법에 대한 관심도 높아진 것 같습니다 🎉

또한 실제로 useFunnel hook을 사용하여 흐름을 관리해서 응집도를 높일 수 있어서 개인적으로 만족스러웠기 때문에, 이후에 페이지 단위로 이동해야 하는 상황에서도 퍼널 패턴을 적용해보고 싶어요.
여기서 더 나아가서 최적화에 대한 고민을 많이 해보고, 더 나은 코드를 위해 노력해보겠습니다!

## 참고자료

- [토스 slash libraries - useFunnel](https://slash.page/libraries/react/use-funnel/readme.i18n/)