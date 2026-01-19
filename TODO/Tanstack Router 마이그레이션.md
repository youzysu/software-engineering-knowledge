- [ ] referrer 관리 방식 (useNavigation)
- [ ] URL path type-safe >> Tanstack

# 1. 도입 배경
- 센트리 대응 관련 간단 qs 라이브러리 사용 시 유의점: `qs` 라이브러리의 `arrayLimit` 기본값이 20으로, 배열 인덱스가 20을 초과하면 배열 대신 객체로 파싱됩니다.
    - 관련 라이브러리 변경은 없었고 그동안 다행히(?) 테이블 필터값 유지를 위해 21개 이상 선택해서 저장한 적이 없었던 것으로 보입니다.
- [논의?] 고민중인 장기적 개선 방향
    - 현재 URL search params 관리가 타입 불안정하여 런타임에서야 에러를 발견할 수 있는 구조
        
    - TanStack Router는 search params 타입 추론을 내장 지원 (참고: [https://tanstack.com/router/latest/docs/framework/react/guide/search-params#validating-search-params](https://tanstack.com/router/latest/docs/framework/react/guide/search-params#validating-search-params))
        
    - 추가로, `location.state.referrer` 패턴도 타입 힌트가 없어 발견하기 어렵고 redirectUrl 등 커스텀 구현이 파편화되는 문제가 있는데, TanStack Router 기반으로 표준화할 수 있는 장점도!
        
        ### 참고: `referrer` 패턴의 현재 한계
        
        페이지 이동 후 "돌아가기" 기능 구현 시 `location.state.referrer` 패턴이 있지만, 잘 활용되지 못하고 있습니다.
        
        ### 현재 문제점
        
        1. **타입에서 알려주지 않음**: `location.state`가 `unknown`이라 referrer 존재를 모르고 `redirectUrl` 같은 커스텀 파라미터를 만들게 됨
            
        2. **패턴 파편화**: 각자 다른 방식으로 구현 (`redirectUrl`, `returnPath`, `backTo` 등)
            
        3. **발견하기 어려움**: 컴포넌트가 있어도 타입 힌트가 없어 존재를 모름
            
            참고: [WorkspaceAppBarClose.tsx](https://github.com/lemonbase-tech/lemonbase/blob/master/app/src/lds/components/WorkspaceAppBar/WorkspaceAppBarClose.tsx) - 현재 referrer 패턴 구현체
            
            ```tsx
            // 현재: 타입 힌트 없음
            const location = useLocation();
            location.state?.referrer; // ❌ 타입이 unknown, 자동완성 안됨
            
            // 그래서 각자 만들게 됨
            const { redirectUrl } = useSearchParams(); // 커스텀 구현 A
            const { returnPath } = location.state;     // 커스텀 구현 B
            ```
            
        
        ### TanStack Router라면?
        
        ```tsx
        // 라우트 정의 시 state 타입도 지정 가능
        const editRoute = createFileRoute('/goals/$goalId/edit')({
          validateSearch: z.object({ ... }),
          // context나 loader에서 referrer 패턴 표준화
        });
        
        // 사용 시 타입 추론
        const navigate = useNavigate();
        navigate({
          to: '/goals/$goalId/edit',
          params: { goalId },
          state: { referrer: location }, // 타입 체크됨
        });
        ```
        
        **결론**: TanStack Router 마이그레이션 시 search params뿐 아니라 `location.state` 패턴도 타입 안전하게 표준화할 수 있어, 중복 구현과 파편화를 방지할 수 있는 장점이..!
        
    - 현재로서는 Tanstack Router 문서만 간단히 훑어본 상황이고, URL 패턴 통합([appRoutePaths.ts](https://github.com/lemonbase-tech/lemonbase/blob/b176aa5686f79984470ce0c8ced9db01ba00d340/app/src/routes/paths/appRoutePaths.ts)) 또한 필요한 상황이기도 해서 본격적으로 검토해볼 예정입니다. 다만 마이그레이션 비용이 크니 어떻게 안전하게 할 수 있을지 제대로 고민 해보겠습니다.