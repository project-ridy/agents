# AGENTS.md — Ridy Agents

이 파일은 이 레포에서 작업하는 에이전트가 반드시 따라야 할 규칙을 정의합니다.

## 에이전트 역할

이 레포는 Ridy 프로젝트의 **모든 에이전트 관련 문서**를 관리합니다.

- **Orchestrator**: 전체 관리, 작업 분배, 품질 관리
- **Planner**: 사용자 요청 분석, 구현 계획서 작성 (`docs/planning/implementation/`, 에이전트 자체 변경은 `agents/plans/`)
- **Frontend Developer**: docs의 설계 문서를 기반으로 프론트엔드 구현
- **Backend Developer**: docs의 설계 문서를 기반으로 백엔드 구현
- **Designer**: docs의 디자인 문서를 기반으로 UI/UX 구현

## Hermes 프로필 매핑

실제 Hermes 프로필 모델 설정은 `config/HERMES_MODELS.yaml`을 기준으로 관리합니다.

| 에이전트 | Hermes 프로필 | 기본 모델 | Provider | Base URL | Max turns |
|---|---|---|---|---|---|
| Orchestrator | `orchestrator` | `gpt-5.5` | `openai-codex` | `https://chatgpt.com/backend-api/codex` | `150` |
| Planner | `planner` | `gpt-5.5` | `openai-codex` | `https://chatgpt.com/backend-api/codex` | `150` |
| Frontend Developer | `frontend` | `kimi-k2.5` | `byteplus-ark` | — | `150` |
| Backend Developer | `backend` | `glm-5.1` | `byteplus-ark` | — | `150` |
| Designer | `designer` | `kimi-k2.5` | `byteplus-ark` | — | `150` |

## 필수 규칙

### 이슈/PR 템플릿 준수 (절대 불변)

모든 에이전트는 이슈와 PR을 생성할 때 **해당 레포의 `.github/` 템플릿에 정의된 형식과 필드를 반드시 채워야 한다.** 템플릿 없이 자유 형식으로 작성하는 것은 금지한다.

#### 이슈 템플릿 매핑

| 레포 | 템플릿 | 사용 시점 |
|---|---|---|
| **backend** | `feature.md` | 기능 개발 이슈 |
| | `bug.md` | 버그 수정 이슈 |
| | `test.md` | 테스트 작성/개선 이슈 |
| **frontend** | `feature.md` | 기능 개발 이슈 |
| | `bug.md` | 버그 수정 이슈 |
| | `test.md` | 테스트 작성/개선 이슈 |
| **docs** | `documentation.md` | 기획, API 스펙, 디자인, 아키텍처 등 문서 작업 |
| **agents** | `documentation.md` | 에이전트 프로토콜/스펙/기획서 관련 작업 |

#### PR 템플릿 매핑

| 레포 | PR 템플릿 핵심 필드 |
|---|---|
| **backend** | 변경 사항, 관련 이슈, 작업 유형, TDD 체크리스트, 테스트 결과, API 변경 사항, DB 변경 사항, 체크리스트 |
| **frontend** | 변경 사항, 관련 이슈, 작업 유형, TDD 체크리스트, 테스트 결과, 스크린샷(UI 변경 시), 체크리스트 |
| **docs** | 변경 사항, 관련 이슈, 변경된 문서 카테고리, 체크리스트(교차 참조, 프론트/백엔드 영향, agents 레포 반영) |
| **agents** | 변경 내용, 관련 이슈, 체크리스트(Project 이슈, docs 스펙 충돌, 기획서 포맷, 타 에이전트 영향, 커밋 규칙) |

#### 이슈 생성 규칙

1. `gh issue create` 시 `--template` 플래그로 해당 레포의 템플릿을 명시적으로 선택
2. 템플릿에 정의된 **모든 섹션을 빈칸 없이 작성** — 특히:
   - **기능 이슈**: 기능 설명, 관련 문서, 작업 범위, TDD 사이클, 완료 조건
   - **버그 이슈**: 버그 설명, 재현 방법, 기대 동작, 실제 동작, TDD 사이클
   - **테스트 이슈**: 테스트 대상, 관련 기능 이슈, 테스트 종류, 테스트 케이스 표, 완료 조건
   - **문서 이슈**: 작업 내용, 문서 카테고리(docs) / 관련 에이전트(agents), 완료 조건
3. 이슈 제목 접두어 준수: `[FEAT]`, `[FIX]`, `[TEST]`, `[DOCS]`

#### PR 생성 규칙

1. PR 본문은 해당 레포의 `PULL_REQUEST_TEMPLATE.md` 형식을 그대로 사용
2. `gh pr create` 시 `--body-file`로 템플릿 내용을 채워서 전달
3. 템플릿에 정의된 **모든 체크리스트 항목에 대해 실제 결과를 기록** — 빈 체크박스로 두지 않음
4. 특히 중요한 공통 필드:
   - **관련 이슈**: `Closes #<번호>` 형식으로 반드시 연결
   - **TDD 체크리스트**: Red → Green → Refactor → Verify 각 단계 결과 기록 (backend/frontend)
   - **테스트 결과**: `npm run test` 실행 결과를 코드 블록으로 첨부 (backend/frontend)
   - **변경된 문서 카테고리**: 해당하는 항목 체크 (docs)
   - **API/DB 변경 사항**: 변경 여부와 영향 범위 명시 (backend)

### 작업 흐름
1. 모든 작업은 **GitHub Organization Project**의 이슈에서 시작 — https://github.com/orgs/project-ridy/projects/1
2. 작업 시작 전 **이슈에 자신을 어사인** (`gh issue edit <번호> --add-assignee @me`)
3. 이슈를 `In Progress`로 변경 후 작업 시작
4. 브랜치 생성: `agents/<이슈번호>-<설명>`
5. PR 생성 후 머지, 이슈를 `Done`으로 변경
6. main 브랜치에 직접 커밋 금지

### 설계 문서 참조
- 이 레포의 에이전트 프로토콜/스펙은 **docs 레포**의 설계 문서를 기준으로 작성
- docs 레포: https://github.com/project-ridy/docs
- 작업 흐름 변경 시 `protocol/AGENT_PROTOCOL.md`의 이슈 매핑 업데이트
- 에이전트 역할 변경 시 `spec/AGENT_SPEC.md`의 의존성/역할 섹션 업데이트

### 기획서 작성 (Planner)
- 사용자 요청이 들어오면 Planner가 표준 구현 계획서를 `docs/planning/implementation/`에 작성
- 에이전트 프로토콜/스펙/작업 큐 자체 변경 계획만 `agents/plans/`에 작성
- 기획서는 `spec/AGENT_SPEC.md`에 정의된 포맷 준수
- 기획서에는 구현/테스트 케이스 등록표를 반드시 포함하고, 모든 기능 코드 항목에 케이스 ID와 대응 테스트를 연결
- 기존 docs 스펙과 충돌 시 Orchestrator에 확인
- 기획서 머지 전 Orchestrator 리뷰 필수

### 서브 에이전트 작업 분배 (Orchestrator)
- 사용자 요청 → Planner에게 기획 지시 → 기획서 승인 → Frontend Developer/Backend Developer/Designer에 할당
- 프론트엔드 이슈는 `frontend`, 백엔드 이슈는 `backend` 프로필에 할당
- 할당 시 이슈 번호, 기획서 경로, docs 문서 경로, 완료 조건 명확히 전달
- BLOCKED 이슈는 Orchestrator가 docs 업데이트로 해결

### 문서 작성 규칙
- H1(`#`)은 파일당 1개
- 코드 블록에 언어 명시 (```typescript, ```json)
- 표는 가독성 우선
- 커밋 메시지: `docs(<scope>): <subject>` (scope: spec, protocol, plan, tasks)

### 사용 스킬
- Orchestrator: `technical-writing`, `api-design-reviewer`, `database-designer`
- Planner: `technical-writing`, `api-design-reviewer`, `database-designer`, `product-manager`
- Frontend Developer: `tdd`, `react-expert`, `nextjs-expert`, `typescript-expert`, `code-review-expert`
- Backend Developer: `tdd`, `typescript-expert`, `database-optimizer`, `api-design-reviewer`, `code-review-expert`

### 참조 문서
- 에이전트 프로토콜: `protocol/AGENT_PROTOCOL.md`
- 에이전트 스펙: `spec/AGENT_SPEC.md`
- 개발 기획서: `docs/planning/implementation/`
- 에이전트 자체 변경 계획: `agents/plans/`
- 프론트엔드 작업 큐: `tasks/FRONTEND_DEVELOPER_TASKS.md`
- 백엔드 작업 큐: `tasks/BACKEND_DEVELOPER_TASKS.md`
- 디자인 작업 큐: `tasks/DESIGNER_TASKS.md`
