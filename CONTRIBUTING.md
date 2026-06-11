# Contributing — Ridy Agents

이 레포는 에이전트 프로토콜, 스펙, 기획서, 작업 큐를 관리합니다.

## 작업 흐름 (필수)

**모든 작업은 GitHub Organization Project에서 관리됩니다.**
📍 프로젝트 보드: https://github.com/orgs/project-ridy/projects/1

```
1. Project 이슈 할당 → 2. 이슈 자신 어사인 → 3. 이슈 In Progress → 4. 브랜치 생성 → 5. 문서 작성/수정 → 6. PR 생성 → 7. 이슈 Done
```

- **main 브랜치에 직접 커밋 금지** — 모든 변경은 PR로
- **Project 이슈 없이 작업 금지** — 모든 작업은 Organization Project의 이슈에서 시작
- **기획서 작성 시 docs 스펙 준수** — 기존 API/DB/디자인 스펙과 충돌 시 Orchestrator 승인 필요

## 브랜치 네이밍

| 타입 | 형식 | 예시 |
|---|---|---|
| 기능 | `feat/<이슈번호>-<설명>` | `feat/3-auth-plan` |
| 수정 | `fix/<이슈번호>-<설명>` | `fix/5-protocol-typo` |
| 문서 | `docs/<이슈번호>-<설명>` | `docs/7-add-spec` |

## 커밋 메시지

[Conventional Commits](https://www.conventionalcommits.org/) 준수:

```
docs(spec): Planner 에이전트 스펙 추가
docs(protocol): 작업 흐름에 기획서 단계 추가
feat(plan): 카풀 매칭 기능 기획서 작성
```

### scope 목록
| scope | 설명 |
|---|---|
| `spec` | 에이전트 스펙 변경 |
| `protocol` | 협업 프로토콜 변경 |
| `plan` | 기획서 추가/수정 |
| `tasks` | 작업 큐 변경 |

## PR 체크리스트

- [ ] Project 이슈가 할당되어 있는가?
- [ ] 기존 docs 스펙과 충돌하지 않는가?
- [ ] 기획서인 경우 `spec/AGENT_SPEC.md`의 포맷을 따랐는가?
- [ ] 다른 에이전트에 영향을 주는 변경인 경우 관련 문서도 업데이트했는가?
- [ ] 커밋 메시지가 Conventional Commits 규칙을 따르는가?
