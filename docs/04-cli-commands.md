# CLI 명령어 및 UI 설정

## 기본 슬래시 명령어

| 명령어 | 설명 |
|--------|------|
| `/help` | 사용 가능한 명령어 목록 표시. 공식 문서에 나오지 않지만 최신 버전에서 추가된 명령어를 바로 확인 가능 |
| `/release-notes` | 최신 릴리스 노트 확인 |
| `/model` | 현재 모델 확인 및 변경 |
| `/resume` | 이전 세션 계속하기 |
| `/status` | 현재 상태 확인. 버던, 디렉토리, 모델, IDE 통합 등 |
| `/doctor` | 클로드 코드 설치와 설정 문제 진단 |
| `/config` | 현재 설정 확인 및 변경 |

## 상태 표시줄 (Status Line)

- 참고: https://code.claude.com/docs/ko/statusline
- `/statusline`: statusline-setup
- 기본적으로 `~/.claude/settings.json` 파일에 설정이 저장된다.
- 커스텀: `/statusline` + 프롬프트

```
예)
/statusline 프로젝트, 모델명, git 브랜치를 컬러풀하게 이모지와 함께 표시해줘
```

## 출력 스타일 (Output Style)

- `/output-style`: 출력 스타일 설정
- 기본 옵션: `default`, `Explanatory`, `Learning`
- `/output-style:new` + 프롬프트: 커스텀 설정 가능
  - `~/.claude/output-styles/junior-developer-education.md` 처럼 저장됨

```
예)
/output-style:new 주니어 개발자 교육 스타일.
- 모든 개념을 ELI5(5살에게 설명하듯) 방식으로
- 실생활 비유 사용
- 단계별 설명(Step 1, 2, 3, ...)
- 자주 하는 실수 예방 팁
- 연습 문제 3개씩 제공
```