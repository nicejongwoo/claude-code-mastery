# BMAD 가이드

> 참고: [BMAD-AT-CLAUDE GitHub 저장소](https://github.com/24601/BMAD-AT-CLAUDE)

BMAD(Breakthrough Method for Agile AI-Driven Development)는 AI 에이전트를 활용한 체계적인 소프트웨어 개발 프레임워크입니다.
전문화된 에이전트들이 각각의 역할을 맡아 협력하여 개발 과정을 진행합니다.

BMAD가 기존 AI 코딩과 다른 것은, **계획(Planning)**과 **개발(Development)**을 명확히 분리하여 진행하기 때문입니다.
AI가 계획을 잘못 세우면 개발 단계에서 방향이 무너지는 일이 자주 생기는데, BMAD는 이 문제를 구조적으로 해결합니다.

---

## 이 가이드의 표기법

아래 두 곳에서 명령을 입력합니다. 각 단계마다 어디에서 실행하는지가 표시되어 있습니다.

| 표기 | 의미 |
|------|------|
| **[터미널]** | VS Code 내장 터미널, iTerm2, Windows Terminal 등에서 입력 |
| **[Claude Code]** | Claude Code가 실행된 후, 그 채팅창에서 입력 |

---

## 핵심 철학

- **자연어 중심**: 모든 소통과 작업 정의가 마크다운과 자연어로 진행됩니다.
- **린 컨텍스트**: 각 에이전트는 필요한 정보만 로드하여 성능 저하를 방지합니다.
- **단계 분리**: 계획과 개발을 나누어 진행하여 컨텍스트 손실을 방지합니다.
- **반복 가능한 워크플로우**: 템플릿과 체클리스트를 활용하여 동일한 과정을 반복할 수 있도록 합니다.

---

## 1단계: 설치 및 환경 설정

순서대로 따라가면 됩니다.

### 1-1. Node.js 설치 확인

**[터미널]** 아래 명령을 실행합니다.

```bash
node --version
```

`v20.0.0` 이상의 숫자가 표시되면 준비 완료입니다.
숫자가 표시되지 않거나 v20 미만이면 [Node.js 공식 사이트](https://nodejs.org)에서 최신 버전을 설치합니다.

### 1-2. 프로젝트 폴더 준비

**[터미널]** 새 프로젝트를 만드는 경우, 아래를 실행합니다.

```bash
mkdir my-project
cd my-project
git init
```

기존 프로젝트가 있으면 해당 폴더로 이동하면 됩니다.
이후 모든 명령은 **이 프로젝트 폴더** 안에서 실행합니다.

### 1-3. BMAD 프레임워크 설치

**[터미널]** 프로젝트 폴더에서 실행합니다.

```bash
npx bmad-method install
```

### 1-4. Claude Code와의 통합 설치

BMAD를 Claude Code에서 직접 사용하려면 아래 단계를 추가로 진행합니다.

**[터미널]** 순서대로 실행합니다.

```bash
# BMAD 저장소를 프로젝트 폴더의 옆에 클론
git clone https://github.com/24601/BMAD-AT-CLAUDE.git

# 통합 폴더로 이동
cd BMAD-AT-CLAUDE/bmad-claude-integration

# 종속성 설치
npm install

# Claude Code에 등록
npm run install:local
```

### 1-5. 프로젝트 폴더로 복귀하고 Claude Code 시작

**[터미널]** 원래 프로젝트 폴더로 돌아와서 Claude Code를 실행합니다.

```bash
cd ../my-project
claude
```

Claude Code가 실행되면 아래 커맨드로 에이전트를 호출할 수 있습니다.

| 커맨드 | 역할 |
|--------|------|
| `/bmad-pm` | Project Manager 에이전트 호출 |
| `/bmad-architect` | Architect 에이전트 호출 |
| `/bmad-dev` | Developer 에이전트 호출 |
| `/bmad-sessions` | 활성 세션 목록 조회 |
| `/bmad-switch <번호>` | 세션 전환 |

설치가 완료되면 아래 **실습 1** 또는 **실습 2**로 넘어가면 됩니다.

---

## 2단계: 에이전트 구성 파악

BMAD는 10개의 전문화된 에이전트로 구성되어 있습니다.
**각 에이전트를 직접 호출할 필요는 없습니다.** 자연어로 요청하면 오케스트레이터가 적절한 에이전트로 자동으로 라우팅합니다.

### 계획 단계 에이전트

| 에이전트 | 역할 | 언제 사용되는가 |
|----------|------|----------------|
| **Analyst** | 시장 조사, 프로젝트 브리프 작성 | 프로젝트 시작 시 가장 먼저 |
| **PM** | PRD(제품 요구사항 문서) 작성 | Analyst 작업 완료 후 |
| **UX Expert** | 프론트엔드 스펙, UX 설계 | PRD 완료 후 |
| **Architect** | 시스템 아키텍처 설계 | UX 스펙 완료 후 |
| **PO** | 모든 문서 검증 및 분할 | 아키텍처 완료 후, 개발 시작 직전 |

### 개발 단계 에이전트

| 에이전트 | 역할 | 언제 사용되는가 |
|----------|------|----------------|
| **SM (Scrum Master)** | 유저 스토리 생성 | PO 검증 완료 후, 매번 스토리 작성 시 |
| **Dev** | 스토리 구현 | SM이 스토리를 승인한 후 |
| **QA** | 구현 리뷰 및 테스트 | Dev 구현 완료 후 |

### 특수 에이전트

| 에이전트 | 역할 |
|----------|------|
| **BMad Master** | BMAD 방법론 질문 답변 |
| **BMad Orchestrator** | 위의 에이전트들을 자동으로 라우팅 |

### Orchestrator 커맨드

특정 에이전트를 직접 호출하거나 커맨드를 실행할 때 `*` 접두사를 사용합니다.

```
*help                → 사용 가능한 커맨드 목록
*agent               → 에이전트 목록 조회
*agent pm            → PM 에이전트로 직접 전환
*agent architect     → Architect 에이전트로 직접 전환
*workflow            → 워크플로우 시작
*status              → 현재 상태 조회
```

---

## 3단계: 프로젝트 유형 구분

아래 두 가지 중 본인의 상황에 맞는 실습을 선택합니다.

| 내 상황 | 해당 실습 |
|---------|-----------|
| 새로운 프로젝트를 처음부터 만들고 싶다 | **실습 1** (Greenfield) |
| 기존 프로젝트에 기능을 추가하거나 수정하고 싶다 | **실습 2** (Brownfield) |

---

## 실습 1: Greenfield 프로젝트 (신규 프로젝트)

> 샘플 프로젝트: **Todo List App** - 직장인용 업무 관리 앱
>
> 아래 모든 단계에서 [Claude Code]로 표시된 것은 Claude Code 채팅창에 입력합니다.
> [터미널]로 표시된 것은 터미널 앱에서 실행합니다.
> 파일이 생성되는 위치는 모두 **프로젝트 폴더** 기준입니다.

---

### Step 1. 프로젝트 브리프 작성 (Analyst)

프로젝트의 목적과 맥락을 정리하는 첫 번째 단계입니다.

**[Claude Code]** 아래 내용을 그대로 복사하여 붙여넣기 합니다.

```
프로젝트 브리프를 작성해주세요.

프로젝트명: Todo List App
대상 사용자: 직장인들이 매일 업무를 관리하는 앱
핵심 기능: 할 일 추가/수정/삭제, 카테고리 분류, 우선순위 설정
경쟁 참고: Todoist, TickTick
```

오케스트레이터가 자동으로 Analyst 에이전트로 라우팅되어 작업을 수행합니다.

**생성되는 파일**: `프로젝트폴더/docs/project-brief.md`

---

### Step 2. PRD 작성 (PM)

프로젝트 브리프를 기반으로 제품 요구사항 문서를 생성합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
PRD를 작성해주세요.
참조 문서: docs/project-brief.md
```

**생성되는 파일**: `프로젝트폴더/docs/prd.md`

PRD에는 다음이 포함됩니다:
- 제품 목표
- 사용자 페르소나
- 기능 요구사항 (에픽 단위로 분류)
- 비기능 요구사항
- 우선순위 정의

---

### Step 3. 프론트엔드 스펙 작성 (UX Expert)

UI/UX 설계 방향을 정리합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
프론트엔드 스펙을 작성해주세요.
참조 문서: docs/prd.md
디자인 가이드라인: 간결하고 깔끔한 미니멀 디자인
```

**생성되는 파일**: `프로젝트폴더/docs/front-end-spec.md`

---

### Step 4. 아키텍처 설계 (Architect)

기술 스택과 시스템 구조를 설계합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.
(원하는 기술 스택을 직접 바꿔도 됩니다.)

```
아키텍처를 설계해주세요.
참조 문서: docs/prd.md, docs/front-end-spec.md

프론트엔드: React + TypeScript
백엔드: Node.js + Express
데이터베이스: PostgreSQL
인증: JWT
```

**생성되는 파일**: `프로젝트폴더/docs/architecture.md`

아키텍처 문서에는 다음이 포함됩니다:
- 기술 스택 결정 및 근거
- 시스템 구성도
- 데이터베이스 스키마
- API 엔드포인트 정의
- 코딩 표준

---

### Step 5. 문서 검증 및 분할 (PO)

앞서 생성된 모든 문서를 검증하고, 개발 단계에 맞게 분할합니다.

**[Claude Code]** 먼저 검증을 실행합니다.

```
체클리스트를 실행하여 모든 문서를 검증해주세요.
```

검증이 완료되면 문서를 분할합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
shard docs/prd.md
shard docs/architecture.md
```

**생성되는 폴더 구조**:

```
프로젝트폴더/docs/
├── prd/
│   ├── epic-1-core-todo-management.md
│   ├── epic-2-category-priority.md
│   └── epic-3-user-auth.md
└── architecture/
    ├── coding-standards.md
    ├── tech-stack.md
    └── source-tree.md
```

**분할이 중요한 이유**: Dev 에이전트가 전체 문서를 읽는 대신 필요한 에픽만 참조하면, 컨텍스트가 작아져서 응답이 빠르고 정확해집니다.

---

### Step 6. 스토리 생성 (SM)

여기서부터 **Step 6 ~ 9가 반복되는 구간**입니다.
각 스토리마다 이 과정을 한 번씩 반복하면 됩니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
스토리를 생성해주세요.
```

SM 에이전트가 자동으로 라우팅되어 첫 번째 스토리를 생성합니다.

**생성되는 파일**: `프로젝트폴더/docs/stories/1.1.add-todo-item.md`

생성된 스토리 파일의 구조:

```markdown
## Status: Draft

## Story
As a 직장인, I want 할 일을 추가하기를, so that 업무를 체계적으로 관리할 수 있도록 합니다.

## Acceptance Criteria
1. 제목과 설명을 입력할 수 있다
2. 카테고리를 선택할 수 있다
3. 우선순위를 설정할 수 있다
4. 저장 시 목록에 즉시 반영된다

## Tasks
- [ ] (AC-1) 입력 폼 UI 구현
- [ ] (AC-2) 카테고리 드롭다운 구현
- [ ] (AC-3) 우선순위 선택 구현
- [ ] (AC-4) API 호출 및 상태 반영

## Dev Notes
- Source Tree: src/features/todo/
- 관련 API: POST /api/todos
- 테스트: 유니트 테스트 + 통합 테스트 필수
```

스토리를 검토한 후, 내용이 괜찮으면 승인합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
스토리를 승인해주세요.
```

스토리 상태가 `Draft` → `Approved`로 변경됩니다.

---

### Step 7. 스토리 구현 (Dev)

개발자 에이전트가 스토리를 실제 코드로 구현합니다.
**새로운 세션**을 시작해야 합니다. (Dev 에이전트는 별도 컨텍스트에서 작업하는 것이 정상입니다.)

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
/bmad-dev
```

Dev 에이전트가 활성화되면, 아래를 입력합니다.

```
docs/stories/1.1.add-todo-item.md 스토리를 구현해주세요.
```

Dev 에이전트는 스토리의 Acceptance Criteria와 Tasks를 기준으로 코드를 작성하고, 완료 후 스토리 파일에 결과를 기록합니다.

---

### Step 8. 구현 리뷰 (QA)

QA 에이전트가 구현된 코드를 검토합니다.
마찬가지로 **새로운 세션**에서 진행합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
docs/stories/1.1.add-todo-item.md 스토리를 리뷰해주세요.
```

QA는 구현 내용을 검토하고 필요시 리팩토링을 제안합니다.
스토리 상태가 `Done`으로 변경되면 이 스토리는 완료입니다.

---

### Step 9. 커밋

구현된 코드를 저장합니다.

**[터미널]** 프로젝트 폴더에서 실행합니다.

```bash
git add .
git commit -m "feat: 할 일 추가 기능 구현 (#1.1)"
git push
```

---

### 반복

**Step 6 ~ 9를 다음 스토리마다 반복합니다.**
모든 에픽의 스토리가 완료될 때까지 같은 과정을 진행합니다.

```
SM(스토리 생성) → Dev(구현) → QA(리뷰) → 커밋 → 다음 스토리
```

---

## 실습 2: Brownfield 프로젝트 (기존 프로젝트)

> 샘플 시나리오: **기존 회원가입/로그인 앱에 소셜 로그인 추가**
>
> 기존 코드가 이미 있는 프로젝트에서 진행합니다.
> 해당 프로젝트 폴더에서 Claude Code를 실행한 후 아래를 따라가면 됩니다.

---

### 접근 방식 선택

상황에 따라 두 가지 경로 중 하나를 선택합니다.

| 내 상황 | 선택할 경로 | 이동 |
|---------|------------|------|
| 이 프로젝트를 잘 알고 있다 | PRD-First | Step 2b로 이동 |
| 이 프로젝트를 처음 본다 | Document-First | Step 2a로 이동 |
| 기능 하나만 추가하면 된다 | 단일 에픽 | Step 1 이후 바로 Step 2b |
| 버그 수정만 필요하다 | 단일 스토리 | Step 1 이후 바로 Step 6 |

---

### Step 1. 코드베이스 평탄화 (Flatten)

기존 프로젝트 코드를 AI가 읽을 수 있는 형태로 변환합니다.

**[터미널]** 프로젝트 폴더에서 실행합니다.

```bash
npx bmad-method flatten
```

이 명령이 완료되면 프로젝트 코드가 하나의 파일로 정리됩니다.
`.gitignore`에 적힌 파일들은 자동으로 제외됩니다.

---

### Step 2a. 시스템 문서화 (Document-First)

프로젝트를 처음 본 경우, 먼저 기존 시스템을 문서화합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
기존 프로젝트를 분석하여 문서화해주세요.
```

Architect 에이전트가 자동으로 라우팅되어 작업을 수행합니다.

**생성되는 파일**: `프로젝트폴더/docs/brownfield-architecture.md`

이 단계에서 정리되는 내용:
- 현재 시스템 구조
- 기술 스택 정리
- 기존 패턴과 규칙
- 기술 부채 항목

---

### Step 2b. PRD 작성 (PRD-First)

프로젝트를 잘 아는 경우, 바로 요구사항 작성을 시작합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.
(기존 기능과 추가할 기능을 직접 바꿔도 됩니다.)

```
기존 프로젝트에 새 기능을 추가하기 위한 PRD를 작성해주세요.

기존 기능: 이메일/비밀번호 회원가입, 로그인
추가할 기능: Google, Kakao OAuth 소셜 로그인
영향받는 모듈: src/auth/, src/user-profile/
호환 조건: 기존 회원 계정과 소셜 로그인 계정 연동 가능
```

**생성되는 파일**: `프로젝트폴더/docs/brownfield-prd.md`

---

### Step 3. 통합 아키텍처 설계 (Architect)

기존 시스템과 새 기능이 어떻게 연동되는지 아키텍처를 설계합니다.

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
기존 시스템과의 통합 아키텍처를 설계해주세요.
참조 문서: docs/brownfield-prd.md
```

**생성되는 파일**: `프로젝트폴더/docs/brownfield-architecture.md` (갱신)

통합 아키텍처에는 다음이 포함됩니다:
- 기존 인증 흐름과의 연관 지점
- OAuth 콜백 처리 설계
- 계정 연동 로직
- 회귀 테스트 범위

---

### Step 4. 문서 검증 (PO)

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
통합 안전성과 기존 기능과의 호환성을 검증해주세요.
```

---

### Step 5. 문서 분할

**[Claude Code]** 아래 내용을 복사하여 붙여넣기 합니다.

```
shard docs/brownfield-prd.md
shard docs/brownfield-architecture.md
```

---

### Step 6~9. 개발 사이클

실습 1과 동일하게 SM → Dev → QA → 커밋을 반복합니다.
실습 1의 **Step 6 ~ Step 9**를 그대로 따라가면 됩니다.

---

## 설정 파일: core-config.yaml

프로젝트 루트 폴더에 `core-config.yaml` 파일을 생성하면 BMAD의 동작을 조정할 수 있습니다.
처음에는 아래 내용을 **그대로** 복사하여 사용하면 됩니다.

**[터미널 또는 에디터]** 프로젝트 루트 폴더에 `core-config.yaml` 파일을 생성하고 아래 내용을 붙여넣기 합니다.

```yaml
markdownExploder: true

prd:
  prdFile: docs/prd.md
  prdVersion: v4
  prdSharded: true
  prdShardedLocation: docs/prd
  epicFilePattern: epic-{n}*.md

architecture:
  architectureFile: docs/architecture.md
  architectureVersion: v4
  architectureSharded: true
  architectureShardedLocation: docs/architecture

# Dev 에이전트가 항상 로드할 파일들
devLoadAlwaysFiles:
  - docs/architecture/coding-standards.md
  - docs/architecture/tech-stack.md
  - docs/architecture/source-tree.md

# 디버그 로그 위치
devDebugLog: .ai/debug-log.md

# 스토리 파일 저장 위치
devStoryLocation: docs/stories

# 슬래시 커맨드 접두사
slashPrefix: BMad
```

주요 설정 항목:

| 설정 | 설명 |
|------|------|
| `prdSharded` | PRD를 에픽별로 분할 저장 여부 |
| `architectureSharded` | 아키텍처 문서를 주제별로 분할 저장 여부 |
| `devLoadAlwaysFiles` | Dev 에이전트가 매번 참조해야 할 핵심 파일 목록 |
| `devDebugLog` | 디버그 정보를 저장할 파일 경로 |
| `slashPrefix` | Claude Code에서 사용할 커맨드 접두사 |

---

## 프로젝트 폴더 구조

BMAD를 사용하면 다음과 같은 폴더 구조가 생깁니다.

```
프로젝트폴더/
├── core-config.yaml              ← BMAD 설정 파일
├── docs/
│   ├── project-brief.md          ← 프로젝트 브리프 (Step 1)
│   ├── prd.md                    ← 제품 요구사항 원본 (Step 2)
│   ├── architecture.md           ← 아키텍처 원본 (Step 4)
│   ├── front-end-spec.md         ← 프론트엔드 스펙 (Step 3)
│   ├── prd/                      ← PRD 분할 폴더 (Step 5)
│   │   ├── epic-1-core-features.md
│   │   └── epic-2-user-management.md
│   ├── architecture/             ← 아키텍처 분할 폴더 (Step 5)
│   │   ├── coding-standards.md
│   │   ├── tech-stack.md
│   │   └── source-tree.md
│   └── stories/                  ← 스토리 폴더 (Step 6~)
│       ├── 1.1.add-todo-item.md
│       └── 1.2.edit-todo-item.md
├── .ai/
│   └── debug-log.md              ← 디버그 로그
└── src/                          ← 실제 소스 코드
```

---

## Expansion Packs

BMAD의 기본 소프트웨어 개발 기능을 다른 영역으로 확장하는 플러그인입니다.

### 기술적 Expansion Packs

| Pack | 주요 에이전트 |
|------|--------------|
| Game Dev (Phaser) | 게임 디자이너, 레벨 디자이너, 아트 디렉터 |
| Game Dev (Unity 2D) | Unity 전문 에이전트 |
| DevOps / Infrastructure | 클라우드 아키텍트, 보안 전문가, SRE |
| Data Science | 데이터 사이언티스트, ML 엔진어 |

### 비기술적 Expansion Packs

| Pack | 주요 에이전트 |
|------|--------------|
| Business Strategy | 전략 컨설턴트, 재무 분석가, 마케팅 전략가 |
| Creative Writing | 플롯 아키텍트, 캐릭터 심리학자, 편집가 |
| Education | 커리큼 아키텍트, 강의 설계가 |
| Personal Development | 라이프 코치, 목표 전략가, 습관 빌더 |

---

## 핵심 원칙 정리

1. **에이전트는 린하게** - Dev 에이전트는 코딩에 집중하기 위해 불필요한 컨텍스트를 로드하지 않습니다.
2. **계획 에이전트는 크게 가능** - 계획 단계 에이전트들은 복잡한 워크플로우를 처리할 수 있습니다.
3. **단일 책임** - 각 에이전트는 하나의 역할만 담당합니다.
4. **템플릿 재사용** - 동일한 형식의 문서는 템플릿을 통해 일관성을 유지합니다.
5. **명시적 종속성** - 에이전트 간의 의존관계를 명확히 정의합니다.
6. **단계별 분리** - 계획과 개발을 분리하여 컨텍스트 손실을 방지합니다.

---

## 빠른 참조

### 워크플로우 요약

```
[Greenfield - 신규 프로젝트]
Analyst → PM → UX Expert → Architect → PO (검증+분할) → SM → Dev → QA → 반복

[Brownfield - 기존 프로젝트]
[Flatten] → Architect/PM (문서화/PRD) → PO (검증+분할) → SM → Dev → QA → 반복
```

### 주요 커맨드

| 상황 | 어디서 | 무엇을 입력 |
|------|--------|------------|
| 신규 프로젝트 시작 | Claude Code | `*workflow greenfield-fullstack` |
| 기존 프로젝트 유지보수 | Claude Code | `*workflow brownfield-fullstack` |
| 코드베이스 평탄화 | 터미널 | `npx bmad-method flatten` |
| PM 에이전트 호출 | Claude Code | `/bmad-pm` |
| Architect 에이전트 호출 | Claude Code | `/bmad-architect` |
| Dev 에이전트 호출 | Claude Code | `/bmad-dev` |
| 문서 검증 | Claude Code | `체클리스트를 실행하여 문서를 검증해주세요.` |
| 문서 분할 | Claude Code | `shard <파일경로>` |
| 시스템 문서화 | Claude Code | `기존 프로젝트를 분석하여 문서화해주세요.` |

### 디버그

**[터미널]** 아래 명령으로 디버그 모드를 실행할 수 있습니다.

```bash
# 디버그 모드 실행
DEBUG=bmad:* npm run install:local

# 테스트 실행
npm test
npm run test:ai
```
