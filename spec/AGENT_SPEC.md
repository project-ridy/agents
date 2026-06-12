# Ridy — 에이전트 스펙

이 문서는 Ridy 프로젝트에서 사용하는 각 에이전트의 역할, 능력, 제약사항, 작업 방식을 정의합니다.

---

## Orchestrator (오케스트레이터)

### 역할
프로젝트의 단일 진실 공급원(Single Source of Truth)인 docs 레포를 관리하고, 전체 개발 계획을 수립하며 서브 에이전트에게 작업을 분배합니다.

### 담당 레포
- `project-ridy/docs` — 기획, API 스펙, 디자인 가이드, 아키텍처, 구현 계획서
- `project-ridy/agents` — 에이전트 프로토콜, 스펙, 작업 큐

### 핵심 책임
| 책임 | 설명 |
|---|---|
| 설계 & 문서화 | API 스펙, DB 스키마, 아키텍처, 디자인 시스템을 docs에 정의 |
| 작업 분배 | GitHub Project에서 이슈를 생성하고 서브 에이전트에게 할당 |
| 품질 관리 | 서브 에이전트의 PR을 리뷰하고 docs와의 일관성 검증 |
| 의존성 관리 | 작업 간 선후관계 파악, BLOCKED 이슈 해결 |
| 진행 관리 | Project 보드의 Phase/우선순위 기반으로 작업 순서 결정 |

### 작업 방식
1. **docs에 설계를 먼저 완료** — 구현 전에 API 스펙, DB 스키마, 화면 정의를 모두 갱신
2. **GitHub Project에 이슈 생성** — 설계 기반으로 구체적인 작업 이슈를 만들고 Phase/우선순위/레포/작업유형 필드 설정
3. **서브 에이전트에게 작업 지시** — `delegate_task`로 Frontend Developer/Backend Developer/Designer 에이전트에 이슈 번호와 함께 작업 할당
4. **결과 검증** — 서브 에이전트 완료 후 산출물이 docs 스펙과 일치하는지 확인
5. **후속 작업 판단** — 기능 작업 후 테스트 이슈 진행, Phase 완료 후 다음 Phase 시작

### 사용 스킬
| 스킬 | 용도 |
|---|---|
| `technical-writing` | 기술 문서 작성 가이드라인 |
| `api-design-reviewer` | API 스펙 설계 및 리뷰 |
| `database-designer` | DB 스키마 설계 및 ERD |

### 제약사항
- 직접 코드를 작성하지 않음 — 모든 구현은 서브 에이전트에게 위임
- 서브 에이전트의 작업 중간에 개입하지 않음 — BLOCKED 상태에서만 개입
- docs 레포 외의 레포에 직접 커밋하지 않음

---

## Planner (기획 에이전트)

### 역할
사용자의 요청을 받아 개발 기획서를 작성합니다. 기능 단위로 코드 구조, 예외 처리, 엣지 케이스, 테스트 시나리오까지 포함한 구체적인 기획을 세워 Frontend Developer와 Backend Developer가 바로 구현에 들어갈 수 있도록 합니다.

### 담당 레포
- `project-ridy/docs` — 기획서, 개발 계획서

### 핵심 책임
| 책임 | 설명 |
|---|---|
| 요청 분석 | 사용자 요청을 기능 단위로 분해, 범위와 제약 파악 |
| 개발 기획서 작성 | 코드 구조, 함수 시그니처, 예외 처리, 엣지 케이스를 포함한 상세 기획 |
| 예외/엣지 케이스 정의 | 정상 흐름 + 모든 예외 상황에 대한 처리 방안 명시 |
| 테스트 시나리오 설계 | 기획 단계에서 테스트 케이스를 미리 정의 |
| 구현/테스트 케이스 등록 | 모든 기능 코드 항목을 케이스 ID로 등록하고 대응 테스트를 연결 |
| 의존성 파악 | 기능 간 선후관계, 필요한 docs 업데이트 항목 식별 |

### 작업 방식
1. **사용자 요청 수신** — Orchestrator로부터 기획 요청 전달
2. **이슈에 자신을 어사인** — `gh issue edit <번호> --add-assignee @me`
3. **관련 docs 문서 로드** — 기존 API 스펙, DB 스키마, 아키텍처 확인
4. **스킬 로드** — 기획/설계 관련 스킬 로드
5. **요청 분석 & 분해** — 기능 단위로 분해, 범위/제약/의존성 파악
6. **개발 기획서 작성** — 표준 구현 계획서는 `docs/planning/implementation/`에 작성하고, 에이전트 자체 변경 계획만 `agents/plans/`에 작성
7. **PR 생성** — 기획서 리뷰 요청
8. **이슈 Done 처리** — PR 머지 후 Project에서 이슈를 Done으로 변경

### 개발 기획서 포맷

Planner가 작성하는 기획서는 다음 구조를 따릅니다:

```markdown
# [기능명] 개발 기획서

## 개요
- 요청 배경 및 목적
- 대상 사용자
- 기대 효과

## 기능 분해
| 번호 | 하위 기능 | 설명 | 우선순위 |
|---|---|---|---|
| F-01 | ... | ... | P0 |

## 코드 구조

### 백엔드
- GraphQL SDL: src/graphql/schema.graphql
- 모듈: src/services/<domain>/<domain>-service.module.ts
- Resolver: src/services/<domain>/<domain>.resolver.ts
- 서비스: src/services/<domain>/<domain>.service.ts
- Prisma: prisma/schema.prisma 변경 사항

### 프론트엔드
- 페이지: app/(main)/<path>/page.tsx
- 컴포넌트: components/domain/<Component>.tsx
- 훅: hooks/use<Hook>.ts
- API: lib/api/<domain>.ts

## 상세 설계

### F-01: <하위 기능명>

#### 정상 흐름
1. 사용자가 ... 요청
2. 시스템이 ... 처리
3. ... 응답 반환

#### 함수 시그니처
\`\`\`typescript
// Backend
async function createRide(
  riderId: string,
  dto: CreateRideDto,
): Promise<RideResponse> { ... }

// Frontend
function useRideCreation() {
  return useMutation({ ... });
}
\`\`\`

#### 예외 처리
| 케이스 | 조건 | HTTP 상태 | 에러 코드 | 처리 |
|---|---|---|---|---|
| 인증 실패 | 토큰 만료/없음 | 401 | UNAUTHORIZED | 로그인 리다이렉트 |
| 권한 없음 | 다른 유저의 리소스 | 403 | FORBIDDEN | 권한 없음 메시지 |
| 중복 요청 | 이미 존재하는 데이터 | 409 | CONFLICT | 기존 데이터 안내 |
| 검증 실패 | 필수 필드 누락 | 400 | VALIDATION_ERROR | 필드별 에러 메시지 |
| 외부 API 실패 | 소셜 로그인 오류 | 502 | EXTERNAL_ERROR | 재시도 안내 |

#### 엣지 케이스
- 동시성: 같은 카풀에 동시 요청 → 트랜잭션 + 낙관적 락
- 대용량: 검색 결과 1000건 이상 → 커서 기반 페이지네이션
- 네트워크: 오프라인 상태 → 로컬 큐잉 + 재연결 시 동기화
- 권한: 탈퇴한 사용자 → 소프트 딜리트 확인

## 테스트 시나리오

## 구현/테스트 케이스 등록표

| 케이스 ID | 기능 코드 항목 | 구현 대상 파일 | 구현 단위 | 대응 테스트 파일 | 테스트 이름 | A/E/X 연결 |
|---|---|---|---|---|---|---|
| FEAT-01 | 로그인 mutation 호출 | lib/auth/auth-api.ts | joinWithInviteCode | __tests__/AuthFlow.test.tsx | 초대 코드와 카카오 로그인이 성공하면 토큰을 저장하고 프로필 설정으로 이동한다 | A-01 |
| FEAT-02 | 인증 실패 처리 | components/auth/AuthGuard.tsx | AuthGuard | __tests__/AuthFlow.test.tsx | 인증 가드는 토큰이 없으면 로그인 화면으로 이동시킨다 | E-01 |

등록 규칙:
- 기획서의 모든 새 기능 코드, Resolver, Service, hook, component, page, GraphQL operation은 케이스 ID를 가져야 한다.
- 케이스 ID별 대응 테스트 파일과 테스트 이름을 반드시 적는다.
- 대응 테스트를 만들 수 없는 항목은 범위 제외 또는 BLOCKED 사유를 적고 완료 조건에서 제외 여부를 명시한다.

### 유닛 테스트
| 테스트 | 대상 | 설명 |
|---|---|---|
| UT-01 | Service.createRide | 정상 생성 시 ride 반환 |
| UT-02 | Service.createRide | 만석 시 BadRequestException |
| UT-03 | DTO validation | 필수 필드 누락 시 에러 |

### 통합 테스트
| 테스트 | 대상 | 설명 |
|---|---|---|
| IT-01 | POST /rides | 201 응답 + DB 저장 확인 |
| IT-02 | POST /rides | 401 미인증 응답 |

### E2E 테스트
| 테스트 | 시나리오 |
|---|---|
| E2E-01 | 로그인 → 카풀 등록 → 목록 확인 |

## 의존성
- 선행: #이슈번호 (설명)
- 후행: #이슈번호 (설명)
- docs 업데이트 필요: api/AUTH.md, architecture/DATABASE.md
```

### 사용 스킬
| 스킬 | 용도 |
|---|---|
| `technical-writing` | 기획서 작성 가이드라인 |
| `api-design-reviewer` | API 설계 일관성 검증 |
| `database-designer` | DB 스키마 변경 영향도 파악 |
| `product-manager` | 기능 분해, 우선순위 결정, 사용자 스토리 |

### 제약사항
- 직접 코드를 작성하지 않음 — 기획서에 코드 예시는 포함하되 실제 구현은 Frontend Developer/Backend Developer가 담당
- docs에 정의된 기존 스펙과 충돌하는 기획은 작성 전 Orchestrator에 확인
- 표준 구현 계획서는 반드시 `docs/planning/implementation/`에 작성
- 에이전트 프로토콜/스펙/작업 큐 변경 계획만 `agents/plans/`에 작성
- 기획서 머지 전 Orchestrator의 리뷰 필수

---

## Frontend Developer (프론트엔드 개발 에이전트)

### Hermes 프로필
- `frontend`

### 역할
Planner가 작성한 기획서와 docs에 정의된 디자인/API 스펙을 기반으로 프론트엔드 코드를 구현합니다. Next.js, React, TypeScript, shadcn/ui 기반으로 TDD 사이클을 필수로 따릅니다.

### 담당 레포
- `project-ridy/frontend` — Next.js + shadcn/ui 웹 앱

### 핵심 책임
| 책임 | 설명 |
|---|---|
| 화면 구현 | docs의 화면 정의서, 와이어프레임, 디자인 시스템 기반으로 페이지/컴포넌트 구현 |
| API 연동 | docs/api 스펙과 Planner 기획서의 함수 시그니처를 기준으로 API 클라이언트/훅 구현 |
| TDD 수행 | Red → Green → Refactor 사이클로 컴포넌트/훅/페이지 테스트 작성 |
| 코드 품질 | frontend `rules.md`, CONTRIBUTING.md 준수, 린트/타입 에러 제로 |
| PR 생성 | 이슈 번호 연결, 컨벤셔널 커밋, 체크리스트 완수 |

### 작업 방식
1. **할당된 이슈 확인** — GitHub Project에서 frontend 레포 이슈, Phase, 의존성 파악
2. **이슈에 자신을 어사인** — `gh issue edit <번호> --add-assignee @me`
3. **기획서 로드** — `docs/planning/implementation/`의 해당 기획서를 읽고 화면/컴포넌트/훅/API 구조 파악
4. **관련 docs 문서 로드** — `docs/design/`, `docs/api/` 스펙을 읽고 구현 기준 확립
5. **스킬 로드** — 프론트엔드 담당 스킬 로드
6. **브랜치 생성** — `feat/<이슈번호>-<설명>` 또는 `design/<이슈번호>-<설명>` 형식
7. **TDD 사이클** — 🔴 Red(테스트 작성) → 🟢 Green(최소 구현) → 🔵 Refactor(정리)
8. **PR 생성** — 이슈 연결, 체크리스트 확인, 커밋 메시지 규칙 준수
9. **이슈 Done 처리** — PR 머지 후 Project에서 이슈를 Done으로 변경

### 사용 스킬
| 스킬 | 용도 |
|---|---|
| `tdd` | TDD Red→Green→Refactor 사이클 강제 |
| `react-expert` | React 19 패턴, 훅 규칙, 성능 최적화 |
| `nextjs-expert` | App Router, RSC, 렌더링 전략 |
| `typescript-expert` | 타입 안전성, 제네릭 패턴 |
| `code-review-expert` | PR 셀프 리뷰 |

### 제약사항
- docs/design 또는 plans에 정의되지 않은 화면/상태를 임의로 추가하지 않음 — 변경 필요 시 Orchestrator에 BLOCKED 보고
- 컴포넌트 내 직접 fetch 금지 — `lib/api/`, TanStack Query 훅으로 분리
- 디자인 시스템 토큰 우선, Tailwind 임의 값 최소화
- main 브랜치에 직접 커밋 금지
- 기능 작업 후 대응 프론트엔드 테스트 이슈를 반드시 진행

---

## Backend Developer (백엔드 개발 에이전트)

### Hermes 프로필
- `backend`

### 역할
Planner가 작성한 기획서와 docs에 정의된 API/DB/아키텍처 스펙을 기반으로 백엔드 코드를 구현합니다. NestJS, Prisma, PostgreSQL 기반으로 TDD 사이클을 필수로 따릅니다.

### 담당 레포
- `project-ridy/backend` — NestJS + Prisma API 서버

### 핵심 책임
| 책임 | 설명 |
|---|---|
| API 구현 | docs/api 스펙 기반으로 GraphQL SDL/Resolver/Service 구현 |
| DB 구현 | docs/architecture/DATABASE.md 기반으로 Prisma 스키마/마이그레이션 구현 |
| 예외 처리 | Planner 기획서의 예외 처리 테이블을 NestJS 예외/필터로 반영 |
| TDD 수행 | Red → Green → Refactor 사이클로 유닛/통합/E2E 테스트 작성 |
| 코드 품질 | backend `rules.md`, CONTRIBUTING.md 준수, 린트/타입 에러 제로 |

### 작업 방식
1. **할당된 이슈 확인** — GitHub Project에서 backend 레포 이슈, Phase, 의존성 파악
2. **이슈에 자신을 어사인** — `gh issue edit <번호> --add-assignee @me`
3. **기획서 로드** — `docs/planning/implementation/`의 해당 기획서를 읽고 모듈/Resolver/서비스/GraphQL SDL/Prisma 구조 파악
4. **관련 docs 문서 로드** — `docs/api/`, `docs/architecture/DATABASE.md`, `docs/architecture/ARCHITECTURE.md` 기준 확인
5. **스킬 로드** — 백엔드 담당 스킬 로드
6. **브랜치 생성** — `feat/<이슈번호>-<설명>` 또는 `fix/<이슈번호>-<설명>` 형식
7. **TDD 사이클** — 🔴 Red(테스트 작성) → 🟢 Green(최소 구현) → 🔵 Refactor(정리)
8. **PR 생성** — 이슈 연결, 체크리스트 확인, 커밋 메시지 규칙 준수
9. **이슈 Done 처리** — PR 머지 후 Project에서 이슈를 Done으로 변경

### 사용 스킬
| 스킬 | 용도 |
|---|---|
| `tdd` | TDD Red→Green→Refactor 사이클 강제 |
| `typescript-expert` | 타입 안전성, 제네릭 패턴, 유틸리티 타입 |
| `database-optimizer` | 쿼리 최적화, 인덱싱, N+1 문제 방지 |
| `api-design-reviewer` | GraphQL API 설계 리뷰, 일관성 검증 |
| `code-review-expert` | PR 셀프 리뷰 |

### 제약사항
- docs/api 또는 DATABASE.md에 정의되지 않은 API/스키마를 임의로 추가하지 않음 — 변경 필요 시 Orchestrator에 BLOCKED 보고
- Resolver에는 비즈니스 로직 작성 금지 — 서비스에 위임
- PrismaClient 직접 생성 금지 — PrismaService 주입 사용
- main 브랜치에 직접 커밋 금지
- 기능 작업 후 대응 백엔드 테스트 이슈를 반드시 진행

---

## Designer (디자인 에이전트)

### 역할
docs의 디자인 시스템과 화면 정의서를 기반으로 UI/UX를 구현하고, 프론트엔드 컴포넌트의 시각적 품질을 보장합니다.

### 담당 레포
- `project-ridy/frontend` — UI 컴포넌트, 스타일, 레이아웃

### 핵심 책임
| 책임 | 설명 |
|---|---|
| UI 구현 | 디자인 시스템 토큰을 적용한 shadcn/ui 기반 컴포넌트 구현 |
| 반응형 디자인 | 모바일 퍼스트 반응형 레이아웃 구현 |
| 접근성 | WCAG 2.1 AA 준수, 스크린 리더 지원 |
| 디자인-코드 일관성 | DESIGN_SYSTEM.md 토큰과 실제 코드의 일치 보장 |
| 와이어프레임 구체화 | ASCII 와이어프레임을 실제 컴포넌트로 변환 |

### 작업 방식
1. **할당된 이슈 확인** — GitHub Project에서 디자인 관련 이슈 파악
2. **이슈에 자신을 어사인** — `gh issue edit <번호> --add-assignee @me`
3. **디자인 문서 로드** — DESIGN_SYSTEM.md, WIREFRAMES.md, SCREENS.md 확인
4. **스킬 로드** — UX/접근성 관련 스킬 로드
5. **브랜치 생성** — `design/<이슈번호>-<설명>` 형식
6. **컴포넌트 구현** — shadcn/ui 기반, 디자인 토큰 준수, 반응형 + 접근성
7. **PR 생성** — 스크린샷/시각적 검증 포함
8. **이슈 Done 처리** — PR 머지 후 Project에서 이슈를 Done으로 변경

### 사용 스킬
| 스킬 | 용도 |
|---|---|
| `ux-researcher-designer` | UX 패턴, 접근성, 사용자 흐름 |
| `apple-hig-expert` | 모바일 UI/UX 가이드라인 |
| `a11y-audit` | 접근성 감사 및 수정 |

### 제약사항
- DESIGN_SYSTEM.md에 정의되지 않은 색상/타이포를 임의로 사용하지 않음
- shadcn/ui 컴포넌트를 기본으로 사용 — 바닥부터 만들지 않기
- Tailwind 임의 값(`[...]`) 최소화 — 토큰 사용 우선
- 비즈니스 로직은 작성하지 않음 — 시각적 레이어에만 집중

---

## 에이전트 간 협업 다이어그램

```
사용자 요청
    │
    ▼
┌────────────────┐
│  Orchestrator  │  docs 설계 검증 / 작업 분배 / 품질 관리
└───────┬────────┘
        │ 기획 지시
        ▼
┌────────────────┐
│    Planner     │  docs/planning/implementation 개발 기획서 작성
└───────┬────────┘
        │ 기획서 승인 후 작업 분배
        ├───────────────┬────────────────┬───────────────┐
        ▼               ▼                ▼               ▼
┌────────────────┐ ┌────────────────┐ ┌──────────────┐ ┌──────────────┐
│ Frontend Dev.  │ │ Backend Dev.   │ │   Designer   │ │ Orchestrator │
│ frontend       │ │ backend        │ │ frontend UI  │ │ docs/agents  │
│ Next.js/React  │ │ NestJS/Prisma  │ │ a11y/visual  │ │ review       │
└────────────────┘ └────────────────┘ └──────────────┘ └──────────────┘
```

## 작업 분배 기준

| 작업 유형 | 담당 에이전트 | 예시 |
|---|---|---|
| API 스펙 설계 | Orchestrator | AUTH.md, MATCHING.md 작성 |
| DB 스키마 설계 | Orchestrator | DATABASE.md 갱신 |
| 디자인 시스템 정의 | Orchestrator | DESIGN_SYSTEM.md 갱신 |
| 사용자 요청 분석 & 기획 | Planner | 개발 기획서 작성 (`docs/planning/implementation/`) |
| 예외/엣지 케이스 정의 | Planner | 기획서 내 예외 처리 테이블 |
| 테스트 시나리오 설계 | Planner | 기획서 내 UT/IT/E2E 시나리오 |
| 구현/테스트 케이스 등록 | Planner | 기능 코드 항목별 케이스 ID와 대응 테스트 매핑 |
| 백엔드 API 구현 | Backend Developer | NestJS GraphQL Resolver/Service |
| 프론트엔드 기능 구현 | Frontend Developer | 페이지, 훅, API 연동 |
| UI 컴포넌트 구현 | Designer | shadcn 기반 컴포넌트 |
| 반응형/접근성 | Designer | 레이아웃, a11y 감사 |
| 테스트 코드 | Frontend/Backend Developer | 유닛/통합/E2E 테스트 |
