# 기본 설정

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

## 권한 관리

- 참고: https://code.claude.com/docs/en/settings#permission-settings

```
/permissions
Add a new rule...
```

- 클로드 코드에 `Bash(npm run build)` 처럼 도구명 + 괄호 `()` 형태로 권한을 부여할 수 있다.
- 해당 도구에 모든 권한을 주고 싶을 경우 도구명만 입력한다. 예: `Bash`
- `.claude/settings.local.json` 파일에 권한 설정이 저장된다.
- `permissions -> Workspace`: 클로드코드가 현재 프로젝트 외부에 있는 다른 프로젝트 파일에 접근할 수 있는 권한을 부여한다.
  - 예) `../other-directory`

## 세션 계속하기

```
claude --continue
혹은
/resume
```

## 권한 모드

클로드 코드가 기본적으로 어떻게 행동할지 결정하는 모드

- 참고: https://code.claude.com/docs/ko/iam#권한-모드

### plan 모드

클로드 코드가 작업을 수행하기 전에 계획을 세우도록 강제하기 때문에 중요하고 많이 사용하길 권장

```
claude
shift + tab
```

### bypassPermissions 모드 (Safe YOLO mode)

클로드 코드가 권한 제한을 무시하고 작업을 수행하도록 허용 (개발 환경에서만 안전하게 사용 권장)

```
claude --dangerouly-skip-permissions
```

## 설정 파일 (settings.json)

- 참고: https://code.claude.com/docs/ko/settings#설정-파일