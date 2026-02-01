# Open Spec + Repomix 가이드

> 참고:
> - [Repomix 공식 홈페이지](https://repomix.com)
> - [Repomix GitHub 저장소](https://github.com/yamadashy/repomix)

이 가이드는 **Spec Builder(대화형 스펙 작성)** + **Repomix(컨텍스트 패킹)** 조합을 사용하여 Claude Code의 효율을 높이는 방법을 설명합니다.

BMAD가 '10개의 에이전트가 역할극을 하는 팀 단위 프레임워크'라면, 이 조합은 **'정확한 문서(Spec)와 완벽한 문맥(Context)을 AI에게 전달하는 것'**에 집중하는 경량 접근입니다.

---

## 개념 정리

이 가이드를 시작하기 전에 두 개의 핵심 개념을 먼저 이해하겠습니다.

### Repomix가 무엇인지

개발 프로젝트는 수십, 수백 개의 파일로 구성되어 있습니다. Claude에게 "이 프로젝트의 로그인 기능을 수정해줘"라고 말해도, Claude가 어떤 파일에 무엇이 있는지 모르면 제대로 도움을 줄 수 없습니다.

**Repomix는 프로젝트의 모든 파일을 하나의 파일로 묶어서 AI에게 전달하는 도구입니다.**

예시로 설명하면, 책상 위에 파일 수십 장이 散만해 있을 때와 정리된 책자 한 권이 있을 때의 차이입니다. 같은 정보라도 정리된 형태가 훨씬 빠르게 파악됩니다.

```
프로젝트폴더/
├── src/
│   ├── auth.ts          ─┐
│   ├── user.ts           │
│   ├── api.ts            ├── Repomix 실행 후 ──→ repomix-output.xml (하나의 파일)
│   └── database.ts      │
├── config/               │
│   └── env.ts           ─┘
└── package.json
```

### Open Spec이 무엇인지

"코드를 작성하기 전에, 무엇을 만들고 싶은지를 정리된 문서(Spec)로 먼저 작성한다"는 방법론입니다.

Claude에게 말로만 설명하면 방향이 흔들리고, 구현이 진행될수록 처음과 달라지는 일이 생깁니다. **SPEC.md 파일이 있으면 Claude는 이 파일을 기준으로 작업하므로 방향이 안정됩니다.**

### 두 도구의 역할 차이

| 도구 | 역할 | 쉬운 설명 |
|------|------|-----------|
| **Spec Builder** | "무엇을 만들지" 정의 | 건축 설계도 작성 |
| **Repomix** | "현재 프로젝트가 어떤지" 정리 | 현장 사진과 기존 구조 파악 |

둘을 합치면, **설계도(SPEC.md)와 현장 파악(repomix-output.xml)이 완비된 상태**에서 Claude가 작업을 시작합니다.

---

## 이 가이드의 표기법

07-bmad-guide와 동일하게, 아래 두 곳에서 명령을 입력합니다.

| 표기 | 의미 |
|------|------|
| **[터미널]** | VS Code 내장 터미널, iTerm2, Windows Terminal 등에서 입력 |
| **[Claude Code]** | Claude Code가 실행된 후, 그 채팅창에서 입력 |

---

## 전체 워크플로우 한눈에 보기

```
[1단계] Repomix 설치
    ↓
[2단계] spec-builder.md 생성 (복사/붙여넣기)
    ↓
[3단계] Claude Code와 대화하며 SPEC.md 자동 생성
    ↓
[4단계] Repomix로 프로젝트 문맥 패킹
    ↓
[5단계] Claude Code에게 구현 요청
    ↓
[6단계] 반복 (수정 → 검증 → 다음 기능)
```

---

## 1단계: Repomix 설치

**[터미널]** 프로젝트 폴더에서 아래 명령을 실행합니다.

```bash
npm install --save-dev repomix
```

설치 완료 확인:

```bash
npx repomix --version
```

숫자가 출력되면 설치가 완료되었습니다. 이후 명령은 모두 `npx repomix`로 실행합니다.

> **Node.js가 없는 경우**: [Node.js 공식 사이트](https://nodejs.org)에서 최신 버전(v20 이상)을 먼저 설치해야 합니다.

---

## 2단계: spec-builder.md 생성

이 파일은 Claude를 **'기획자 모드'**로 전환시키는 스위치입니다. 이 파일이 있으면 Claude가 사용자에게 질문을 해가며 SPEC.md를 자동으로 작성합니다.

**[터미널 또는 에디터]** 프로젝트 루트 폴더에 `spec-builder.md` 파일을 생성하고, 아래 내용을 **그대로** 붙여넣기 합니다.

```markdown
# Role: Senior Technical Product Manager (Spec Builder)

## Goal
사용자와 대화하여 프로젝트의 요구사항을 명확히 하고, 최종적으로 완벽한 `SPEC.md` 파일을 생성하는 것.

## Process
1. **Interview Phase (대화 단계)**:
   - 사용자가 만들고 싶은 기능이나 서비스에 대해 질문합니다.
   - 한 번에 너무 많은 질문을 하지 말고, 2~3개씩 핵심을 파고드세요.
   - 필수 확인 사항:
     - 프로젝트 목표 (Goal)
     - 기술 스택 (Tech Stack)
     - 주요 기능 목록 (Functional Requirements)
     - 제약 사항 (Constraints - 인증 방식, 데이터 구조 등)

2. **Draft Phase (초안 작성)**:
   - 대화가 충분하다고 판단되거나 사용자가 "정리해줘"라고 하면, 수집된 정보를 바탕으로 `SPEC.md` 내용을 작성합니다.

3. **File Generation (파일 생성)**:
   - 사용자가 승인하면 최종 내용을 `SPEC.md` 파일로 저장합니다.

## Output Format (SPEC.md 템플릿)
```markdown
# Project Specification: {Project Name}

## 1. Goal
- 핵심 목표를 한 문장으로 기술

## 2. Context
- **현재 상황**: 프로젝트의 현재 상태
- **관련 파일**: 주요 파일 경로 나열

## 3. Tech Stack
- Language, Framework, DB, Build Tool 등

## 4. Requirements (Checklist)
- [ ] 기능 요구사항 1
- [ ] 기능 요구사항 2

## 5. Constraints
- 제약 사항 나열

## 6. Implementation Plan
1. 단계 1
2. 단계 2
```

## Instruction
지금부터 "무엇을 만들고 싶으신가요?"라고 물으며 인터뷰를 시작하세요. 한국어로 진행하세요.
```

> **파일을 만드는 것이 불편하시면**, Claude Code가 실행된 후 3단계에서 직접 요청할 수 있습니다.

---

## 3단계: Claude Code와 대화하며 SPEC.md 생성

여기서부터 **Claude와 대화하면서** SPEC.md가 자동으로 완성됩니다. 직접 문서를 작성할 필요가 없습니다.

**[터미널]** 먼저 프로젝트 폴더에서 Claude Code를 실행합니다.

```bash
claude
```

**[Claude Code]** 아래를 입력합니다.

```
spec-builder.md 파일 내용을 읽고, 지시대로 나랑 인터뷰를 진행해서 SPEC 문서를 만들어줘.
```

> **spec-builder.md를 만들지 않은 경우**, 위 대신 아래를 입력하면 됩니다:
>
> ```
> 지금부터 나에게 질문을 해가며, SPEC.md 파일을 만들어줘. 질문은 2~3개씩, 프로젝트 목표 → 기술 스택 → 주요 기능 → 제약 사항 순서로 진행하세요. 한국어로 진행해주세요.
> ```

### 대화 예시

이후 Claude가 질문을 시작합니다. 아래와 같은 흐름으로 진행됩니다:

```
Claude: "안녕하세요! 어떤 서비스를 만들고 싶으신가요?"
사용자: "유튜브 영상을 자동으로 분석하고 요약 블로그 글을 만드는 시스템"

Claude: "좋습니다. 영상 소스는 특정 채널만 대상인가요, 아니면 URL을 직접 입력하는 방식인가요?
         그리고 블로그 글의 발행 플랫폼은 정해져 있나요?"
사용자: "URL을 직접 입력하고, 블로그는 Wordpress에 자동 발행"

Claude: "기술적으로 어떤 환경을 생각하고 계신가요? ...
```

Claude가 충분한 정보를 수집하면 자동으로 초안을 작성합니다.

```
Claude: "아래가 SPEC.md 초안입니다. 확인해보고 수정할 내용이 있으면 말씀해 주세요..."
```

초안을 확인한 후:

```
Claude: "이 내용으로 SPEC.md 파일을 생성할까요?"
사용자: "네, 만들어줘"
```

이제 프로젝트 루트에 `SPEC.md` 파일이 생깁니다.

---

## 4단계: Repomix로 프로젝트 문맥 패킹

SPEC.md가 완성되었으면, 현재 프로젝트 코드와 합쳐서 AI에게 넘길 준비를 합니다.

### 4-1. 설정 파일 생성 (초기 설정)

처음 사용하는 경우, 먼저 설정 파일을 생성합니다.

**[터미널]** 프로젝트 폴더에서 실행합니다.

```bash
npx repomix --init
```

실행 후 `repomix.config.json` 파일이 생깁니다.

### 4-2. 설정 파일 수정

생성된 `repomix.config.json`을 열고, 아래 내용으로 교체합니다.

```json
{
  "output": {
    "style": "xml",
    "filePath": "repomix-output.xml"
  },
  "ignore": {
    "useGitignore": true,
    "customPatterns": [
      "**/*.lock",
      "**/node_modules/**",
      "**/build/**",
      "**/dist/**",
      "**/.idea/**",
      "**/*.log",
      "repomix-output.xml"
    ]
  }
}
```

각 설정의 의미:

| 설정 | 의미 |
|------|------|
| `style: "xml"` | 출력 파일 형식. XML이 AI에게 가장 잘 정리된 형태입니다 |
| `useGitignore: true` | `.gitignore`에 적힌 파일은 자동으로 제외 |
| `customPatterns` | 추가로 제외할 파일 패턴. `node_modules`, `build` 등 불필요한 폴더 제외 |

### 4-3. 패킹 실행

**[터미널]** 프로젝트 폴더에서 실행합니다.

```bash
npx repomix
```

실행 후 두 가지 정보가 출력됩니다:

- **생성된 파일 경로**: `repomix-output.xml`
- **토큰 수**: 예시 `Total tokens: 45,320` — 이 숫자가 클수록 AI에게 더 많은 정보를 전달합니다.

> **토큰 수가 너무 크면 (예: 200,000 이상)**, 프로젝트 전체 대신 관련 폴더만 패킹할 수 있습니다:
>
> ```bash
> npx repomix --include "src/auth/**,SPEC.md"
> ```
>
> 이렇게 하면 `src/auth/` 폴더와 `SPEC.md`만 패킹됩니다.

---

## 5단계: Claude Code에게 구현 요청

패킹된 문맥과 SPEC.md를 Claude에게 주고, 구현을 시작합니다.

**[Claude Code]** 아래를 입력합니다. 파일명을 언급하면 Claude Code가 자동으로 읽습니다.

```
repomix-output.xml과 SPEC.md를 읽고, SPEC.md의 Implementation Plan 1번 단계를 구현해줘. 코드 작성 전에 먼저 계획을 설명해주세요.
```

> **터미널에서 한 줄로도 가능합니다** (Claude Code가 실행되어 있지 않은 경우):
>
> ```bash
> cat repomix-output.xml | claude "SPEC.md의 Implementation Plan 1번 단계를 구현해줘"
> ```

---

## 6단계: 반복 (수정 → 검증 → 다음 기능)

구현이 완료되면 아래 루프를 반복합니다.

### 검증 요청

구현된 코드가 SPEC.md의 요구사항을 만족하는지 확인합니다.

**[Claude Code]**

```
구현된 코드가 SPEC.md의 Requirements를 모두 만족하는지 검증해줘.
충족되지 않은 항목이 있으면 구체적으로 알려주세요.
```

### SPEC.md 업데이트

기능이 완료되면 SPEC.md의 체크박스를 체크합니다.

```markdown
## 4. Requirements (Checklist)
- [x] API Endpoint 구현        ← 완료 시 [x]로 변경
- [ ] 테스트 코드 작성          ← 아직 진행 중
```

### 다음 기능으로 이동

```
SPEC.md의 Requirements에서 아직 완료되지 않은 항목 중 다음 것을 구현해줘.
```

---

## Repomix 자주 쓰는 명령어

| 명령어 | 무엇을 하는가 |
|--------|--------------|
| `npx repomix` | 설정 파일 기준으로 전체 프로젝트 패킹 |
| `npx repomix --init` | 설정 파일 (`repomix.config.json`) 초기 생성 |
| `npx repomix --include "src/**"` | `src/` 폴더만 패킹 |
| `npx repomix --exclude "**/*.test.ts"` | 테스트 파일 제외하고 패킹 |
| `npx repomix --output my-pack.xml` | 출력 파일명 지정 |

---

## BMAD와 비교

이미 BMAD를 사용하고 계신 분께 참고용입니다.

| 구분 | BMAD | Open Spec + Repomix |
|------|------|---------------------|
| 설치 복잡도 | 높음 (npx bmad-method install) | 낮음 (repomix 설치 + 파일 1개) |
| 스펙 작성 | 에이전트(PM, Analyst)가 질문하며 작성 | spec-builder.md로 동일한 대화형 작성 |
| 컨텍스트 관리 | bmad-method flatten | repomix |
| 적합한 상황 | 팀 단위, 대규모 프로젝트 | 개인 개발자, 빠른 프로토타입 |
| 개발 사이클 | Story → Dev → QA (반복) | SPEC.md 기준 구현 → 검증 (반복) |

두 방법은 **경쟁 관계가 아닙니다.** BMAD가 너무 무거운 느낌이면 이 조합을 사용하고, 프로젝트가 커지면 BMAD로 전환하는 것도 가능합니다.

---

## 폴더 구조 참고

Open Spec + Repomix를 적용하면 프로젝트가 다음과 같은 구조로 정리됩니다.

```
프로젝트폴더/
├── SPEC.md                      ← 프로젝트 스펙 (진실의 기준)
├── spec-builder.md              ← Spec 대화형 생성을 위한 프롬프트 파일
├── repomix.config.json          ← Repomix 설정 파일
├── repomix-output.xml           ← 패킹된 컨텍스트 파일 (자동 생성)
├── .gitignore                   ← repomix-output.xml 추가 권장
├── src/
│   └── ...                      ← 실제 소스 코드
└── package.json
```

> **.gitignore에 추가 권장**: `repomix-output.xml`은 자동 생성 파일이므로 저장소에 넣을 필요가 없습니다.
>
> ```
> repomix-output.xml
> ```

---

## 빠른 시작 체크리스트

| # | 무엇을 해야 하는가 | 어디서 |
|---|-------------------|--------|
| 1 | `npm install --save-dev repomix` 실행 | 터미널 |
| 2 | 프로젝트 폴더에 `spec-builder.md` 생성 | 에디터 |
| 3 | `claude` 실행 | 터미널 |
| 4 | `spec-builder.md 파일을 읽고 인터뷰 진행해줘` 입력 | Claude Code |
| 5 | 대화하며 SPEC.md 생성 완료 | Claude Code |
| 6 | `npx repomix --init` 및 설정 파일 수정 | 터미널 |
| 7 | `npx repomix` 실행 | 터미널 |
| 8 | `repomix-output.xml과 SPEC.md를 읽고 구현 요청` 입력 | Claude Code |
| 9 | 구현 완료 후 검증 및 반복 | Claude Code |