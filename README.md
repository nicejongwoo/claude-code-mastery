# Expo 앱에 오신 것을 환영합니다 👋

이 프로젝트는 [`create-expo-app`](https://www.npmjs.com/package/create-expo-app)으로 생성된 [Expo](https://expo.dev) 프로젝트입니다.

```expo 프로젝트 생성
npx create-expo-app@latest claude-example 
```

## 시작하기

1. 의존성 설치

   ```bash
   npm install
   ```

2. 앱 시작

   ```bash
   npx expo start
   ```

출력에서 다음 환경에서 앱을 열 수 있는 옵션을 찾을 수 있습니다.

- [개발 빌드](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android 에뮬레이터](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS 시뮬레이터](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), Expo로 앱 개발을 시도하기 위한 제한된 샌드박스

**app** 디렉토리 내의 파일을 편집하여 개발을 시작할 수 있습니다. 이 프로젝트는 [파일 기반 라우팅](https://docs.expo.dev/router/introduction)을 사용합니다.

## 새 프로젝트 시작하기

준비가 되면 다음 명령어를 실행하세요:

```bash
npm run reset-project
```

이 명령어는 시작 코드를 **app-example** 디렉토리로 이동하고, 개발을 시작할 수 있는 빈 **app** 디렉토리를 생성합니다.

## 더 알아보기

Expo로 프로젝트를 개발하는 방법에 대해 더 알아보려면 다음 자료를 참고하세요:

- [Expo 문서](https://docs.expo.dev/): 기본 사항을 배우거나, [가이드](https://docs.expo.dev/guides)를 통해 고급 주제를 살펴보세요.
- [Expo 튜토리얼 배우기](https://docs.expo.dev/tutorial/introduction/): Android, iOS, 웹에서 실행되는 프로젝트를 만드는 단계별 튜토리얼을 따라해보세요.

## 커뮤니티 참여

범용 앱을 만드는 개발자 커뮤니티에 참여하세요.

- [GitHub의 Expo](https://github.com/expo/expo): 오픈 소스 플랫폼을 보고 기여하세요.
- [Discord 커뮤니티](https://chat.expo.dev): Expo 사용자들과 채팅하고 질문하세요.

# 클로드 코드 활용 가이드

## 바이브 코팅 상급 팁

1. 무조건 스펙 먼저 작성하자
2. 스펙은 여러 고급 모델끼리 토론시켜서 품질을 끌어 올리자
3. 반드시 중간에 개입해서 AI의 방향성을 통제하고, 문맥을 이해하면서 명령하자
4. AI는 주니어 개발자다.
5. 비싼 모델을 설계/리뷰, 전용 코딩 모델은 구현하도록 위임하자. 적절한 역할 분리로 비용, 속도 품질을 모두 잡자

## CLAUDE.md 작성하기

```
# 클로드 코드 접속
claude

# 초기화 명령어
/init
```

## 서브에이전트 설정

Create new agent
![sub-agents_1.png](images/sub-agents_1.png)
Project
![sub-agents_2.png](images/sub-agents_2.png)
Generate with Claude
![sub-agents_3.png](images/sub-agents_3.png)
Describe what this agent should do and when it should be used

- UI만 구현하는 서브 에이전트 생성한다. UI 구현이 필요할 때 호출한다.
- 이미 존재하는 UI 구현 서브 에이전트와 협력하여 기능을 구현하는 서브 에이전트이다. UI외의 기능을 구현할 때 이 서브 에이전트를 호출한다.
- 이 에이전트는 UI 생성 서브 에이전트와 기능 구현 서브 에이전트의 협력 결과를 최종 리부하는 서브 에이전트이다. UI 생성과 기능 구현이 한 번 완료될 때 이 서브 에이전트가 사용된다.
  ![sub-agents_4.png](images/sub-agents_4.png)
  All tools > Continue
  ![sub-agents_5.png](images/sub-agents_5.png)
  Sonnet
  ![sub-agents_6.png](images/sub-agents_6.png)
  Automatic color
  ![sub-agents_7.png](images/sub-agents_7.png)
  result
  ![sub-agents_8.png](images/sub-agents_8.png)
  준비되면 얘기해 주세요.

### 서브 에이전트 설명 GPT 프롬프트 예시

```
나는 react native expo 프로젝트에서 claude code를 통해 바이브 코딩을 하고 있어. 3가지 서브 에이전트를 생성할건데, description을 생성해줘.
1. UI 관련 에이전트,
2. 기능 구현 에이전트,
3. 리뷰 에이전트

Describe what this agent should do and when it should be used (be comprehensive for best results)
```

## MCP

### Playwright MCP 설정 1. Playwright MCP 서버 설치하기

```
npm install -g @executeautomation/playwright-mcp-server
```

2. Claude Code에 연결하기

# 기본 연결 (현재 프로젝트에서만)

```
claude mcp add playwright -- npx @executeautomation/playwright-mcp-server

# 모든 프로젝트에서 사용하려면

claude mcp add playwright -s user -- npx @executeautomation/playwright-mcp-server
```

### Figma Remote MCP 설정

1. Claude Code에 Figma MCP 추가

```
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

2. Claude Code 재시작
3. 인증 확인
4. Claude Code에서 /mcp 명령 입력
5. figma 선택
6. "Authenticate" 선택
7. "Allow Access" 클릭하여 Figma 계정 연결