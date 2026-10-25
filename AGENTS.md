# Codex Motion Harness

이 저장소는 Codex용 모션 하네스와 승인된 소개 영상 프로젝트다. 사용자에게 한국어로 응답한다.

- “모션 만들자”·“영상 만들자” 또는 영상 제작·수정 요청을 받으면 루트의 `CLAUDE.md`와 `skills/make-video/SKILL.md`를 먼저 읽는다. `CLAUDE.md`는 수업에서 정한 공통 하네스 규약이며 Codex는 이 `AGENTS.md`를 진입점으로 사용한다.
- 역할 정의는 `.codex/agents/`의 TOML 네 개다. 해당 프로필과 스킬의 단계·승인·독립 검수 규약을 따른다.
- 디자인·움직임·영상 룩은 `docs/`, 실제 원본 재료는 `src/`를 읽는다. 기준값을 지시문에 복사하지 않는다.
- 실제 HyperFrames 구현을 수정하기 전 설치된 `.agents/skills/hyperframes/SKILL.md`와 관련 참조를 읽는다. 외부 스킬 설치 방법은 `README.md`에 있다.
- `out/`의 완료된 영상·승인 기록은 보존한다. 새 영상은 스킬이 정한 새 작업 폴더에서 시작하며 기존 승인을 새 작업에 재사용하지 않는다.
