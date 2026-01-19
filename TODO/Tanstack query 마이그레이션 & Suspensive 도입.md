https://tkdodo.eu/blog/the-query-options-api

# 1. 문제 상황

react-query
옵저버 -> 성능 이슈 (메모리,)

useGoalItemTree 

사용 패턴 hook 호출

단일키에 option이 머지

observer를 하나로 두고, option 이 머지되어서 실행이 안될 수도 있음

revalidateInterval
보장되지 않는다.

observer 최소화

hook
내부에 상태 두기
modal hook

1. https://suspensive.org/ko/docs/react-query/SuspenseQueries
	1. suspensible