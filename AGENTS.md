# AGENTS.md — Ridy Agents

이 파일은 이 레포에서 작업하는 에이전트가 반드시 따라야 할 규칙을 정의합니다.

## 에이전트 역할

이 레포는 Ridy 프로젝트의 **모든 에이전트 관련 문서**를 관리합니다.

- **Orchestrator**: 전체 관리, 작업 분배, 품질 관리
- **Planner**: 사용자 요청 분석, 개발 기획서 작성 (plans/)
- **Developer**: docs의 설계 문서를 기반으로 구현
- **Designer**: docs의 디자인 문서를 기반으로 UI/UX 구현

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
- API 스펙 변경 시 `AGENT_PROTOCOL.md`의 이슈 매핑 업데이트
- DB 스키마 변경 시 `AGENT_SPEC.md`의 의존성 섹션 업데이트

### 기획서 작성 (Planner)
- 사용자 요청이 들어오면 Planner가 `plans/`에 기획서 작성
- 기획서는 `AGENT_SPEC.md`에 정의된 포맷 준수
- 기존 docs 스펙과 충돌 시 Orchestrator에 확인
- 기획서 머지 전 Orchestrator 리뷰 필수

### 서브 에이전트 작업 분배 (Orchestrator)
- 사용자 요청 → Planner에게 기획 지시 → 기획서 승인 → Developer/Designer에 할당
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

### 참조 문서
- 에이전트 프로토콜: `protocol/AGENT_PROTOCOL.md`
- 에이전트 스펙: `spec/AGENT_SPEC.md`
- 개발 기획서: `plans/`
- 개발 작업 큐: `tasks/DEVELOPER_TASKS.md`
- 디자인 작업 큐: `tasks/DESIGNER_TASKS.md`
