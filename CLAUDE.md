# CLAUDE.md

이 파일은 이 저장소의 코드를 다룰 때 Claude Code (claude.ai/code)에게 지침을 제공합니다.

## 프로젝트 개요

이 프로젝트는 Expo 프로젝트입니다. Expo는 iOS, Android, 웹에서 실행되는 범용 앱을 구축하기 위한 React Native 프레임워크입니다. 이 앱은 파일 기반 라우팅을 위해 Expo Router를 사용하며, 라이트 및 다크 테마를 지원하는 탭 기반 내비게이션 구조를 특징으로 합니다.

## 개발 명령어

### 앱 실행
- `npm start` – Expo 개발 서버를 시작합니다.
- `npm run ios` – iOS 시뮬레이터에서 앱을 실행합니다.
- `npm run android` – Android 에뮬레이터에서 앱을 실행합니다.
- `npm run web` – 웹에서 앱을 실행합니다.

### 코드 품질
- `npm run lint` – Expo의 설정으로 ESLint를 실행합니다.

### 프로젝트 초기화
- `npm run reset-project` – 시작 코드를 `app-example/`로 이동하고 빈 `app/` 디렉토리를 생성합니다.

## 아키텍처 개요

### 주요 기술
- **Expo Router**: 파일 기반 라우팅 시스템입니다. `app/` 디렉토리 내의 파일 구조를 사용하여 `(tabs)` 폴더 그룹으로 라우트를 정의합니다.
- **React Navigation**: `@react-navigation/bottom-tabs`를 통해 탭 내비게이션을 제공합니다.
- **테마 관리**: 사용자 정의 색상 정의와 함께 React Navigation의 테마 제공자를 사용합니다.
- **Expo 모듈**: 다양한 Expo 모듈이 플랫폼별 기능(스플래시 화면, 상태 표시줄, 글꼴 등)을 처리합니다.
- **TypeScript**: 엄격 모드가 활성화되어 있으며, 경로 별칭(`@/*`는 프로젝트 루트를 가리킴)을 사용합니다.
- **React 19 & React Native 0.81**: Expo 설정에서 새로운 아키텍처가 활성화된 최신 React 기능을 사용합니다.

### 디렉토리 구조

- **`app/`** – Expo Router 라우트입니다. `_layout.tsx` 파일은 라우트 구조와 내비게이션을 정의합니다.
  - `(tabs)/` – 탭 기반 라우트 (홈 및 탐색 화면)
  - `modal.tsx` – 탭 위에 표시되는 모달 라우트
  - `_layout.tsx` – 테마 제공자가 있는 루트 레이아웃

- **`components/`** – 재사용 가능한 UI 컴포넌트
  - `ParallaxScrollView`, `ThemedText`, `ThemedView`와 같은 사용자 정의 컴포넌트는 테마에 따라 스타일을 적용합니다.
  - `ui/` – 하위 수준 UI 기본 요소

- **`hooks/`** – 사용자 정의 React 훅
  - `use-color-scheme.ts` – 라이트/다크 모드를 감지합니다 (네이티브 구현).
  - `use-color-scheme.web.ts` – 웹 전용 색상 스키마 감지 재정의
  - `use-theme-color.ts` – 테마 색상에 접근하기 위한 훅

- **`constants/`** – 전역 상수
  - `theme.ts` – 라이트 및 다크 테마를 위한 색상 및 글꼴 정의, 플랫폼별 글꼴 패밀리 포함

### 테마 및 스타일링

색상과 글꼴은 `constants/theme.ts`에 중앙 집중화되어 있습니다. 앱은 두 가지 계층의 테마 방식을 사용합니다:
1. **라이트/다크 모드 감지**: `useColorScheme()` 훅이 시스템 테마 선호도를 감지합니다.
2. **색상 적용**: 컴포넌트는 `ThemedText` 및 `ThemedView`를 사용하여 테마 색상을 자동으로 적용합니다.

플랫폼별 글꼴은 `theme.ts`에서 `Platform.select()`를 사용하여 iOS, Android, 웹의 차이를 처리하도록 정의됩니다.

### 내비게이션 구조

- **루트 레이아웃** (`app/_layout.tsx`): React Navigation의 `ThemeProvider`로 앱을 감싸고 메인 스택을 정의합니다.
- **탭 레이아웃** (`app/(tabs)/_layout.tsx`): 홈 및 탐색 화면이 있는 하단 탭 내비게이션을 정의합니다.
- **모달 화면** (`app/modal.tsx`): 탭 스택 위에 모달로 표시됩니다.

## 설정

- **TypeScript**: `tsconfig.json`은 엄격 모드와 경로 별칭을 사용하여 Expo의 기본 설정을 확장합니다.
- **ESLint**: 플랫 설정 형식을 사용하여 Expo의 ESLint 설정을 사용합니다.
- **Expo 설정**: `app.json`은 앱 메타데이터, 아이콘 및 Expo 특정 플러그인(라우터, 스플래시 화면)을 정의합니다.
- **새로운 아키텍처**: 최신 React 기능과의 React Native 호환성을 위해 `app.json`에서 활성화됩니다.

## 주요 규칙

- 프로젝트 루트에서 가져오기 위해 `@/*` 경로 별칭을 사용합니다 (예: `@/components/themed-text`).
- 플랫폼별 코드는 플랫폼 재정의를 위해 파일 확장자(`.ios.ts`, `.android.ts`, `.web.ts`)를 사용할 수 있습니다.
- 색상 및 글꼴 상수는 앱 전체의 일관성을 위해 `constants/theme.ts`에서 접근해야 합니다.
- 테마를 인식하는 컴포넌트는 `useColorScheme()`을 사용하고 중앙 집중식 테마 정의에서 색상을 적용해야 합니다.
- 앞으로 답변을 생성할때는 한글로 해주고, 입문자가 이해하기 쉬운 형태로 답변한다. 비유적인 표현은 사용하지 않는다.