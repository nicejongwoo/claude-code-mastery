# 클로드 코드 프로젝트 가이드

이 프로젝트는 **Expo 기반 React Native 앱 개발**을 위한 클로드 코드 워크플로우를 설정한 가이드 프로젝트입니다.

## 프로젝트 개요

이 프로젝트는 클로드 코드(Claude Code)를 사용하여 Expo React Native 애플리케이션을 효율적으로 개발하기 위한 구조를 제공합니다. 세 개의 전문화된 서브 에이전트가 설정되어 있어 UI 개발, 기능 구현, 최종 검수를 각각 담당합니다.

## 빠른 시작

### 클로드 코드 실행

```bash
claude
```

### 프로젝트 초기화

```bash
/init
```

## 설정된 서브 에이전트

이 프로젝트에는 다음 세 개의 서브 에이전트가 설정되어 있습니다:

### 1. UI 컴포넌트 빌더 (ui-component-builder)

**역할**: UI 컴포넌트와 화면 레이아웃 구현

이 에이전트는 다음 작업을 담당합니다:
- `components/` 디렉토리에 재사용 가능한 UI 컴포넌트 생성
- Expo Router 라우트 구조에 맞는 화면 UI 구성
- `constants/theme.ts`의 중앙 집중식 색상 및 글꼴 사용
- 필요시 `.ios.ts`, `.android.ts`, `.web.ts` 파일 확장자로 플랫폼별 UI 구현

**언제 사용하나요?**
- "회원가입 화면 UI를 만들어줘"
- "상품 목록을 표시하는 컴포넌트가 필요해"
- "다크 테마를 지원하는 버튼을 만들어줘"

### 2. 기능 구현자 (feature-implementer)

**역할**: 비즈니스 로직 및 상태 관리 구현

UI 에이전트가 화면 구조를 완성한 후, 이 에이전트는 다음을 구현합니다:
- 데이터 페칭 및 API 통신
- 상태 관리 (useState, useReducer, Context API 등)
- 이벤트 핸들러 및 사용자 인터랙션 로직
- 에러 처리 및 로딩 상태 관리

**언제 사용하나요?**
- UI는 완성되었고 서버에서 데이터를 불러오는 기능이 필요할 때
- 각 탭 간에 상태를 동기화해야 할 때
- "홈 화면 UI는 완성되었는데, 사용자 목록을 API에서 불러오는 기능을 구현해줘"

### 3. 최종 검수 담당자 (final-review-coordinator)

**역할**: 통합 검증 및 품질 보증

UI 개발과 기능 구현이 모두 완료된 후, 이 에이전트는 다음을 확인합니다:
- UI와 기능이 제대로 통합되었는지 검증
- 프로젝트 표준 준수 확인
- 코드 품질 및 성능 검토
- 배포 전 최종 승인

**언제 사용하나요?**
- UI와 기능 구현이 모두 완료되어 최종 검수가 필요할 때
- "로그인 화면 UI와 인증 로직이 완성되었는데, 최종 검수해줄 수 있어?"

## 권장 개발 워크플로우

### 1단계: UI 설계 및 구현

```
사용자: "회원가입 화면을 만들어줘. 입력 필드 3개(이메일, 비밀번호, 닉네임)와 가입 버튼이 필요해"

→ ui-component-builder 에이전트가 회원가입 화면의 UI를 생성합니다
```

### 2단계: 기능 구현

```
사용자: "회원가입 UI는 완성되었는데, 실제 회원가입 API 호출 기능을 추가해줘"

→ feature-implementer 에이전트가 API 통신 및 상태 관리 로직을 구현합니다
```

### 3단계: 최종 검수

```
사용자: "회원가입 화면의 UI와 기능이 모두 완성되었습니다. 최종 검수해주세요"

→ final-review-coordinator 에이전트가 전체 통합을 검증하고 승인합니다
```

## 프로젝트 구조

```
.
├── app/                      # Expo Router 라우트
│   ├── _layout.tsx          # 루트 레이아웃 (테마 제공자)
│   ├── (tabs)/              # 탭 기반 라우트
│   └── modal.tsx            # 모달 라우트
├── components/              # 재사용 가능한 UI 컴포넌트
│   ├── themed-text.tsx
│   ├── themed-view.tsx
│   └── ui/                  # 기본 UI 요소
├── hooks/                   # 사용자 정의 React 훅
│   ├── use-color-scheme.ts
│   └── use-theme-color.ts
├── constants/               # 전역 상수
│   └── theme.ts            # 색상 및 글꼴 정의
├── CLAUDE.md               # 클로드 코드 프로젝트 지침
└── README.md               # 본 파일
```

## 개발 명령어

프로젝트가 준비되면 다음 명령어를 사용할 수 있습니다:

```bash
# 앱 실행
npm start              # Expo 개발 서버 시작
npm run ios           # iOS 시뮬레이터에서 실행
npm run android       # Android 에뮬레이터에서 실행
npm run web           # 웹에서 실행

# 코드 품질 검사
npm run lint          # ESLint 실행

# 프로젝트 초기화
npm run reset-project # 시작 코드를 app-example/로 이동
```

## 주요 규칙

- **경로 별칭**: `@/*`를 사용하여 프로젝트 루트에서 임포트 (예: `@/components/themed-text`)
- **테마 관리**: `constants/theme.ts`의 중앙 집중식 색상과 글꼴 사용
- **플랫폼별 코드**: `.ios.ts`, `.android.ts`, `.web.ts` 확장자로 플랫폼별 구현
- **TypeScript**: 엄격 모드 준수, 명시적 타입 정의
- **테마 인식 컴포넌트**: `ThemedText`, `ThemedView` 사용으로 자동 테마 적용

## 서브 에이전트 설정 방법

새로운 서브 에이전트를 추가하려면:

1. Claude Code의 프로젝트 설정에서 "Create new agent" 선택
2. 에이전트의 설명(description)과 역할(when to use) 정의
3. `.claude/agents/` 디렉토리에 마크다운 파일로 저장

## MCP 통합 (선택사항)

필요에 따라 MCP(Model Context Protocol) 서버를 추가할 수 있습니다:

### Playwright MCP 설치

```bash
# 서버 설치
npm install -g @executeautomation/playwright-mcp-server

# Claude Code에 추가 (현재 프로젝트만)
claude mcp add playwright -- npx @executeautomation/playwright-mcp-server

# 모든 프로젝트에서 사용하려면
claude mcp add playwright -s user -- npx @executeautomation/playwright-mcp-server
```

### Figma Remote MCP 설치

```bash
# Figma MCP 추가
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

그 후 Claude Code를 재시작하고 `/mcp` 명령으로 Figma 인증을 진행하세요.

## 참고

- 더 자세한 프로젝트 지침은 `CLAUDE.md` 파일을 확인하세요
- 각 서브 에이전트의 상세 설명은 `.claude/agents/` 디렉토리의 파일들을 참고하세요