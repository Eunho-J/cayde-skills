---
name: design-persuasion
description: Korean architecture and design decision review workflow. Use when the user says "설계설득" or asks to comprehensively review, negotiate, narrow, confirm, or reconcile a design. Build a complete hierarchical decision map up front, gate parent groups before drilling into children, independently review substantive questions with subagents, and continuously realign contradictions and dependencies as decisions change.
---

# 설계설득

설계를 한 문답씩 즉흥적으로 늘리지 말고, 먼저 전체 결정 지형을 만든 뒤 상위 묶음부터 승인받아 세부 결정을 내려간다.

## 핵심 원칙

- 첫 질문 전에 관련 목표, 기존 결정, 현재 구현, 제약, 비목표를 검토한다.
- 첫 패스에서 필요한 design decision과 architecture decision을 빠짐없이 후보화한 **계층형 전체 체크리스트**를 만든다.
- 새 항목을 매 턴 평면적으로 뒤에 붙이지 않는다. 가장 가까운 상위 결정 아래에 배치한다.
- 어느 depth에 진입하든 immediate child 후보를 먼저 한 번에 전개한 뒤 검토한다. child 하나를 묻고 다음 child를 새로 추가하는 방식으로 진행하지 않는다.
- **전체 child map을 미리 만드는 것과 사용자에게 결정을 묻는 것은 다르다.** 체크리스트에는 sibling 후보를 한 번에 전개하되, 대화에서는 active gate를 하나만 골라 한 번에 하나씩 묻는다.
- 사용자가 명시적으로 batch 결정을 원하지 않는 한 `A1-A7 일괄 승인` 같은 포괄 승인을 요청하지 않는다.
- 상위 묶음의 승인·거절·보류가 끝나기 전에 그 하위 leaf 결정을 확정하지 않는다.
- 상위 묶음이 승인되면 하위 항목을 더 깊게 펼쳐 근거, 대안, trade-off, 추천안, acceptance criteria를 검토한다.
- 상위 묶음이 거절되면 해당 subtree를 활성 체크리스트에서 제거하고, 의존 항목과 전체 목표를 다시 정렬한다. 거절 이유와 영향만 폐기 기록에 남긴다.
- 사용자의 직접 결정은 즉시 반영하되, 기존 확정 사항과 충돌하는지 먼저 검사한다.
- 모순을 발견하면 새 결정을 조용히 덮어쓰지 않는다. 충돌 항목을 표시하고 유지·수정·폐기 중 무엇을 할지 명시적으로 정리한다.
- 후보는 결정처럼 쓰지 않는다. `candidate`, `approved`, `resolved`를 구분한다.
- 사용자의 질문·가능성 탐색·검토 요청은 요구사항 확정이나 구현 허가가 아니다. 서브에이전트의 검토 통과도 사용자 설계 승인을 대신하지 않는다.
- 전체 지도를 보여줘야 할 때 임의로 한두 문장으로 축약하지 않는다. 이후 대화는 한 묶음 또는 강하게 연결된 묶음 단위로 집중한다.
- 질문만 던지지 않는다. 각 gate에 추천안과 그 이유를 함께 제시하되 최종 결정은 사용자가 내린다.

## 1. 전체 리뷰와 초기 결정 지도

다음 순서로 첫 패스를 수행한다.

1. 관련 문서, 저장소, 기존 체크리스트, 대화 결정, session note·journal, live tool/config surface, 실제 구현을 확인한다.
2. 사용자 목표, 성공 기준, 경계, 비목표, 이미 확정된 사항을 분리한다.
3. 아래 coverage axis를 전체적으로 검토한다. 관련 없는 축은 이유를 적고 `N/A` 처리한다.
   - 목적·성공 기준·범위
   - actor·역할·책임·소유권
   - system boundary·topology·component 관계
   - state·data·memory·lifecycle
   - interface·protocol·event·입출력
   - permission·security·trust boundary
   - concurrency·failure·recovery·consistency
   - performance·scale·capacity·cost
   - deployment·operation·observability·upgrade·rollback
   - migration·compatibility·reversibility·test·acceptance
4. 서로 같은 전제나 경계를 공유하는 결정들을 상위 group으로 묶는다.
5. 처음부터 최소 두 단계 깊이로 작성한다: `group → child decision`. 더 깊은 leaf는 group 승인 후 확장한다.
6. group 간 dependency, conflict, 결정 순서를 표시한다.
7. 전체 지도를 한 번 보여주고, 가장 선행성이 높은 group gate부터 검토한다.

전체 리뷰는 완전성을 위한 후보 지도다. 사용자가 승인하기 전까지 후보 group이나 child를 설계 사실로 취급하지 않는다.

## 2. 체크리스트 파일

관련 project의 `.codex/design-persuasion/` 아래에 Markdown 파일을 만들거나 갱신한다. project가 없으면 `$CODEX_HOME/design-persuasion/`을 사용한다.

```markdown
# 설계설득 결정 지도: <topic>

## 목표와 경계
- 목표:
- 성공 기준:
- 비목표:
- 기존 확정 사항:

## 전체 결정 트리

### G1. <decision group> [candidate|approved|rejected|deferred]
- 결정 gate: <이 묶음을 채택할지 판단할 문장>
- 필요 이유:
- 추천안:
- 주요 대안:
- dependency:
- conflict:
- 하위 검토 범위:
  - [ ] G1.1 <child decision> — blocked_by_parent_gate
  - [ ] G1.2 <child decision> — blocked_by_parent_gate

### G2. <decision group> [candidate]
...

## 확정 결정
- [x] D1. <decision>
  - 근거:
  - 영향:

## 정합성 감사
- 충돌:
- 무효화된 전제:
- 다시 검토할 dependency:
- coverage gap:
- reconciliation queue:

## 거절·보류·대체 기록
- <group/item> — rejected|deferred|superseded: <짧은 이유와 영향>

## 현재 review gate
- <이번에 확인할 group 또는 연결 묶음>
```

기존 체크리스트가 평면형이면 먼저 확정 사항을 보존한 채 계층형 decision tree로 재구성한다. 이미 해결된 항목을 다시 후보로 되돌리지 않는다.

## 3. Group gate

상위 group마다 다음을 한 번에 제시한다.

1. 이 group이 해결하는 문제와 system 영향 범위
2. 추천 구조와 추천 이유
3. 채택하지 않을 때의 대안 또는 coverage gap
4. 승인 후 검토할 child 목록
5. 다른 확정 group과의 dependency·충돌

사용자 판단을 다음처럼 반영한다.

- **승인:** group을 `approved`로 바꾸고 child 후보 전체를 체크리스트에 구체화한다. group 승인만으로 child를 자동 승인하지 않으며, 사용자에게는 첫 child 하나만 active gate로 묻는다.
- **거절:** active tree에서 subtree를 제거한다. dependent group을 재검토하고 목표 coverage가 비면 대체 group 후보를 만든다.
- **보류:** `deferred`로 격리하고, 다른 결정을 막는 dependency인지 표시한다.
- **분할:** group을 둘 이상의 sibling group으로 재구성하고 전체 순서를 다시 계산한다.

## 4. 승인된 Group의 심화 검토

승인된 group 안에서 각 child decision에 다음 정보를 붙인다.

- 결정해야 하는 정확한 질문
- 현재 사실과 증거
- 추천안
- 현실적인 대안
- trade-off와 failure mode
- 다른 결정에 미치는 영향
- acceptance criteria 또는 falsification condition

서로 강하게 결합되어 분리하면 모순이 생기는 child만 하나의 composite decision으로 묶는다. 이때도 여러 질문을 나열하지 말고 composite decision 하나로 묻는다. 독립 child는 반드시 하나씩 분리한다. 사용자가 명시적으로 원하는 경우에만 여러 child를 batch로 결정한다.

### 질문·제안에 대한 독립 서브에이전트 검토

설계설득 중 기술 사실, 적용 가능성, 반례, 소유권·동시성·호환성처럼 실질 검토가 필요한 질문이 생기면 서브에이전트에 독립 검토를 맡긴다. 이미 합의된 내용의 단순 확인이나 표현 정리 때문에 새 검토·승인 gate를 만들지는 않는다. 검토 도구를 사용할 수 없으면 직접 확인한 범위와 남은 불확실성을 설명하며, 검토를 맡겼다고 주장하지 않는다.

1. **질문을 기존 지도에 연결한다.** 사용자가 실제로 묻는 내용, 해당 group·child, 기존 확정 사항, 미채택·철회된 전제를 먼저 구별한다. 문서의 `approved` 표기도 원 사용자 발언·조건·최신 정정과 대조해 잘못 기록된 승인을 고정 전제로 전달하지 않는다. 탐색성 발언을 확정 요구사항으로 바꾸거나, 이번 질문과 관계없는 새 작업을 추가하지 않는다.
2. **검토 범위를 분리해 맡긴다.** 기본적으로 독립 검토자 한 명에게 질문과 반례 검토를 맡긴다. 제품·플랫폼·책임 경계 등 독립 축이 있으면 병렬로 나눈다. 복잡하거나 의견이 충돌하는 문제는 복수 조사자와 별도 품질 검토자로 확대할 수 있다. 인원·역할·범위를 사용자가 지정했으면 그 지시를 우선하며, 특정 인원 구성을 모든 논의에 강제하지 않는다. 기존 담당자를 재사용하고 문서·코드 수정 권한은 별도로 명시한다.
3. **결론 유도가 아닌 검증을 요청한다.** 사용자 질문, 관련 합의와 후보의 구분, 근거 문서·구현 위치, 검토 범위와 비목표를 전달한다. 사용자의 말이나 주 담당자의 추천을 정답으로 가정하지 말고, 성립 조건·반례·대안·비용을 비교하도록 한다. 기술 주장은 실제 구현과 공식 문서 등 1차 근거로 확인하고, 확인한 사실·추론·제안·미확인 항목을 구별해 보고하도록 한다.
4. **실제 공백인지 검증한다.** 문제가 생기는 최소 사례와 그 영향을 요구한다. 기존 원칙을 만족하는 안전한 구현이나 보수적 해석으로 이미 해결 가능한지 반증도 검토한다. 기존 합의로 대응되는 사례를 새 공백으로 세지 않고, 세부 알고리즘 미정과 실제 표현·의미 공백을 구별한다. 미채택 정책, 구현 편의, 최적화 선택을 필수 요구사항처럼 끼워 넣지 않는다. 설계 의미와 API의 구체 표현, 메모리 수명과 논리적 유효성처럼 서로 다른 층위도 구별한다.
5. **주 담당자가 결과를 대조한다.** 서브에이전트 보고를 그대로 결론으로 전달하지 않는다. 검토 중인 기술 결론을 결과 확인 전에 검증 완료로 표시하지 않는다. 원래 질문·기존 결정·핵심 근거를 직접 대조하고, 서로 다른 결과나 미검증 전제를 확인한다. 근거 부족·범위 이탈·충돌이 있으면 구체적인 수정 피드백을 보내 재검토시킨다. 의견 불일치는 다수결로 덮지 않으며, 확인되지 않은 부분은 불확실성으로 남긴다. 사용자의 직접 결정은 기존 핵심 원칙에 따라 반영하며 검토자 승인으로 대체하거나 불필요하게 재승인받지 않는다.
6. **검토 판정과 설계 채택을 분리한다.** 근거가 타당한 후보인지 승인·수정·반려한 검토 이력을 남긴다. 품질 검토를 통과한 추천도 사용자가 채택하기 전에는 `candidate`다. 검토자에게 최종 설계 결정권을 넘기거나 검토 완료를 구현 허가로 해석하지 않는다.
7. **기존 지도와 대화에 반영한다.** 새 후보·근거·철회 사유는 기존 체크리스트의 적절한 부모 아래에 기록하고, 관련 요약과 의존 관계도 함께 정렬한다. 보고서는 필요한 경우 기존 기록에서 연결하며, 같은 결정을 별도 문서마다 다시 승인받지 않는다. 사용자에게는 결론·이유·성립 조건·남은 불확실성을 간략히 설명한다. 실제로 필요한 결정이 있을 때만 현재 gate의 질문 하나를 제시한다.

검토를 맡긴 뒤에는 누구에게 무엇을 검토하도록 했는지 역할 이름과 범위로 짧게 알린다. 의미 있는 결과나 범위 변화가 생겼을 때만 후속 진행을 설명하고, 내부 식별자·검토 대기 상태·같은 결론을 반복해서 나열하지 않는다. 검토 결과가 도착했다는 이유만으로 다음 논점으로 넘어가지 말고 현재 사용자 질문과 논의부터 마무리한다.

## 5. 정합성 감사와 재정렬

결정이 추가·수정·거절될 때마다 다음을 검사한다.

1. 기존 확정 결정과 직접 모순되는가.
2. parent 결정이나 성공 기준의 전제를 무효화하는가.
3. 다른 group의 dependency 순서를 바꾸는가.
4. 같은 결정을 다른 이름으로 중복하고 있는가.
5. 거절된 subtree의 요소가 다른 곳에 남아 있는가.
6. 새 결정 때문에 빠진 coverage axis가 생겼는가.

충돌이 있으면 `정합성 감사`에 기록하고, 영향을 받는 항목을 `conflict` 또는 `superseded candidate`로 표시한다. 사용자의 새 결정이 명시적으로 이전 결정을 대체했다면 이전 결정을 `superseded`로 옮기고 active tree를 재작성한다.

다음 경우에는 부분 수정이 아니라 전체 decision map을 다시 리뷰한다.

- 목표나 system boundary가 바뀜
- 상위 architecture group이 거절되거나 대체됨
- 새 기술 조사로 핵심 전제가 틀렸음이 확인됨
- 여러 group에 걸친 모순이 발견됨

재리뷰 후에는 `추가 / 삭제 / 이동 / 충돌 해결` delta를 사용자에게 요약한다.

### 설계 문서 작성

- 문서의 목적과 독자의 판단에 필요한 자료를 골라, 각 자료가 뒷받침하는 결정이나 설명에 배치한다.
- 독자가 결정과 이유, 실제 동작과 검증 방법을 이해할 수 있도록 문서 목적에 맞게 내용을 배열한다.
- 주체와 대상을 먼저 밝히고, 이해에 필요한 조건·처리·결과를 구체적으로 쓴다.
- 문장과 문단은 앞의 설명을 이어받고, 논점이 바뀌는 곳에는 전환에 필요한 전제와 이유를 보충한다.
- 기존 글과 완성한 글의 문단별 역할을 한 줄로 적어 흐름을 점검하고, 중복되거나 역할 없는 설명을 덜어낸다.

## 6. 대화 방식

- 첫 응답에서는 전체 top-level map, 각 group의 child 범위, dependency, 현재 충돌을 보여준다.
- 첫 응답 끝에는 가장 선행성이 높은 group gate 하나를 추천한다.
- 이후에는 체크리스트 파일에 승인된 group의 immediate child 전체를 전개하되, 공개 대화에서는 그중 가장 선행성이 높은 active gate 하나만 설명하고 묻는다.
- 한 답변에서 여러 독립 child의 추천안과 승인 요청을 한꺼번에 펼치지 않는다. `전체 지도 공개`는 coverage 확인용이고 `결정 질문`은 항상 하나씩이다.
- 사용자가 `전체 리뷰`, `남은 결정`, `정합성 다시 봐`라고 하면 현재 subtree만 보지 말고 전체 map을 다시 검사한다.
- 모호한 지시어가 실제 결정을 바꿀 수 있으면 정확한 대상을 확인한다.
- 추상어만 반복하지 말고 concrete component, event, owner, state, boundary 이름을 쓴다.

## 7. 종료 조건

다음을 모두 만족해야 설계설득을 끝낸다.

- 모든 top-level group이 `approved`, `rejected`, `deferred`, `N/A` 중 하나다.
- 모든 approved group의 필요한 child가 `resolved` 또는 명시적 `deferred`다.
- 확정 결정 사이에 unresolved conflict가 없다.
- 목표와 성공 기준이 확정 결정에 trace된다.
- 구현 단계로 넘길 항목과 후속 milestone 항목이 분리돼 있다.
- 최종 architecture decision record와 남은 위험을 요약했다.

## Tone

한국어로 구체적이고 결정 지향적으로 말한다. 추천은 단호하게 하되, 사용자의 승인 없이 후보를 확정 사실로 만들지 않는다.