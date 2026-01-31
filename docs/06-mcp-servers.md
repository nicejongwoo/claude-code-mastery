# MCP (Model Context Protocol)

## 참고 자료

- 공식 MCP Server: https://github.com/modelcontextprotocol/servers
- Awesome MCP Servers: https://github.com/punkpeye/awesome-mcp-servers
- MCP Server Hub: https://mcpserverhub.com/
- Glama MCP Directory: https://glama.ai/mcp/servers
- smithery.ai: https://smithery.ai/

## MCP 범위 설정

### Local 범위

- 개인적으로 사용할 MCP 서버
- default scope는 local

```
claude mcp add <mcp-name> -scope local /path/to/server
```

### Project 범위

- 팀 협업을 가능하도록 프로젝트 단위로 MCP 서버 추가
- 프로젝트 루트 디렉토리의 `.mcp.json` 파일에 구성

```
claude mcp add <mcp-name> -scope project /path/to/server
```

## Context7

- https://github.com/upstash/context7
- 아래 명령어 실행 후 `.mcp.json` 에서 api key는 지워도 사용하는데 큰 무리 없음

```
# remote
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: YOUR_API_KEY" --scope project

# local
# npx 앞쪽에 --scope project 옵션 추가
claude mcp add context7 --scope project -- npx -y @upstash/context7-mcp --api-key YOUR_API_KEY
```

- 프롬프트 입력후 `use context7` 을 입력한다

```
예)
Next.js로 개발을 진행할꺼야. 최신 웹 개발 기술 스택을 추천해줘
use context7
```

- `settings.json` 에서 `"mcp__context7"` 로 설정하면 모든 권한 허용

## Playwright MCP

### 1. 서버 설치

```
npm install -g @executeautomation/playwright-mcp-server
```

### 2. Claude Code에 연결

```
# 기본 연결 (현재 프로젝트에서만)
claude mcp add playwright -- npx @executeautomation/playwright-mcp-server

# 모든 프로젝트에서 사용하려면
claude mcp add playwright -s user -- npx @executeautomation/playwright-mcp-server
```

## Figma Remote MCP

Claude Code에 Figma MCP 추가:

```
# 프로젝트별 설정
claude mcp add --transport http figma https://mcp.figma.com/mcp -s project
```

연결 후 인증 절차:

1. Claude Code 재시작
2. Claude Code에서 `/mcp` 명령 입력
3. figma 선택
4. "Authenticate" 선택
5. "Allow Access" 클릭하여 Figma 계정 연결

## BrowserTools MCP

### 1. Chrome 확장 프로그램 설치

- https://playbooks.com/mcp/agentdeskai-browser-tools
- 압축해제
- 크롬 확장 프로그램(`chrome://extensions/`) -> 개발자 모드 켜기 -> 압축해제된 확장 프로그램 로드

### 2. 로컬 node 서버 실행

```
npx @agentdeskai/browser-tools-server@1.2.0
```

### 3. MCP 서버 설치

```
claude mcp add-json "browser-tools" '{"command":"npx","args":["@agentdeskai/browser-tools-mcp@1.2.0"]}'  -s project
```