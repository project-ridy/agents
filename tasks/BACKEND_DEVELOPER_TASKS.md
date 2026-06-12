# Ridy — 백엔드 개발 에이전트 작업 큐

> Orchestrator가 이 파일에 백엔드 작업을 추가합니다. Backend Developer(`backend`)는 여기서 작업을 가져가 수행합니다.

## 현재 작업

### TASK-B004: 매칭 API 구현
- **상태**: PENDING
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: TASK-B001, TASK-B003
- **설명**: GraphQL schema-first 방식으로 카풀 생성, 검색, 요청, 요청 승인/거절 API 구현
- **완료 조건**:
  - `createRide`, `searchRides`, `requestRide`, `myRides` GraphQL resolver 동작
  - 회사 단위 데이터 격리 적용
  - 동시 요청/만석 예외 테스트 통과
  - `npm run codegen`, `npm run test`, `npm run build` 통과
- **참고 문서**: docs/api/MATCHING.md, docs/architecture/DATABASE.md

---

## 완료된 작업

### TASK-B001: 백엔드 프로젝트 셋업
- **상태**: DONE
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: 없음
- **설명**: NestJS 11, GraphQL Gateway, Prisma 7, 공통 context/event/health 계층 구성
- **완료 근거**:
  - `src/app/app.module.ts`, `src/gateway/graphql/graphql-gateway.module.ts` 존재
  - `src/services/health/*`, `src/common/context/*`, `src/common/events/*` 존재
  - `src/architecture/msa-structure.spec.ts`, `src/services/health/health.service.spec.ts` 존재
- **참고 문서**: docs/architecture/ARCHITECTURE.md, docs/planning/implementation/2026-06-11_backend-msa-project-structure.md

### TASK-B002: 인증 API 구현
- **상태**: DONE
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: TASK-B001
- **설명**: 초대 코드 가입, 로그인, refresh token, 현재 사용자 조회, JWT 발급/검증, OAuth verifier 구현
- **완료 근거**:
  - `src/services/auth/auth.resolver.ts`, `auth.service.ts`, `jwt-token.service.ts`, `auth.guard.ts`, `social-oauth.verifier.ts` 존재
  - `src/services/auth/auth.service.spec.ts`, `social-oauth.verifier.spec.ts` 존재
  - `frontend/src/graphql/operations/auth.graphql`에서 인증 operation 사용 중
- **참고 문서**: docs/api/AUTH.md, docs/architecture/DATABASE.md, docs/planning/implementation/2026-06-11_backend-auth-api.md

### TASK-B003: DB 스키마 구현
- **상태**: DONE
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: TASK-B001
- **설명**: Prisma 7 스키마로 companies, invite_codes, users, rides, ride_requests, chat_rooms, messages, settlements, reviews, vehicles, payment_methods 모델 구현
- **완료 근거**:
  - `prisma/schema.prisma`에 핵심 모델과 enum 존재
  - `src/prisma/prisma.service.ts`, `prisma.service.spec.ts` 존재
  - `src/graphql/schema.graphql`, `src/graphql/generated/schema-types.ts` 존재
- **참고 문서**: docs/architecture/DATABASE.md, docs/planning/implementation/2026-06-11_be2-prisma-schema.md

### TASK-B013: 회사 초대 코드 시스템 구현
- **상태**: DONE
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: TASK-B001, TASK-B003
- **설명**: 관리자 초대 코드 생성/검증/목록/비활성화와 회사 정원 검증 구현
- **완료 근거**:
  - `src/services/company/invite-code/invite-code.resolver.ts`, `invite-code.service.ts`, `invite-code.module.ts` 존재
  - `src/services/company/invite-code/invite-code.service.spec.ts` 존재
- **참고 문서**: docs/planning/implementation/2026-06-11_be13-invite-code-system.md
