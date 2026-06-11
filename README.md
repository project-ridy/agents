# Ridy Agents 🤖

**함께 타는 길, Ridy** — 에이전트 프로토콜, 스펙, 기획서, 작업 큐를 관리하는 레포

## 📂 디렉토리 구조

```
agents/
├── AGENTS.md                        # 에이전트 필수 규칙
├── README.md                        # 이 파일
├── CONTRIBUTING.md                  # 기여 가이드
├── rules.md                         # 문서 작성 규칙
├── config/                          # 에이전트 실행 설정값
│   └── HERMES_MODELS.yaml           # Hermes 프로필별 모델/provider 설정
├── spec/                            # 에이전트 스펙
│   └── AGENT_SPEC.md                # 5개 에이전트 역할/능력/제약
├── protocol/                        # 협업 프로토콜
│   └── AGENT_PROTOCOL.md            # 작업 흐름, 규칙, 의사소통
├── plans/                           # 개발 기획서 (Planner 산출물)
│   └── README.md                    # 기획서 포맷 & 가이드
└── tasks/                           # 작업 큐
    ├── FRONTEND_DEVELOPER_TASKS.md  # 프론트엔드 개발 작업 큐
    ├── BACKEND_DEVELOPER_TASKS.md   # 백엔드 개발 작업 큐
    └── DESIGNER_TASKS.md            # 디자인 에이전트 작업 큐
```

## 🤖 에이전트

| 에이전트 | Hermes 프로필 | 역할 | 담당 레포 | 상세 |
|---|---|---|---|---|
| **Orchestrator** | `orchestrator` | 전체 관리, 작업 분배, 품질 관리 | docs | [AGENT_SPEC.md](spec/AGENT_SPEC.md) |
| **Planner** | `planner` | 사용자 요청 분석, 개발 기획서 작성 | agents (plans/) | [AGENT_SPEC.md](spec/AGENT_SPEC.md) |
| **Frontend Developer** | `frontend` | 프론트엔드 기능 구현 | frontend | [AGENT_SPEC.md](spec/AGENT_SPEC.md) |
| **Backend Developer** | `backend` | 백엔드 API/DB 구현 | backend | [AGENT_SPEC.md](spec/AGENT_SPEC.md) |
| **Designer** | `designer` | UI/UX 디자인, 접근성 | frontend | [AGENT_SPEC.md](spec/AGENT_SPEC.md) |

## 🔗 관련 레포

| 레포 | 설명 |
|---|---|
| [project-ridy/docs](https://github.com/project-ridy/docs) | 기획, API 스펙, 디자인, 아키텍처 (설계 문서) |
| [project-ridy/frontend](https://github.com/project-ridy/frontend) | Next.js 15 + shadcn/ui 프론트엔드 |
| [project-ridy/backend](https://github.com/project-ridy/backend) | NestJS 11 + Prisma 6 백엔드 |
| [project-ridy/agents](https://github.com/project-ridy/agents) | 에이전트 프로토콜, 스펙, 기획서, 작업 큐 (이 레포) |

## 📋 GitHub Project

**프로젝트 보드**: https://github.com/orgs/project-ridy/projects/1
