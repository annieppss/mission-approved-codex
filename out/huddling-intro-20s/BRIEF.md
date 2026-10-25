# BRIEF — 허들링 클럽 서비스 소개

revision: brief-r1
사용자 답변 확정일: 2026-10-25
제작 폴더: `out/huddling-intro-20s/`

## 무엇을

저장소의 실제 화면·부품으로 허들링 클럽을 소개한다.
메시지의 범위는 스킬 탐색, 미션 참여·제출, 승인 결과, 내 자산 관리 등 원본 화면에서 확인되는 기능이다.
금액·성과·외부 서비스 연동을 실제보다 확대해 주장하지 않는다. 문구는 서비스 원본과 이 목적에서 도출한다.
기획 세 안은 이 범위 안에서 강조점을 달리한다. 사용자가 고른 안만 상세화한다.

## 몇 초

총 길이: **20초** (사용자 선택).
출력 규격과 룩은 `docs/video.md`, 움직임은 `docs/motion.md`, 디자인·재료는 `docs/design.md`를 따른다.

## 레퍼런스와 역할

| 레퍼런스 | URL | 담당 역할 | 로컬 분석용 원본 |
|---|---|---|---|
| Nafa Original | https://skillry.dev/ai-videos/opus-5-5?category=motion&video=saadpastadev-347341 | 전체 뼈대·전환; 구도·카메라·글자 배치는 관찰 근거를 나눠 기록 | `references/skillry-nafa/original.mp4` |
| Remotion Device Rise | https://www.remotion.dev/prompts/apple-style-device-rise-animation | 화면 등장·감속 안착; 전체 뼈대는 맡지 않음 | `references/remotion-device-rise/original.mp4` |

위 역할 조합은 사용자 답변으로 확정됐다. 전체 뼈대 담당은 Nafa 하나다.
레퍼런스의 색·서체·로고·기기 외형·새 UI는 가져오지 않는다. 실제 제작 에셋은 `src/`에서 사용한다.
수업의 Figma 노드 `39:557`은 제작 진행 절차를 설명하는 자료이며 영상의 시각 레퍼런스가 아니다.

## 제작·승인

art-director의 레퍼런스 정리 → director의 기획 3안 → 사용자 선택 → 선택안의 상세 장면표 승인 → 스냅보드 승인 → 영상 제작·검수.
현재 답변은 BRIEF 입력 선택이며 장면표·스냅보드 승인이 아니다.
`CLAUDE.md`와 `skills/make-video/SKILL.md`의 역할·게이트·실패 처리 규약으로 진행한다.
