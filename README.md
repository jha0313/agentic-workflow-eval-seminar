# 삼성 AI 개발실 2차 — Agentic Workflow × AI Eval 세미나

<div align="center">

**Jae Ha** — Meta 시니어 엔지니어 · 9년 차 · Dev Productivity champion  
<sub>메타에서 실제로 해보고 배운 것을 계속 공유합니다</sub>

[![YouTube @sv.developer](https://img.shields.io/badge/YouTube-%40sv.developer-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@sv.developer)
[![Course](https://img.shields.io/badge/Course-Claude%20Code%20%26%20Agentic%20Workflow-1F6FEB?style=for-the-badge&logo=bookstack&logoColor=white)](https://fastcampus.co.kr/data_online_svvibecoding)
[![LinkedIn](<https://img.shields.io/badge/LinkedIn-jae--sang--ha-0A66C2?style=for-the-badge&logo=data:image/svg%2Bxml;base64,PHN2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHZpZXdCb3g9JzAgMCAyNCAyNCc+PHRleHQgeD0nMS41JyB5PScxOS41JyBmb250LWZhbWlseT0nQXJpYWwsSGVsdmV0aWNhLHNhbnMtc2VyaWYnIGZvbnQtd2VpZ2h0PSdib2xkJyBmb250LXNpemU9JzE5JyBmaWxsPSd3aGl0ZSc+aW48L3RleHQ+PC9zdmc+>)](https://www.linkedin.com/in/jae-sang-ha/)
[![KakaoTalk Open Chat](https://img.shields.io/badge/Open%20Chat-KakaoTalk-FEE500?style=for-the-badge&logo=kakaotalk&logoColor=000000)](https://open.kakao.com/o/g3Os2Lri)

<table>
<tr><td align="right">▶️ <b>유튜브</b></td><td><a href="https://www.youtube.com/@sv.developer">@sv.developer</a> — Claude Code · Agentic Engineering</td></tr>
<tr><td align="right">🎓 <b>강의</b></td><td><a href="https://fastcampus.co.kr/data_online_svvibecoding">실리콘밸리 엔지니어의 Claude Code — 바이브 코딩 &amp; 에이전틱 워크플로우 실전 로드맵</a> (일부 강의 무료 공개)</td></tr>
<tr><td align="right">💼 <b>링크드인</b></td><td><a href="https://www.linkedin.com/in/jae-sang-ha/">jae-sang-ha</a></td></tr>
<tr><td align="right">💬 <b>오픈카톡방</b></td><td><a href="https://open.kakao.com/o/g3Os2Lri">open.kakao.com/o/g3Os2Lri</a> — 세미나 질문</td></tr>
</table>

</div>

Agentic Workflow, AI Eval, 그리고 실제 업무에서 달라진 일하는 방식을 다룬 세미나의 **32장 발표 슬라이드**와, 발표에서 사용한 **스킬 3종**(workflow-orchestrator · eval-writer · skill-evaluator)을 담은 저장소다.

## 빠른 시작

### 슬라이드 열기

GitHub에서 HTML 파일을 클릭하면 소스가 보인다. clone한 뒤 저장소 루트에서 정적 서버를 띄우고 브라우저로 연다. 영상이 `assets/` 상대경로라 파일을 직접 열면 브라우저에 따라 재생되지 않을 수 있다.

```bash
git clone https://github.com/jha0313/agentic-workflow-eval-seminar.git
cd agentic-workflow-eval-seminar
python3 -m http.server 8000
# 브라우저: http://localhost:8000/slides/index.html
```

- 방향키로 장을 넘긴다. 고유 조작이 있는 장은 **INTERACTIVE** 표시와 사용 방법이 화면에 나온다.
- 오른쪽 위 메뉴: 전체 슬라이드(O) · 전체 화면(F) · 다크/라이트 테마 · 현재 슬라이드 PNG · Print/PDF · 발표 타이머(T).
- 영상 재생이나 설명용 제어는 새 AI 작업이나 배포를 실행하지 않는다.

### 스킬 설치

Claude Code 기준. 저장소 루트에서 실행하면 세 폴더를 `~/.claude/skills/`에 통째로 복사하고, 이미 있는 대상은 덮어쓰지 않고 건너뛴다.

```bash
mkdir -p ~/.claude/skills
for skill in workflow-orchestrator eval-writer skill-evaluator; do
  if [ -e "$HOME/.claude/skills/$skill" ] || [ -L "$HOME/.claude/skills/$skill" ]; then
    printf '기존 설치 보존: %s\n' "$skill"
  else
    cp -R "$skill" "$HOME/.claude/skills/$skill"
  fi
done
```

복사 대신 `ln -s "$PWD/$skill" "$HOME/.claude/skills/$skill"`로 symlink해도 된다. SKILL.md만 복사하지 말고 폴더 전체를 설치한다. Codex 등 다른 호스트는 해당 호스트의 스킬 경로를 사용한다.

- workflow-orchestrator는 호스트의 subagent 기능이 필요하다.
- eval-writer·skill-evaluator의 실제 실행은 Python 3.11+, [uv](https://docs.astral.sh/uv/getting-started/installation/), native eval 기능(`claude plugin eval`)이 있는 인증된 Claude Code가 필요하다. 아래 명령으로 환경을 확인한다(설치 없이 저장소 루트에서 실행 가능).

```bash
uv run skill-evaluator/scripts/evaluate.py doctor
uv run skill-evaluator/scripts/evaluate.py --help
```

## 구성

| 경로 | 내용 |
|---|---|
| [slides/index.html](slides/index.html) | 32장 발표 deck. 단계별 설명, 실제 AutoKliq 영상, 선별된 실행 근거, 평가 전후 비교. |
| [slides/workflow-orchestrator.html](slides/workflow-orchestrator.html) | 조율자와 작업자의 역할, 단계별 입력·산출물·근거를 눌러 확인하는 개념도. |
| `slides/assets/` | 데모 영상 2개, 브랜드 워드마크, LinkedIn 공식 로고와 [출처 기록](slides/assets/linkedin-source.json). |
| [workflow-orchestrator/](workflow-orchestrator/README.md) · [eval-writer/](eval-writer/README.md) · [skill-evaluator/](skill-evaluator/README.md) | 발표에서 사용한 스킬 3종. 각 폴더의 README와 SKILL.md가 사용법이다. |

## 발표에서 사용한 스킬

| 스킬 | 하는 일 |
|---|---|
| [workflow-orchestrator](workflow-orchestrator/README.md) | 조사·계획·구현·검증·리뷰를 작업자에게 맡기고, 조율자는 의존성과 근거만 관리한다. |
| [eval-writer](eval-writer/README.md) | 대상 스킬의 계약에서 재현 가능한 평가 사례와 루브릭을 작성·검토한다. |
| [skill-evaluator](skill-evaluator/README.md) | 격리된 실제 실행, 독립 채점, 근거 보존, 한국어 로컬 HTML/Markdown 보고서를 만든다. |

세 폴더는 [jha0313/skills_repo](https://github.com/jha0313/skills_repo)의 한국어판과 같은 내용이다.

## 데모 근거

- **Slide 13 · AutoKliq**: [PR #80](https://github.com/careerhackeralex/autokliq/pull/80)(채널 프로듀서 작업실)과 [PR #81](https://github.com/careerhackeralex/autokliq/pull/81)(썸네일 UI/UX). 영상 2개와 deck에 내장한 선별 발췌만 포함하며, 원본 리플레이·raw 도구 기록·하위 에이전트 대화는 포함하지 않는다.
- **Slide 24 · skill-evaluator**: Before 8/10 통과·평균 4.434/5·종합 FAIL → After 9/10 통과·평균 4.4605/5·종합 FAIL. 같은 조건의 10개 사례 **한 쌍**을 관찰한 것이며, 일반적인 성능 향상이나 스킬 사용 유무 A/B의 증거가 아니다.

## 공유 범위

- 내부 설문 수치·원문, 발표자 노트, 발표 원고는 포함하지 않는다.
- deck은 외부 리소스 4건 — Google Fonts(preconnect 1건, 스타일시트 2건)와 PNG 내보내기용 `html-to-image` 스크립트 1건 — 을 불러온다. 오프라인에서는 시스템 글꼴로 대체되고 PNG 내보내기만 동작하지 않는다. `workflow-orchestrator.html`은 외부 로드가 없다.
- 두 HTML 모두 위 리소스 로드 외에는 데이터를 전송하지 않으며, AI 작업·설치·배포를 실행하지 않는다. 링크는 클릭할 때만 새 탭으로 열린다.
