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

# 내용을 추가한 초기화 예시
/init 프로젝트 초기화를 한국어로 진행해주세요.
다음 설정을 CLAUDE.md에 추가해주세요:
## 언어 및 커뮤니케이션 규칙
- **기본 응답 언어**: 한국어
- **코드 주석 언어**: 한국어로 작성
- **커밋 메시지**: 한국어로 작성
- **문서화**: 한국어로 작성
- **변수명/함수명**: 영어(코드 표준 준수)
```

## 권한
### 권한 관리
https://code.claude.com/docs/en/settings#permission-settings
```
/permissions
Add a new rule...
```
- 클로드 코드에 Bash(npm run build) 처럼 도구명 + 괄호 () 형태로 권한을 부여할 수 있다.
- 해당 도구에 모든 권한을 주고 싶을 경우 도구명만 입력한다. 예: Bash
- .claude/settings.local.json 파일에 권한 설정이 저장된다.
- permissions -> Workspace: 클로드코드가 현재 프로젝트 외부에 있는 다른 프로젝트 파일에 접근할 수 있는 권한을 부여한다.
예) ../other-directory

## 세션 계속하기
```
claude --continue
혹은
/resume
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

- Claude Code에 Figma MCP 추가
  ```
  # 프로젝트별 설정
  claude mcp add --transport http figma https://mcp.figma.com/mcp -s project
  ```
- Claude Code 재시작
- 인증 확인
- Claude Code에서 /mcp 명령 입력
- figma 선택
- "Authenticate" 선택
- "Allow Access" 클릭하여 Figma 계정 연결

### BrowserTools MCP 설정
- Download the latest Chrome extension
  - https://playbooks.com/mcp/agentdeskai-browser-tools
  - 압축해제
  - 크롬 확장 프로그램(chrome://extensions/) -> 개발자 모드 켜기 - > 압축해제된 확장 프로그램 로드
- Run the local node server
  ```
  npx @agentdeskai/browser-tools-server@1.2.0
  ```
- Install the MCP server
  ```
  claude mcp add-json "browser-tools" '{"command":"npx","args":["@agentdeskai/browser-tools-mcp@1.2.0"]}'  -s project
  ```