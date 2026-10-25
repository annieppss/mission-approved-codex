# Motion Harness

목적: 초보자의 자연어 요청을 우리 서비스의 기준을 지키는 모션 영상으로 완성한다.

## 고정과 변경

- 영상마다 바뀌는 것은 BRIEF와 레퍼런스다. 레퍼런스의 역할은 나누고 전체 뼈대는 하나만 선택한다.
- 표현 기준은 `docs/`, 화면·부품·브랜드 원본은 `src/`를 따른다. docs의 값은 여기에 복사하지 않는다.

## 꼭 지킬 규칙

1. 스타일·모션 수치는 기준 문서에 연결한다. 새 후보는 물어서 승인받고 기준에 등록한 뒤 쓴다.
2. 장면표와 스냅보드는 현재 버전에 대한 사람의 승인이 있어야 다음 단계로 넘어간다.
3. 만든 역할과 판정하는 critic을 분리한다. 결과를 먼저 보여주고 판정은 한 줄, 같은 게이트에서 두 번 불합격하면 멈추고 묻는다.

## 실행

“모션 만들자” 또는 “영상 만들자” → `skills/make-video/SKILL.md`를 읽고 그 순서로 실행한다.
BRIEF → art-director → director → builder의 스냅보드 → builder의 영상. critic은 각 결과를 판정만 한다.
역할 정의는 `.codex/agents/art-director.toml`, `director.toml`, `builder.toml`, `critic.toml`이다.
Codex의 native custom agent로 순차 실행하고 모델 설정은 상속한다. 해당 기능이 없으면 각 정의를 읽은 별도 subagent에 작업을 맡긴다.
별도 판정자를 실행할 수 없으면 독립 critic이 수행됐다고 주장하지 말고 그 지점에서 알린다.

## 폴더와 시작 방법

- 기준: `docs/design.md` · `docs/motion.md` · `docs/video.md`.
- 재료: `src/screens/` · `src/parts/` · `src/assets/`. 레퍼런스 원본: `references/`.
- 영상별 BRIEF와 산출물: `out/<영상이름>/`. 승인·실패 기록과 재개 방법은 스킬을 따른다.
- Codex에서 이 파일은 자동 로드를 전제로 하지 않는다. 현재 채팅에서는 “모션 만들자”로 시작하며, 새 채팅에서는 **“CLAUDE.md와 skills/make-video/SKILL.md를 읽고 모션 만들자”**로 시작한다.
- 이번 설계 파일은 이 파일, agent TOML 네 개, 스킬 한 개다. 실제 제작 산출물은 실행할 때 만든다.
