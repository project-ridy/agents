# Ridy — 디자인 에이전트 작업 큐

> Orchestrator가 이 파일에 작업을 추가합니다. Designer 에이전트는 여기서 작업을 가져가 수행합니다.

## 현재 작업

### TASK-D004: 매칭 결과/상세 화면 디자인
- **상태**: PENDING
- **담당**: designer
- **레포**: frontend
- **우선순위**: MEDIUM
- **의존성**: TASK-D001, backend 매칭 API 구현
- **설명**: 매칭 검색 결과 리스트, 매칭 상세 프로필 화면 UI
- **완료 조건**:
  - 매칭 카드 리스트 UI
  - 매칭 상세 화면 (프로필, 경로 지도, 요청 버튼)
  - 지도 컴포넌트 포함
- **참고 문서**: docs/design/WIREFRAMES.md, docs/design/SCREENS.md

### TASK-D005: 채팅 화면 디자인
- **상태**: PENDING
- **담당**: designer
- **레포**: frontend
- **우선순위**: MEDIUM
- **의존성**: TASK-D001
- **설명**: 채팅방 목록, 실시간 채팅 화면 UI
- **완료 조건**:
  - 채팅방 리스트 UI
  - 채팅 화면 (메시지 버블, 입력창, 타이핑 표시)
  - 시스템 메시지 스타일
- **참고 문서**: docs/design/WIREFRAMES.md, docs/design/SCREENS.md

---

## 완료된 작업

### TASK-D001: 디자인 시스템 컴포넌트 구현
- **상태**: DONE
- **담당**: designer
- **레포**: frontend
- **우선순위**: HIGH
- **의존성**: 없음
- **설명**: DESIGN_SYSTEM.md에 정의된 토큰과 shadcn/ui 기반 컴포넌트 구현
- **완료 근거**:
  - `app/globals.css`에 Ridy 색상/타이포/간격/반경 토큰 존재
  - `components/ui/*` shadcn/ui 컴포넌트 존재
  - `components/ridy/MatchingCard.tsx`, `RouteInput.tsx`, `BottomNavigation.tsx` 존재
  - `__tests__/designTokens.test.ts`, `__tests__/RidyComponents.test.tsx` 존재
- **참고 문서**: docs/design/DESIGN_SYSTEM.md

### TASK-D002: 로그인/온보딩 화면 디자인 구체화
- **상태**: DONE
- **담당**: designer
- **레포**: frontend
- **우선순위**: HIGH
- **의존성**: TASK-D001
- **설명**: 로그인, 초대 코드, 프로필 설정, 차주 차량 입력 화면 구현
- **완료 근거**:
  - `components/auth/LoginForm.tsx`, `ProfileSetupForm.tsx` 존재
  - `app/login/page.tsx`, `app/profile/setup/page.tsx` 존재
  - `__tests__/AuthFlow.test.tsx` 존재
- **참고 문서**: docs/design/WIREFRAMES.md, docs/design/SCREENS.md

### TASK-D003: 홈 화면 디자인 구체화
- **상태**: DONE
- **담당**: designer
- **레포**: frontend
- **우선순위**: MEDIUM
- **의존성**: TASK-D001
- **설명**: 탑승자/차주 홈 화면 UI 구체화. 출발지/도착지 입력, 정기 카풀 카드, 바텀 내비게이션
- **완료 근거**:
  - `app/page.tsx`에 탑승자 홈, 경로 입력, 정기 카풀 카드, 빈 상태, 하단 내비게이션 존재
  - `__tests__/Home.test.tsx` 존재
- **후속**: 차주 홈 데이터화와 실 API 상태는 backend 매칭 API 이후 진행
- **참고 문서**: docs/design/WIREFRAMES.md, docs/design/SCREENS.md
