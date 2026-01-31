# BMAD 가이드

> 참고: [BMAD-AT-CLAUDE GitHub 저장소](https://github.com/24601/BMAD-AT-CLAUDE)

BMAD(Breakthrough Method for Agile AI-Driven Development)는 AI 에이전트를 활용한 체계적인 소프트웨어 개발 프레임워크입니다.
전문화된 에이전트들이 각각의 역할을 맡아 협력하여 개발 과정을 진행합니다.

BMAD가 기존 AI 코딩과 다른 것은, **계획(Planning)**과 **개발(Development)**을 명확히 분리하여 진행하기 때문입니다.
AI가 계획을 잘못 세우면 개발 단계에서 방향이 무너지는 일이 자주 생기는데, BMAD는 이 문제를 구조적으로 해결합니다.

---

## 핵심 철학

- **자연어 중심**: 모든 소통과 작업 정의가 마크다운과 자연어로 진행됩니다.
- **린 컨텍스트**: 각 에이전트는 필요한 정보만 로드하여 성능 저하를 방지합니다.
- **단계 분리**: 계획과 개발을 나누어 진행하여 컨텍스트 손실을 방지합니다.
- **반복 가능한 워크플로우**: 템플릿과 체클리스트를 활용하여 동일한 과정을 반복할 수 있도록 합니다.

---

## 1단계: 설치 및 환경 설정

### 필수 조건

- Node.js v20 이상

### 기본 설치

```bash
npx bmad-method install
```

이 단일 명령어로 신규 설치, 업그레이드, Expansion Pack 추가가 모두 가능합니다.

### Claude Code와의 통합 설치

BMAD를 Claude Code에서 직접 사용하려면 추가 설정이 필요합니다.

```bash
# 저장소 클론
git clone https://github.com/24601/BMAD-AT-CLAUDE.git
cd BMAD-AT-CLAUDE/bmad-claude-integration

# 종속성 설치
npm install

# 로컬 설치 실행
npm run install:local
```

설치 완료 후 Claude Code에서 사용 가능한 커맨드:

| 커맨드 | 역할 |
|--------|------|
| `/bmad-pm` | Project Manager 에이전트 호출 |
| `/bmad-architect` | Architect 에이전트 호출 |
| `/bmad-dev` | Developer 에이전트 호출 |
| `/bmad-sessions` | 활성 세션 목록 조회 |
| `/bmad-switch <번호>` | 세션 전환 |

---

## 2단계: 에이전트 구성 파악

BMAD는 10개의 전문화된 에이전트로 구성되어 있습니다.

### 계획 단계 에이전트

| 에이전트 | 역할 |
|----------|------|
| **Analyst** | 시장 조사, 프로젝트 브리프 작성, 요구사항 분석 |
| **PM** | PRD(제품 요구사항 문서) 작성 및 관리 |
| **UX Expert** | 프론트엔드 스펙, UX 설계 |
| **Architect** | 시스템 아키텍처 설계 및 문서화 |
| **PO** | 모든 아티팩트 검증, 체클리스트 확인, 문서 분할(sharding) |

### 개발 단계 에이전트

| 에이전트 | 역할 |
|----------|------|
| **SM (Scrum Master)** | 유저 스토리 생성 및 스프린트 관리 |
| **Dev** | 스토리 구현 및 테스트 |
| **QA** | 구현 리뷰, 리팩토링, 회귀 테스트 |

### 특수 에이전트

| 에이전트 | 역할 |
|----------|------|
| **BMad Master** | BMAD 방법론 전문가 |
| **BMad Orchestrator** | 에이전트 라우팅 및 워크플로우 전체 관리 |

### Orchestrator 커맨드

Orchestrator와 대화할 때 `*` 접두사를 사용합니다.

```
*help                → 사용 가능한 커맨드 목록
*agent               -> 에이전트 목록 조회
*agent pm            -> PM 에이전트로 전환
*agent architect     -> Architect 에이전트로 전환
*workflow            -> 워크플로우 시작
*status              -> 현재 상태 조회
```

---

## 3단계: 프로젝트 유형 구분

BMAD는 두 가지 프로젝트 유형을 구분합니다.

| 구분 | Greenfield | Brownfield |
|------|-----------|-----------|
| **정의** | 새로운 프로젝트 생성 | 기존 프로젝트 유지보수/확장 |
| **주요 작업** | PRD → 아키텍처 → 개발 | 문서화 → PRD → 통합 개발 |
| **워크플로우** | greenfield-fullstack / ui / service | brownfield-fullstack / ui / service |

---

## 실습 1: Greenfield 프로젝트 (신규 프로젝트)

> 샘플 프로젝트: **Todo List App** - 직장인용 업무 관리 앱

### Step 1. Analyst → 프로젝트 브리프

프로젝트의 목적과 맥락을 정리하는 첫 번째 단계입니다.

```
@analyst
프로젝트 브리프를 작성해주세요.

프로젝트명: Todo List App
대상 사용자: 직장인들이 매일 업무를 관리하는 앱
핵심 기능: 할 일 추가/수정/삭제, 카테고리 분류, 우선순위 설정
경쟁 참고: Todoist, TickTick
```

**생성된 파일**: `docs/project-brief.md`

### Step 2. PM → PRD 작성

프로젝트 브리프를 기반으로 상세한 제품 요구사항 문서를 생성합니다.

```
@pm
PRD를 작성해주세요.
참조 문서: docs/project-brief.md
```

**생성된 파일**: `docs/prd.md`

PRD는 다음과 같은 구조로 작성됩니다:
- 제품 목표
- 사용자 페르소나
- 기능 요구사항 (에픽 단위)
- 비기능 요구사항
- 우선순위 정의

### Step 3. UX Expert → 프론트엔드 스펙

UI/UX 설계 방향을 정리합니다.

```
@ux-expert
프론트엔드 스펙을 작성해주세요.
참조 문서: docs/prd.md
디자인 가이드라인: 간결하고 깔끔한 미니멀 디자인
```

**생성된 파일**: `docs/front-end-spec.md`

### Step 4. Architect → 아키텍처 설계

기술 스택과 시스템 구조를 설계합니다.

```
@architect
아키텍처를 설계해주세요.
참조 문서: docs/prd.md, docs/front-end-spec.md

프론트엔드: React + TypeScript
백엔드: Node.js + Express
데이터베이스: PostgreSQL
인증: JWT
```

**생성된 파일**: `docs/architecture.md`

아키텍처 문서에는 다음이 포함됩니다:
- 기술 스택 결정 및 근거
- 시스템 구성도
- 데이터베이스 스키마
- API 엔드포인트 정의
- 코딩 표준

### Step 5. PO → 검증 및 문서 분할

PO가 모든 문서를 검증한 후, 에픽별로 분할합니다.

```
@po
체클리스트를 실행하여 모든 문서를 검증해주세요.
```

검증 완료 후 문서 분할:

```
@po
shard docs/prd.md
shard docs/architecture.md
```

분할 결과:

```
docs/
├── prd/
│   ├── epic-1-core-todo-management.md
│   ├── epic-2-category-priority.md
│   └── epic-3-user-auth.md
└── architecture/
    ├── coding-standards.md
    ├── tech-stack.md
    └── source-tree.md
```

**분할이 중요한 이유**: Dev 에이전트가 전체 문서를 읽는 대신, 필요한 에픽만 참조하면 컨텍스트가 작아져서 응답이 빠르고 정확해집니다.

### Step 6. SM → 스토리 생성

스크럼 마스터가 개발할 스토리를 생성합니다.

```
@sm
*draft
```

**생성된 파일**: `docs/stories/1.1.add-todo-item.md`

스토리 파일의 구조:

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

스토리를 승인합니다:

```
@sm
스토리 상태를 Draft에서 Approved로 변경해주세요.
```

### Step 7. Dev → 스토리 구현

새로운 채팅 세션을 열고 Dev 에이전트를 로드합니다.

```
@dev
*develop-story docs/stories/1.1.add-todo-item.md
```

Dev 에이전트는 스토리의 Acceptance Criteria와 Tasks를 기준으로 구현을 진행하고, 완료 후 스토리 파일에 결과를 기록합니다.

### Step 8. QA → 리뷰

새로운 세션에서 QA 에이전트를 로드합니다.

```
@qa
*review docs/stories/1.1.add-todo-item.md
```

QA는 구현 내용을 검토하고, 필요시 리팩토링을 제안합니다. 이후 스토리 상태가 `Done`으로 변경됩니다.

### Step 9. 커밋 및 반복

```bash
git add .
git commit -m "feat: 할 일 추가 기능 구현 (#1.1)"
git push
```

**Step 6 ~ 9를 모든 스토리가 완료될 때까지 반복합니다.**

---

## 실습 2: Brownfield 프로젝트 (기존 프로젝트)

> 샘플 시나리오: **기존 회원가입/로그인 앱에 소셜 로그인 추가**

### 접근 방식 선택

| 상황 | 권장 접근 |
|------|-----------|
| 코드베이스가 크고 복잡 | PRD-First |
| 프로젝트를 잘 알고 있음 | PRD-First |
| 코드베이스를 처음 본다 | Document-First |
| 단일 에픽 추가 | `create-brownfield-epic` |
| 간단한 버그 수정 | `create-brownfield-story` |

### Step 1. 코드베이스 평탄화 (Flatten)

기존 프로젝트 전체를 AI가 읽을 수 있는 단일 파일로 변환합니다.

```bash
npx bmad-method flatten
```

- 프로젝트를 XML 형식의 단일 파일로 변환
- `.gitignore` 패턴을 자동으로 존중
- 토큰 수 카운팅과 진행 상황 표시 기능 포함

### Step 2a. Document-First (처음 본 프로젝트일 때)

기존 시스템을 먼저 문서화합니다.

```
@architect
*document-project
```

**생성된 파일**: `docs/brownfield-architecture.md`

이 단계에서 생성되는 내용:
- 현재 시스템 구조
- 기술 스택 정리
- 기존 패턴과 규칙
- 기술 부채 항목

### Step 2b. PRD-First (잘 아는 프로젝트일 때)

바로 요구사항 작성을 시작합니다.

```
@pm
*create-brownfield-prd

기존 기능: 이메일/비밀번호 회원가입, 로그인
추가할 기능: Google, Kakao OAuth 소셜 로그인
영향받는 모듈: src/auth/, src/user-profile/
호환 조건: 기존 회원 계정과 소셜 로그인 계정 연동 가능
```

**생성된 파일**: `docs/brownfield-prd.md`

### Step 3. Architect → 통합 아키텍처

기존 시스템과 어떻게 연동할지 아키텍처를 설계합니다.

```
@architect
*create-brownfield-architecture
참조 문서: docs/brownfield-prd.md
```

**생성된 파일**: `docs/brownfield-architecture.md` (갱신)

통합 아키텍처에는 다음이 포함됩니다:
- 기존 인증 흐름과의 연관 지점
- OAuth 콜백 처리 설계
- 계정 연동 로직
- 회귀 테스트 범위

### Step 4. PO → 검증

```
@po
*execute-checklist-po
통합 안전성과 기존 기능과의 호환성을 검증해주세요.
```

### Step 5. 문서 분할

```
@po
shard docs/brownfield-prd.md
shard docs/brownfield-architecture.md
```

### Step 6~9. 개발 사이클

Greenfield와 동일하게 SM → Dev → QA 사이클을 반복합니다.

---

## 설정 파일: core-config.yaml

프로젝트 루트에 `core-config.yaml`을 생성하여 BMAD 동작을 조정합니다.

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
project/
├── core-config.yaml              ← BMAD 설정 파일
├── docs/
│   ├── project-brief.md          ← 프로젝트 브리프
│   ├── prd.md                    ← 제품 요구사항 (원본)
│   ├── architecture.md           ← 아키텍처 (원본)
│   ├── front-end-spec.md         ← 프론트엔드 스펙
│   ├── prd/                      ← PRD 분할 폴더
│   │   ├── epic-1-core-features.md
│   │   └── epic-2-user-management.md
│   ├── architecture/             ← 아키텍처 분할 폴더
│   │   ├── coding-standards.md
│   │   ├── tech-stack.md
│   │   └── source-tree.md
│   └── stories/                  ← 스토리 폴더
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
| Education | 커리큘럼 아키텍트, 강의 설계가 |
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
[Greenfield]
Analyst → PM → UX Expert → Architect → PO (검증+분할) → SM → Dev → QA → 반복

[Brownfield]
[Flatten] → Architect/PM (문서화/PRD) → PO (검증+분할) → SM → Dev → QA → 반복
```

### 주요 커맨드

| 상황 | 커맨드 |
|------|--------|
| 신규 프로젝트 시작 | `*workflow greenfield-fullstack` |
| 기존 프로젝트 유지보수 | `*workflow brownfield-fullstack` |
| 코드베이스 평탄화 | `npx bmad-method flatten` |
| 스토리 생성 | `@sm` → `*draft` |
| 스토리 구현 | `@dev` → `*develop-story <파일경로>` |
| 스토리 리뷰 | `@qa` → `*review <파일경로>` |
| 문서 검증 | `@po` → `*execute-checklist-po` |
| 문서 분할 | `@po` → `shard <파일경로>` |
| 시스템 문서화 | `@architect` → `*document-project` |
| Brownfield PRD | `@pm` → `*create-brownfield-prd` |

### 디버그

```bash
# 디버그 모드 실행
DEBUG=bmad:* npm run install:local

# 테스트 실행
npm test
npm run test:ai
```
