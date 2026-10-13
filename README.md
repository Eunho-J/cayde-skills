![글의 구성, 설계 결정, 구현 검증을 연결하는 cayde-skills 배너](assets/cayde-skills-banner.png)

# cayde-skills

글의 구조를 잡고, 설계의 결정을 정리하고, 구현을 검증하는 한국어 Codex 스킬 모음입니다. 각 스킬의 원문과 함께 필요한 템플릿·참고 문서를 관리합니다.

[스킬 고르기](#스킬-고르기) · [설치](#설치) · [사용 예시](#사용-예시) · [함께 사용할 때](#함께-사용할-때)

## 스킬 고르기

| 스킬 | 이런 작업에 사용합니다 | 남기는 결과 |
|---|---|---|
| [글의 구성과 집필](skills/writing-structure-guide/SKILL.md) · `writing-structure-guide` | 글의 구성 틀을 비교하고, 섹션·문단을 설계하거나 원고를 재구성할 때 사용합니다. | 목적에 맞는 구성과 필요한 설명을 갖춘 원고를 만듭니다. |
| [설계설득](skills/design-persuasion/SKILL.md) · `design-persuasion` | 설계·아키텍처의 선택지를 검토하고 사용자와 결정을 좁혀 갈 때 사용합니다. | 계층형 결정 지도에 후보·확정 결정·의존 관계와 충돌을 기록합니다. |
| [레드팀](skills/red-team/SKILL.md) · `red-team` | 확정된 설계에 따라 구현 단위의 입출력과 변수값 변화를 검증할 때 사용합니다. | 설계에서 도출한 사례 목록에 표준 테스트와 항목별 독립 검토의 근거를 연결합니다. |

## 설치

처음 설치할 때는 저장소를 복제하고 스킬 디렉터리를 개인 스킬 경로에 복사합니다.

```bash
git clone https://github.com/Eunho-J/cayde-skills.git
mkdir -p ~/.agents/skills
cp -R -n cayde-skills/skills/. ~/.agents/skills/
```

`-n`은 기존 파일을 보존합니다. 같은 이름의 스킬이 이미 있으면 원문 차이를 확인한 뒤 필요한 변경을 반영하세요. 한 스킬만 설치할 때도 `SKILL.md`와 그 디렉터리의 관련 파일을 함께 복사합니다.

프로젝트에서만 사용하려면 해당 저장소의 `.agents/skills/`에 복사합니다. 검색 경로와 호출 방식은 [OpenAI 공식 스킬 문서](https://learn.chatgpt.com/docs/build-skills)를 참고하세요.

## 사용 예시

Codex CLI·IDE에서는 `$`로 스킬을 지정하거나 `/skills`에서 선택할 수 있습니다. 작업 대상과 관련 자료를 함께 전달하면 스킬의 범위를 구체적으로 정할 수 있습니다.

**글의 구조를 잡을 때**

```text
$writing-structure-guide로 이 보고서의 독자와 목적에 맞는 구성을 고르고,
섹션과 문단의 흐름을 점검한 뒤 원고를 다듬어 줘.
```

**설계의 결정을 좁혀 갈 때**

```text
$design-persuasion으로 이 설계를 전체 검토해 줘.
기존 결정과 제약을 확인하고, 계층형 결정 지도를 만든 뒤
상위 묶음부터 한 번에 하나의 결정씩 논의해 줘.
```

**설계에 따라 구현을 검증할 때**

```text
$red-team으로 이 설계에서 구현 단위로 테스트할 수 있는 엣지 케이스를
코드와 분리해 먼저 작성해 줘. 각 사례의 표준 테스트를 보완하고,
독립 검토자에게 한 항목씩 전달해 목록 전체를 검토해 줘.
```

### 레드팀의 검증 범위

레드팀은 **설계 분석 → 테스트·보완 → 항목별 독립 검토** 순서로 진행합니다. 자세한 경계는 [스킬 원문](skills/red-team/SKILL.md)에 있습니다.

- 테스트는 프로젝트의 표준 테스트 코드·스크립트와 러너 안에서 작성하고 실행합니다.
- 모든 테스트는 실제 구현의 입출력과 실행 전후 프로그램 변수값만 검사합니다.
- 유닛테스트의 정규식 사용, 소스 텍스트·스니펫·AST 노드의 존재를 검사하는 가짜 테스트, 별도 외부 검증기·실행기·수집기는 금지합니다.
- 음성 등 실제 계정·운영 환경의 실사용 시험은 사용자가 수행합니다. 에이전트는 필요한 절차를 안내하고 전달받은 결과를 기록·분석합니다.

## 함께 사용할 때

세 스킬은 필요한 작업에 맞춰 각각 사용할 수 있습니다. 설계부터 구현 검증까지 이어지는 작업에서는 다음처럼 연결할 수 있습니다.

1. **설계설득으로 결정합니다.** 전체 결정 지도를 만들고, 상위 묶음부터 합의하며 의존 관계와 충돌을 정리합니다.
2. **글쓰기 스킬로 설명합니다.** 결정과 이유를 독자가 따라갈 순서로 배치하고, 문단 사이에 빠진 관계와 중복을 점검합니다.
3. **레드팀으로 검증합니다.** 확정된 설계에서 사례를 도출하고, 표준 테스트와 순차 독립 검토로 구현을 확인합니다.

레드팀에서 사용하는 [사례 목록 양식](skills/red-team/assets/edge-cases.template.md)과 [항목별 검토 지시](skills/red-team/references/single-case-review.md)도 함께 제공합니다.

<details>
<summary>README 구성에 참고한 저장소</summary>

2026-10-13 07:09 UTC에 아래 GitHub 저장소의 실제 README와 스타 표시를 확인했습니다. 인기 저장소 몇 곳을 살펴본 참고 기록이며, 정확한 전체 순위를 뜻하지 않습니다. 스타 수는 확인 시점의 표시값을 반올림한 규모입니다.

| 참고 저장소 | 스타 규모 | 참고한 구성 |
|---|---:|---|
| [sindresorhus/awesome](https://github.com/sindresorhus/awesome#readme) | 약 518k | 상단 이미지와 짧은 탐색 링크, 한 줄 설명이 붙은 목록을 참고했습니다. |
| [freeCodeCamp/freeCodeCamp](https://github.com/freeCodeCamp/freeCodeCamp#readme) | 약 457k | 첫 문단에서 목적과 독자에게 주는 가치를 설명하는 구성을 참고했습니다. |
| [nilbuild/developer-roadmap](https://github.com/nilbuild/developer-roadmap#readme) | 약 369k | 시작 경로와 자료 목록을 앞에 두는 구성을 참고했습니다. |
| [microsoft/generative-ai-for-beginners](https://github.com/microsoft/generative-ai-for-beginners#readme) | 약 121k | 상단 배너와 시작 안내, 항목별 설명 표를 참고했습니다. |

</details>
