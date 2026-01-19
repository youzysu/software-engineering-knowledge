#### 1. 문제 상황
테이블 필터에서 21개 이상의 값을 선택한 후 페이지를 새로고침하면 `TypeError: Ot?.map is not a function` 에러가 발생합니다.
에러 발생 위치: `antd/es/table/hooks/useFilter/index.js` (Ant Design 내부)

#### 2. 문제 원인
`qs` 라이브러리의 `arrayLimit` 기본값이 20이기 때문입니다. URL 쿼리스트링에서 배열 인덱스가 20을 초과하면 `qs.parse`가 배열 대신 객체로 파싱합니다:

```typescript
// 인덱스 0-20: 배열로 파싱
queryDeserializer('items[0]=a&items[20]=b')
// → { items: ['a', ..., 'b'] }

// 인덱스 21 이상 포함: 객체로 파싱
queryDeserializer('items[0]=a&items[21]=b')
// → { items: { 0: 'a', 21: 'b' } }
```

Ant Design Table 내부에서 `filteredValues.map(String)`을 호출하는데, 객체에는 `.map()` 메서드가 없어 TypeError가 발생합니다.

#### 3. qs 라이브러리 공식 문서 (참고: https://github.com/ljharb/qs#parsing-arrays)

> **qs will also limit specifying indices in an array to a maximum index of 20.** Any array members with an index of greater than 20 will instead be converted to an object with the index as the key. This is needed to handle cases when someone sent, for example, `a[999999999]` and it will take significant time to iterate over this huge array.

### 5. 재현 [링크](https://master.lemonbase.io/app/admin/reviews/analysis/individual-report/eee31064-210e-4e66-83e1-eaa417fbfe7b/results?individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bpagination%5D%5Bcurrent%5D=1&individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bpagination%5D%5BpageSize%5D=50&individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bpagination%5D%5Bposition%5D%5B0%5D=bottomCenter&individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bpagination%5D%5BhideOnSinglePage%5D=true&individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bpagination%5D%5BshowSizeChanger%5D=false&individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bfilters%5D%5Brole%5D%5B0%5D=apple&individual_report_result_list_eee31064-210e-4e66-83e1-eaa417fbfe7b%5Bfilters%5D%5Brole%5D%5B25%5D=%ED%8C%80%EC%9E%A5)

- role[25]=팀장으로 인덱스 25가 arrayLimit(20)을 초과하여 에러 발생
- (참고) 동작 확인용 테스트 코드

```typescript
describe('arrayLimit 동작 확인', () => {
  it('인덱스가 20 이하인 배열은 배열로 파싱된다', () => {
    const query = Array.from({ length: 21 }, (_, i) => `items[${i}]=value${i}`).join('&');
    const result = queryDeserializer(query);

    expect(Array.isArray(result.items)).toBe(true);
    expect(result.items).toHaveLength(21);
  });

  it('인덱스가 20을 초과하면 배열이 아닌 객체로 파싱된다 (qs arrayLimit 기본값 20)', () => {
    const query = 'items[0]=a&items[21]=b';
    const result = queryDeserializer(query);

    expect(Array.isArray(result.items)).toBe(false);
    expect(result.items).toEqual({ 0: 'a', 21: 'b' });
  });

  it('테이블 필터에서 21개 이상의 필터값이 선택되면 객체로 파싱되어 문제가 발생할 수 있다', () => {
    const roles = Array.from({ length: 25 }, (_, i) => `Role${i}`);
    const query = roles.map((role, i) => `filters[role][${i}]=${encodeURIComponent(role)}`).join('&');
    const result = queryDeserializer(query);

    expect(Array.isArray((result.filters as Record<string, unknown>).role)).toBe(false);
  });
});
```

### 6. 해결 방법

- 필터 선택 개수는 동적으로 변하다보니, arrayLimit 옵션 조정할 수 없고, `usePersistTable.ts`에서 `qs`가 객체로 파싱한 경우 `Object.values()`로 배열로 변환하도록 수정

```typescript
const normalizedFilterValue =
  filterValue && typeof filterValue === 'object' && !Array.isArray(filterValue)
    ? (Object.values(filterValue) as FilterValue)
    : filterValue;
```