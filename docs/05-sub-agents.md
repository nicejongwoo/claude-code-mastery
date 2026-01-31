# 서브에이전트 및 메모리 관리

## 메모리 관리 (CLAUDE.md)

- 참고: https://code.claude.com/docs/ko/memory

## 서브에이전트 설정

Create new agent
![sub-agents_1.png](../images/sub-agents_1.png)

Project
![sub-agents_2.png](../images/sub-agents_2.png)

Generate with Claude
![sub-agents_3.png](../images/sub-agents_3.png)

Describe what this agent should do and when it should be used

---

세 가지 서브에이전트 구성:

1. **UI 에이전트** – UI만 구현하는 서브 에이전트를 생성한다. UI 구현이 필요할 때 호출한다.
2. **기능 구현 에이전트** – 이미 존재하는 UI 구현 서브 에이전트와 협력하여 기능을 구현하는 서브 에이전트이다. UI외의 기능을 구현할 때 호출한다.
3. **리뷰 에이전트** – UI 생성 서브 에이전트와 기능 구현 서브 에이전트의 협력 결과를 최종 리뷰하는 서브 에이전트이다. UI 생성과 기능 구현이 한 번 완료될 때 사용된다.

### 에이전트 설정 절차

![sub-agents_4.png](../images/sub-agents_4.png)
All tools > Continue

![sub-agents_5.png](../images/sub-agents_5.png)
Sonnet

![sub-agents_6.png](../images/sub-agents_6.png)
Automatic color

![sub-agents_7.png](../images/sub-agents_7.png)
result

![sub-agents_8.png](../images/sub-agents_8.png)
준비되면 얘기해 주세요.

### 서브 에이전트 설명 GPT 프롬프트 예시

```
나는 react native expo 프로젝트에서 claude code를 통해 바이브 코딩을 하고 있어. 3가지 서브 에이전트를 생성할건데, description을 생성해줘.
1. UI 관련 에이전트,
2. 기능 구현 에이전트,
3. 리뷰 에이전트

Describe what this agent should do and when it should be used (be comprehensive for best results)
```