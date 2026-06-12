# Ridy — 프론트엔드 개발 에이전트 작업 큐

> Orchestrator가 이 파일에 프론트엔드 작업을 추가합니다. Frontend Developer(`frontend`)는 여기서 작업을 가져가 수행합니다.

## 현재 작업

### TASK-F003: 매칭 화면 및 매칭 API 연동
- **상태**: PENDING
- **담당**: frontend
- **레포**: frontend
- **우선순위**: HIGH
- **의존성**: backend 매칭 API 구현
- **설명**: `/matchings` 화면, 검색 결과, `searchRides`/`myRides` GraphQL operation, TanStack Query 훅 구현
- **완료 조건**:
  - `/matchings` 경로 렌더링
  - 홈 화면 검색 쿼리 파라미터 반영
  - `src/graphql/operations/matching.graphql` 작성 및 `npm run codegen` 성공
  - 로딩/빈/에러/데이터 상태 테스트 통과
- **참고 문서**: docs/planning/implementation/2026-06-12_fe05-home-screen.md, docs/api/MATCHING.md

---

## 완료된 작업

### TASK-F001: 프론트엔드 프로젝트 셋업
- **상태**: DONE
- **담당**: frontend
- **레포**: frontend
- **우선순위**: HIGH
- **의존성**: 없음
- **설명**: Next.js 16 (App Router) 프로젝트 초기 셋업. Tailwind CSS, Pretendard 폰트, 디자인 토큰 설정
- **완료 근거**:
  - `app/layout.tsx`, `app/globals.css`, `app/providers.tsx` 존재
  - `components/ui/*` shadcn/ui 기반 컴포넌트 존재
  - `__tests__/designTokens.test.ts`로 토큰 검증
- **참고 문서**: docs/design/DESIGN_SYSTEM.md

### TASK-F002: 로그인/온보딩 화면 구현
- **상태**: DONE
- **담당**: frontend
- **레포**: frontend
- **우선순위**: MEDIUM
- **의존성**: TASK-F001
- **설명**: 초대 코드 기반 로그인, 카카오/구글 소셜 로그인 진입, 프로필 설정, 차주 차량 등록 폼 구현
- **완료 근거**:
  - `app/login/page.tsx`, `app/profile/setup/page.tsx` 존재
  - `components/auth/LoginForm.tsx`, `ProfileSetupForm.tsx`, `AuthGuard.tsx` 존재
  - `lib/auth/*`, `lib/api/graphql-client.ts` 존재
  - `__tests__/AuthFlow.test.tsx`로 로그인/온보딩 플로우 검증
- **참고 문서**: docs/design/WIREFRAMES.md, docs/design/DESIGN_SYSTEM.md, docs/api/AUTH.md

### TASK-F005: 홈 화면 1차 구현
- **상태**: DONE
- **담당**: frontend
- **레포**: frontend
- **우선순위**: HIGH
- **의존성**: TASK-F001, TASK-F002
- **설명**: 인증 보호 홈 화면, 경로 검색 입력, 매칭 화면 라우팅, 정기 카풀 섹션, 하단 내비게이션 구현
- **완료 근거**:
  - `app/page.tsx` 존재
  - `components/ridy/RouteInput.tsx`, `MatchingCard.tsx`, `BottomNavigation.tsx` 존재
  - `__tests__/Home.test.tsx`로 인증 보호, 검색 라우팅, 하단 내비게이션 검증
- **후속**: backend 매칭 API 완료 후 `myRides`/`searchRides` 실연동
- **참고 문서**: docs/planning/implementation/2026-06-12_fe05-home-screen.md
