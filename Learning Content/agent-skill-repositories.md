# 재사용 가능한 AI 에이전트 스킬 저장소

**조사 스냅샷: 2026-09-29 (UTC).** 별 수는 조사 당일 GitHub REST API의 `stargazers_count`를 기준으로 가까운 천 단위로 반올림한 근사치다. 각 항목의 별 숫자에는 해당 저장소의 GitHub API와 README를 개별 연결했다. GitHub stars는 관심·인지도의 대리 지표일 뿐 품질, 안전성, 유지관리 수준, 실제 사용량을 보증하지 않는다. 기능과 에이전트 호환성은 각 프로젝트 README 기준이며, 소유자 측 설명·지원 표기를 독립 검증한 것은 아니다.

## 스킬 콘텐츠 및 프레임워크

별 순으로 정렬했다. 이 범주는 설치 도구보다는 재사용 가능한 스킬 본문, 혹은 이를 묶은 개발 워크플로를 제공하는 프로젝트를 우선한다.

| 순위 | 프로젝트 / 별(근사) | 무엇을 제공하나 | 특징과 주의점 |
|---:|---|---|---|
| 1 | [obra/superpowers](https://github.com/obra/superpowers) · [API](https://api.github.com/repos/obra/superpowers) · **293k** | 스킬을 중심에 둔 소프트웨어 개발 방법론. 아이디어 구체화, 설계·계획, TDD, 하위 에이전트 기반 개발 등의 흐름을 연결한다. README는 Claude Code, Codex, Cursor, Gemini CLI, OpenCode 등 여러 호스트별 설치 경로를 안내한다. [README](https://github.com/obra/superpowers#readme) | 완성된 개발 프로세스 전체를 채택하려는 팀에 적합하다. 단순 참고용 개별 프롬프트 모음과 달리 에이전트의 작업 순서를 바꾸는 프레임워크이므로 기존 팀의 워크플로와 충돌하는지 먼저 검토할 것. |
| 2 | [mattpocock/skills](https://github.com/mattpocock/skills) · [API](https://api.github.com/repos/mattpocock/skills) · **271k** | AI Hero의 Matt Pocock이 실제 엔지니어링 작업에 사용한다고 소개하는 작고 조합 가능한 스킬 모음. 요구사항 인터뷰·도메인 모델링·명세/티켓화·TDD·디버깅·코드 리뷰 등의 스킬을 포함하며 Claude Code 플러그인 또는 `npx skills add mattpocock/skills`로 설치한다. [README](https://github.com/mattpocock/skills#readme) | 질문을 통한 의도 구체화와 표준 엔지니어링 피드백 루프를 선호하면 특히 잘 맞는다. AI Hero의 [스킬 페이지](https://www.aihero.dev/skills)는 이 GitHub 저장소를 원본으로 명시한다. 설치는 Claude Code 플러그인(관리형·읽기 전용)과 파일 복사 방식이 있으므로 둘 다 설치해 중복시키지 않도록 README를 따른다. |
| 3 | [anthropics/skills](https://github.com/anthropics/skills) · [API](https://api.github.com/repos/anthropics/skills) · **179k** | Anthropic의 Agent Skills 예제 및 스킬 구현 모음. 문서 처리, 창작·디자인, 개발/기술, 기업 커뮤니케이션 분야 등을 포함한다. Claude Code 플러그인으로 설치하고 Claude.ai 및 Claude API에서 사용하는 방법도 README에 안내한다. [README](https://github.com/anthropics/skills#readme) | 포맷과 패턴을 익히는 일차 출처로 가장 유용하다. README는 이 저장소를 Claude 스킬 시스템의 예시로 설명하고, 일부 문서 스킬은 오픈소스가 아니라 source-available이라고 명시한다. 제공된 구현의 결과가 제품 기능과 동일하다고 보장하지 않으며, 교육·시연 목적으로 테스트 후 사용하라고 경고한다. |
| 4 | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) · [API](https://api.github.com/repos/addyosmani/agent-skills) · **100k** | 명세화·계획·구현·검증·리뷰·배포 개발 전 과정을 위한 25개 스킬과 에이전트 페르소나를 제공한다. README가 Claude Code, Cursor, Antigravity, Gemini CLI, Windsurf, OpenCode, GitHub Copilot, Codex 등의 설치/사용법을 안내한다. [README](https://github.com/addyosmani/agent-skills#readme) | 소프트웨어 엔지니어링의 구체적인 품질 게이트와 생애주기 스킬을 찾을 때 유용하다. README는 개별 스킬 설치 시 공유 `references/` 디렉터리가 따라오지 않는 이식성 제약을 알린다. |
| 5 | [wshobson/agents](https://github.com/wshobson/agents) · [API](https://api.github.com/repos/wshobson/agents) · **40k** | 184개 스킬과 플러그인·에이전트·명령을 포함하는 에이전트 플러그인 마켓플레이스다. README는 Claude Code, Codex CLI, Cursor, OpenCode, Antigravity CLI, GitHub Copilot, Pi 대상 배포를 안내한다. [README](https://github.com/wshobson/agents#readme) | 하나의 스킬뿐 아니라 분야별 에이전트와 플러그인까지 함께 구성할 때 유용하며, 스킬만 `gh skill install` 또는 `npx skills add`로 설치할 수도 있다. 전체 마켓플레이스 설치는 skills-only와 다르므로 필요한 구성요소만 선택한다. README 수량은 프로젝트가 자체 집계한 것으로 변경될 수 있다. |
| 6 | [Vercel agent-skills](https://github.com/vercel-labs/agent-skills) · [API](https://api.github.com/repos/vercel-labs/agent-skills) · **32k** | Vercel 공식 스킬 모음. React/Next.js 성능 최적화, 웹 디자인·접근성 점검, 글쓰기 지침, React Native, Vercel 배포 등의 구체적 스킬을 제공하며 `npx skills add vercel-labs/agent-skills`로 설치한다. [README](https://github.com/vercel-labs/agent-skills#readme) | React/Vercel 프런트엔드 규칙을 적용하거나 UI·문서 리뷰를 자동화할 때 특히 적합하다. 범용 개발 생애주기 전체가 아니라 Vercel/React 생태계에 강점이 집중된다. |

## 카탈로그·디렉터리 (탐색 대상 찾기)

아래 두 저장소는 대규모 스킬을 발견하기 위한 색인 성격이 강하다. 저장소에 나열된 모든 항목이 해당 저장소 안에 완전한 스킬 파일로 들어 있는 것은 아니므로, 설치·사용 전 원본 프로젝트와 라이선스를 확인한다.

| 순위 | 프로젝트 / 별(근사) | 무엇을 제공하나 | 특징과 주의점 |
|---:|---|---|---|
| 1 | [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) · [API](https://api.github.com/repos/ComposioHQ/awesome-claude-skills) · **76k** | 문서·개발·데이터·업무·창작 등 분류별 스킬/플러그인 디렉터리이며, 일부는 저장소 내 파일 및 Composio 앱 연결 플러그인으로 제공된다. README는 Claude 외 Codex, Cursor, Gemini CLI, Antigravity 등도 언급한다. [README](https://github.com/ComposioHQ/awesome-claude-skills#readme) | 하나의 도메인보다 폭넓게 둘러볼 때 편리하다. 목록에는 외부 저장소로 연결되는 제3자 스킬도 포함되며, 함께 홍보되는 Composio 앱 연동은 외부 계정·인증/서비스 의존성을 추가한다. 항목별 출처·라이선스·실행 코드 및 유지 상태를 확인할 것. |
| 2 | [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) · [API](https://api.github.com/repos/VoltAgent/awesome-agent-skills) · **35k** | README가 1,497개 이상의 스킬과 Claude Code, Codex, Gemini CLI, Cursor, Copilot, OpenCode 등 호환을 표방하는 큐레이션 목록이다. Anthropic, Vercel, Google, Stripe 등의 공식 팀 및 커뮤니티 항목을 범주별로 모은다. [README](https://github.com/VoltAgent/awesome-agent-skills#readme) | 다양한 제공자와 스택의 공식 자료를 한 번에 탐색하기 좋다. 이 숫자는 README의 자체 표기이고, 별도 검증된 고유·동작 스킬 개수가 아니다. 다수 항목이 별도 저장소/링크이므로 이 목록의 큐레이션만으로 개별 스킬의 신뢰성이나 호환성을 보증하지 않는다. |

## 설치·탐색 도구 및 형식 명세 (스킬 컬렉션과 구분)

| 프로젝트 / 별(근사) | 역할 | 어떻게 활용하나 / 주의점 |
|---|---|---|
| [Vercel `skills` CLI](https://github.com/vercel-labs/skills) · [API](https://api.github.com/repos/vercel-labs/skills) · **33k** | 여러 저장소에서 스킬을 찾아 지원 에이전트에 설치·업데이트하는 오픈소스 CLI다. README는 Claude Code, Codex, Cursor, OpenCode 등을 포함한 75개 이상의 에이전트 지원을 표기한다. [README](https://github.com/vercel-labs/skills#readme) | `npx skills add OWNER/REPO --list`로 목록 확인, `--skill`/`--agent`로 선택 설치할 수 있다. **스킬 컬렉션 자체가 아니며**, 각 에이전트의 설치 경로를 처리하는 도구다. 설치하는 서드파티 스킬의 안전성은 별도 검토해야 한다. |
| [Agent Skills 표준 저장소](https://github.com/agentskills/agentskills) · [API](https://api.github.com/repos/agentskills/agentskills) · **26k** | `SKILL.md` 메타데이터·지침 및 선택적 스크립트/참조자료를 포함하는 이식 가능한 스킬 포맷의 명세와 문서다. [README](https://github.com/agentskills/agentskills#readme) | 컬렉션이 아니라 스킬 형식·호환성·작성법을 이해하기 위한 출처다. 실제 스킬 예제는 [Anthropic 저장소](https://github.com/anthropics/skills)를 참조한다. |

## 별 외에 확인할 사용 신호

[skills.sh 리더보드](https://www.skills.sh/)는 GitHub stars가 아니라 `skills` CLI의 익명 설치 텔레메트리를 인기 지표로 사용한다. [공식 설명](https://www.skills.sh/docs)은 설치된 스킬 ID만 집계하며 개인 정보나 사용 패턴은 추적하지 않는다고 밝힌다. 조사 당시 표시된 예시는 `mattpocock/skills`의 `grill-me` 약 **120만 설치**, `anthropics/skills`의 `frontend-design` 약 **93만**, `vercel-labs/agent-skills`의 `vercel-react-best-practices` 약 **75만**, `obra/superpowers`의 `brainstorming` 약 **38만**이다. 이는 CLI 설치 횟수 집계이지 고유 사용자 수나 품질 점수가 아니며, 저장소 star 수와는 별도 지표다.

## 추천 시작점

- **AI Hero 페이지와 같은 스킬 세트를 찾을 때:** [Matt Pocock의 skills](https://github.com/mattpocock/skills)가 해당 페이지가 가리키는 원본이다. 요구사항 명확화, 티켓/계획, TDD, 코드베이스 설계 스킬을 골라 사용한다.
- **개발 에이전트에 구조화된 일하는 방식을 붙이고 싶을 때:** [Superpowers](https://github.com/obra/superpowers). 팀의 프로세스를 많이 바꾸므로 도입 범위와 리뷰 지점을 맞춘다.
- **스킬 포맷과 모범 예제를 배우려 할 때:** [Anthropic Skills](https://github.com/anthropics/skills). 단, README가 source-available로 고지한 스킬의 라이선스는 별도로 확인한다.
- **여러 에이전트용 스킬·에이전트·플러그인을 함께 둘러볼 때:** [wshobson/agents](https://github.com/wshobson/agents); 필요한 스킬만 개별 설치할지 전체 플러그인을 설치할지 구분한다.
- **폭넓은 개발 생애주기 스킬을 선택할 때:** [Addy Osmani Agent Skills](https://github.com/addyosmani/agent-skills). 공유 references의 설치 제약을 고려한다.
- **React/Vercel 프런트엔드 작업에 좁혀 적용할 때:** [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills).
- **새로운 주제의 후보를 빠르게 찾을 때:** [Composio 목록](https://github.com/ComposioHQ/awesome-claude-skills)이나 [VoltAgent 목록](https://github.com/VoltAgent/awesome-agent-skills)에서 찾고, 원본 저장소에서 내용을 검토한 뒤 설치한다.
- **여러 에이전트로 설치를 자동화할 때:** 컬렉션으로 착각하지 말고 [Vercel Skills CLI](https://github.com/vercel-labs/skills)를 별도로 고려한다.

## 범위 및 평가 기준

- 주요 선별 기준은 공개 GitHub 별 수와 인지도뿐 아니라 저장소 README로 확인되는 재사용 스킬 콘텐츠, 활동 흔적, 출처/지원 설명이다. 개인 저장소의 변동성 높은 긴 꼬리 목록을 늘어놓기보다 공식 프로젝트·유지되는 프레임워크·대표적인 카탈로그 위주로 추렸다.
- 순위와 근사 별 수는 **2026-09-29 조사 시점의 GitHub API 응답**에 기반하며, 시간이 지나면 변한다. 숫자 링크를 누르면 각 repo API에서 최신 `stargazers_count`를 확인할 수 있다.
- 별은 품질 점수가 아니다. 스킬을 실행하기 전 `SKILL.md` 및 스크립트의 지시·외부 호출·파일 변경을 검토하고, 항목별 라이선스·필요 권한·호스트 호환성을 확인한다. README가 지원한다고 주장하는 에이전트 버전/동작도 실제 환경에서 확인한다.
- 비교를 위해 확인한 [OpenAI `openai/skills`](https://github.com/openai/skills)의 README는 저장소가 **deprecated**라고 명시하고, 현재 Codex 플러그인 예제를 [OpenAI Plugins](https://github.com/openai/plugins)로 안내한다. 따라서 예전 유명 Codex 스킬 저장소라는 이유만으로 최신 추천 목록에 올리지는 않았다.
