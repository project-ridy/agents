# Ridy — 백엔드 개발 에이전트 작업 큐

> Orchestrator가 이 파일에 백엔드 작업을 추가합니다. Backend Developer(`backend`)는 여기서 작업을 가져가 수행합니다.

## 현재 작업

### TASK-B001: 백엔드 프로젝트 셋업
- **상태**: PENDING
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: 없음
- **설명**: NestJS 프로젝트 초기 셋업. Prisma, ESLint, Prettier, 환경변수 설정 포함
- **완료 조건**:
  - `npm run start:dev` 실행 시 서버 정상 구동
  - Prisma 스키마 초기 파일 존재
  - health check 엔드포인트 (`GET /health`) 정상 응답
- **참고 문서**: docs/architecture/ARCHITECTURE.md

### TASK-B002: 인증 API 구현
- **상태**: PENDING
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: TASK-B001
- **설명**: 소셜 로그인 (카카오/구글/Apple) 인증 API 구현. JWT 발급/갱신, 휴대폰 인증
- **완료 조건**:
  - POST /auth/login 정상 동작
  - POST /auth/refresh 정상 동작
  - JWT 검증 미들웨어 동작
  - 유닛 테스트 통과
- **참고 문서**: docs/api/AUTH.md, docs/architecture/DATABASE.md

### TASK-B003: DB 스키마 구현
- **상태**: PENDING
- **담당**: backend
- **레포**: backend
- **우선순위**: HIGH
- **의존성**: TASK-B001
- **설명**: Prisma 스키마 작성 및 마이그레이션. users, rides, ride_requests, chat_rooms, messages, settlements, reviews, vehicles, payment_methods 테이블
- **완료 조건**:
  - `npx prisma migrate dev` 성공
  - `npx prisma generate` 성공
  - 스키마가 DATABASE.md와 일치
- **참고 문서**: docs/architecture/DATABASE.md

---

## 완료된 작업

(없음)
