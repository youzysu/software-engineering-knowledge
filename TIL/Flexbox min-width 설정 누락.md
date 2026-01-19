## 문제의식 (Why this matters)

- Flex 컨테이너 내부에서 긴 텍스트가 있는 자식 요소에 `truncate`(text-overflow: ellipsis)를 적용해도 의도대로 잘리지 않는 경우가 자주 발생합니다.
- 원인은 flex item의 기본 `min-width: auto` 동작으로, 자식 컨텐츠가 깨지지 않도록 최소 너비를 보장하려고 하기 때문입니다.
- 이로 인해 레이아웃 깨짐, overflow, 스크롤 발생, 또는 UI가 비일관적으로 보이는 문제가 생깁니다.
- 팀에서 반복적으로 `min-w-0`을 추가하는 일이 발생하며, 빠뜨리면 버그가 생깁니다. 따라서 일관된 패턴 또는 자동화가 필요합니다.

---

## 기술적 배경 (간단히)

- Flexbox 알고리즘에서 아이템이 얼마나 줄어들 수 있는지는 `min-width`에 의해 제한됩니다.
- 기본값 `min-width: auto`는 컨텐츠에 맞춰 최소 너비를 보장하므로, 부모가 좁아져도 줄어들지 않습니다.
- `min-width: 0`을 적용하면 flex item이 0까지 줄어들 수 있으므로 `overflow: hidden`이나 `text-overflow: ellipsis`가 정상 동작합니다.

---

## 해결 전략 요약

flex 기반 레이아웃에서는 `min-w-0`을 부모(flex item) 수준에서 적용하는 것이 가장 안전하고 일관됨.

- min-w-0 vs max-w-full
    - `min-w-0`은 부모(flex item)의 최소 너비 제한을 제거하여 근본적인 문제 해결
    - `max-w-full`은 자식의 최대 너비만 제한하므로 부모의 `min-width: auto`가 남아있다면 문제를 해결하지 못하는 경우가 있음
- 부모에 `overflow-hidden` 적용
    - 일부 상황에서 동작하지만, 의도치 않게 다른 overflow 컨텐츠를 숨길 수 있음 → 부작용 위험
- Grid 사용 전환

---

## 제안: `flexbox` 커스텀 클래스 추가

- 커스텀 유틸리티/컴포넌트 제공으로 사용자 실수 최소화
- 목적: 팀원이 단순히 `flexbox` 클래스를 사용하면 항상 안전한 기본값(=flex + min-w-0)을 얻도록 함
- 장점: 기존 `flex` 사용 습관을 크게 바꾸지 않고도 넛지 가능, 코드 가독성 유지

사용 예:

```html
<!-- 권장 -->
<div class="flexbox">
  <div class="truncate">아주 긴 텍스트...</div>
</div>

<!-- 기존 방식 (실수 가능성) -->
<div class="flex">
  <div class="truncate">아주 긴 텍스트...</div>
</div>

```

---

## tailwind.css에 간단한 커스텀 클래스 추가하기

[https://github.com/lemonbase-tech/lemonbase/commit/06f20cdd25e1210443fbbe5733d7f0267e9b2c6e](https://github.com/lemonbase-tech/lemonbase/commit/06f20cdd25e1210443fbbe5733d7f0267e9b2c6e)

- 과연.. 이 커스텀 클래스의 존재를 잊지 않고 쓰는 것과 min-w-0 추가를 습관화하는 것 중에 무엇이 나을까…