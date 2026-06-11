# AGENTS.md — Ridy Agents

이 파일은 이 레포에서 작업하는 에이전트가 반드시 따라야 할 규칙을 정의합니다.

## 에이전트 역할

이 레포는 Ridy 프로젝트의 **모든 에이전트 관련 문서**를 관리합니다.

- **Orchestrator**: 전체 관리, 작업 분배, 품질 관리
- **Planner**: 사용자 요청 분석, 개발 기획서 작성 (plans/)
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
- 사용자 요청이 들어오면 Planner가 `plans/`에 기획서 작성
- 기획서는 `spec/AGENT_SPEC.md`에 정의된 포맷 준수
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
- 개발 기획서: `plans/`
- 프론트엔드 작업 큐: `tasks/FRONTEND_DEVELOPER_TASKS.md`
- 백엔드 작업 큐: `tasks/BACKEND_DEVELOPER_TASKS.md`
- 디자인 작업 큐: `tasks/DESIGNER_TASKS.md`
