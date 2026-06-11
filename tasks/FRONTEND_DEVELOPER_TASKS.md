# Ridy — 프론트엔드 개발 에이전트 작업 큐

> Orchestrator가 이 파일에 프론트엔드 작업을 추가합니다. Frontend Developer(`frontend`)는 여기서 작업을 가져가 수행합니다.

## 현재 작업

### TASK-F001: 프론트엔드 프로젝트 셋업
- **상태**: PENDING
- **담당**: frontend
- **레포**: frontend
- **우선순위**: HIGH
- **의존성**: 없음
- **설명**: Next.js 15 (App Router) 프로젝트 초기 셋업. Tailwind CSS, Pretendard 폰트, 디자인 토큰 설정
- **완료 조건**:
  - `npm run dev` 실행 시 개발 서버 구동
  - Tailwind CSS 정상 동작
  - Pretendard 폰트 적용
  - 디자인 시스템 토큰 (색상, 타이포) Tailwind config에 정의
- **참고 문서**: docs/design/DESIGN_SYSTEM.md

### TASK-F002: 로그인 화면 구현
- **상태**: PENDING
- **담당**: frontend
- **레포**: frontend
- **우선순위**: MEDIUM
- **의존성**: TASK-F001
- **설명**: 소셜 로그인 화면 UI 구현. 카카오/구글/Apple 로그인 버튼, 스플래시
- **완료 조건**:
  - 로그인 화면 렌더링
  - 소셜 로그인 버튼 클릭 시 인증 플로우 진입
  - 디자인 시스템 준수
- **참고 문서**: docs/design/WIREFRAMES.md, docs/design/DESIGN_SYSTEM.md

---

## 완료된 작업

(없음)
