# motion.md

우리 서비스의 움직임 기준 · 성격: **경쾌함** (사용자 선택, 2026-10-25).
짧게 반응하고 부드럽게 감속한다. 도착한 요소는 정지하며 반복 부유·튕김을 기본값으로 쓰지 않는다.
외형은 `design.md`와 `src/` 원본을 따른다. 레퍼런스의 색·글자·로고·기기 외형은 가져오지 않는다.
장면 순서·화면 배치·절대 시작 시각은 영상별 비트 시트에서 정한다. 아래 시각은 출처 위치일 뿐이다.

출처:
- **R1**: [Remotion Prompts — Apple-Style Device Rise Animation](https://www.remotion.dev/prompts/apple-style-device-rise-animation). 프롬프트 문서와 원본 영상 모두 확인.
- **R2**: [Skillry — @SaadPastaDev / Nafa 원본](https://skillry.dev/ai-videos/opus-5-5?category=motion&video=saadpastadev-347341). Remake가 아닌 Original을 확인.
- 원본·관찰 자료: `references/remotion-device-rise/`, `references/skillry-nafa/`.
- **명시** = R1 문서값, **관찰** = 원본 프레임에서 추정, **채택** = 관찰 리듬을 우리 규칙으로 정규화, **파생** = 기존 토큰의 계산값. 관찰을 원본 코드값으로 단정하지 않는다.

## 1 Primitives

### 시간: duration 6단계

기준 `fps = 30` (R1 문서 명시, R1·R2 영상 메타데이터 일치). `1f = 1/30초`; 다른 fps에서도 초 길이를 유지하고 프레임은 반올림한다.

| 토큰 | f / 초 | 용도 | 출처·근거 |
|---|---|---|---|
| `dur-xxs` | 4f / 0.133333…s | 글자·숫자 단위 갱신 간격 | R2 00:05.37–00:05.77 입력 글자 교체, 약 4f 간격 관찰·채택 |
| `dur-xs` | 6f / 0.2s | 즉각 반응·빠른 교체 | R2 00:16.43–00:16.63 짧은 문구 등장, 약 6f 관찰·채택 |
| `dur-s` | 9f / 0.3s | 목록 지연·소형 요소 등장 | R1 00:01.5 이후 카드 등장, 문서의 카드 간격 0.3s를 명시값으로 채택 |
| `dur-m` | 12f / 0.4s | 카드 등장·짧은 전환 | R2 00:13.3–00:13.7 확인 컨트롤 이동, 약 12f 관찰·채택 |
| `dur-l` | 30f / 1s | 큰 요소 강조·카메라 이동 | R2 00:15–00:16 화면을 향한 확대 관찰; 약 1s를 채택 |
| `dur-xl` | 45f / 1.5s | 주인공 화면의 상승·안착 | R1 00:00–00:01.5 기기 상승, 문서의 1.5s 명시값 |

R2 경계는 프레임 표본에 따른 근사치다. 짧은 구간은 1–2f, 큰 동작은 약 0.2s 수준의 경계 불확실성이 있다.
`dur-s`의 출처는 카드 **간격**이며 원본 카드의 동작 길이라고 주장하지 않는다. 소형 등장에 쓰는 것은 서비스의 채택 규칙이다.

### 곡선

`t`는 각 동작의 정규화 진행률(시작 0, 끝 1)이다.

| 토큰 | 함수 / 구현 | 출처·적용 |
|---|---|---|
| `ease-out` | `1-(1-t)^3` / GSAP `power2.out` | R1 00:00–00:01.5, 문서 `Easing.out(Easing.cubic)` 명시. 등장·안착 |
| `ease-in` | `t^3` / GSAP `power2.in` | R1 `ease-out`의 시간 반전 파생. 퇴장용 채택이며 R2 곡선의 실측값은 아님 |
| `ease-linear` | `t` / GSAP `none` | R2 00:05.37–00:05.77 일정한 글자 갱신 관찰을 균등 진행으로 채택. 원본 곡선은 미공개 |

### 움직임 범위

| 토큰 | 값 | 출처·해석 |
|---|---|---|
| `move-part` | `space-section` = 24px | `design.md` §4 기존 값 재사용. R1 00:01.5–00:02.4 카드 이동 방식을 부품 좌표로 축소 적용; 원본 이동량은 아님 |
| `scale-reveal` | 0.8 → 1 | R1 00:00–00:01.5, 문서 scale 명시. 원본을 감싸는 래퍼에 적용 |
| `tilt-reveal` | rotateX 35° → 8° | R1 같은 구간, 문서 근삿값을 채택. 3D를 요청한 화면 래퍼에만 적용 |
| `alpha-reveal` | 0 → 1 | R2 00:05.2–00:05.4 UI 교체 관찰을 완전 숨김→표시로 채택. 중간 곡선은 `ease-out` |

좌표의 px는 부품의 원본 좌표계 기준이며 외부 배율과 함께 변환한다. 위 범위를 장면 배치나 새 기기 목업 규격으로 사용하지 않는다.

## 2 Semantic

아래 **5개만** 의미 토큰으로 사용하고 구현에서는 Primitive를 별칭으로 연결한다.

| 토큰 | 시간 · 곡선 · 속성 | 출처·용도 |
|---|---|---|
| `enter` | `dur-m` + `ease-out`; y `move-part`→원위치, `alpha-reveal` | R1 00:01.5–00:02.4 카드 감속 등장 + R2 00:05.2–00:05.4 교체를 부품에 채택 |
| `exit` | `dur-xs` + `ease-in`; alpha 역방향 | R2 00:05.2–00:05.4 짧은 퇴장 관찰; 곡선은 R1 반전 파생. 외형·좌표 유지 |
| `press` | `dur-xs` + `ease-out`; 기본은 반응 시작 시간 상한 | R2 00:13.3–00:13.7 조작→결과의 즉시성을 채택. 눌림 축소·복원 배율은 새 후보 |
| `emphasis` | `dur-l` + `ease-out`; 진행량·숫자를 실제 목표값까지 | R2 00:10.5–00:11.5 금액 갱신, 00:13.3–00:13.7 진행 표현에서 동작 방식 채택 |
| `transition` | `dur-m` + `ease-out`; 들어오는 래퍼의 마스크를 닫힘→열림 | R2 00:16.0–00:16.5 세로 와이프 관찰을 12f로 정규화. 색 변경은 포함하지 않음 |

`press`는 스케일 애니메이션의 승인이 아니다. 결과가 달라지는 동작은 연결된 실제 상태·데이터를 사용하고 가짜 성공 상태를 만들지 않는다.
문자 단위 갱신은 `dur-xxs`, 반복 부품의 등장 간격은 `dur-s`를 참조한다. 동시 등장 개수·실행 순서는 비트 시트에서 정한다.

## 3 Pattern

Pattern은 다음 실제 파일의 적용법이며 새 화면·에셋·`src/patterns/` 파일이 이미 있다고 가정하지 않는다.

| Pattern / 실제 대상 | 적용 규칙 | 출처 |
|---|---|---|
| `card-enter` / `mission-card.html`, `skill-card-1.html`·`2`·`3`, `asset-card.html` | 바깥 래퍼에 `enter`; 반복 목록 간격 `dur-s`. 카드 내부 비율 유지 | R1 00:01.5–00:02.4 카드 등장·9f 간격 |
| `button-response` / `button-submit.html` | 조작 후 `press` 시간 안에 반응 시작; 기존 `data-state`가 실제 바뀌면 `exit`·`enter`로 내용 교체 | R2 00:13.3–00:14.6 조작 후 결과. 스와이프 전용 UI는 복제하지 않음 |
| `progress-fill` / `mission-card.html` | `#mission-card-progress-fill`의 `--progress`를 현재값→실제 목표값, `emphasis`; `#mission-card-progress-text`와 동기화 | R2 00:13.3–00:13.7 진행 이동; 수치 끝점은 원본·영상 입력 |
| `amount-count` / `stat-card.html` | `#stat-card-amount`를 현재값→`data-final`, `emphasis`; 갱신 간격 `dur-xxs`, 마지막에는 정확한 목표값 | R2 00:10.5–00:11.5 숫자 입력·갱신 |
| `approval-reveal` / `badge-approved.html` | `enter`로 완료 표시 후 정지. 반복 pulse·overshoot는 새 후보 | R2 00:14.0–00:14.6 완료 표시의 등장 |
| `screen-reveal` / `src/screens/*.png` 5장 | 원본 이미지 전체를 감싼 래퍼에 `dur-xl`·`ease-out`·`scale-reveal`; 기울임은 요청 시만 `tilt-reveal` | R1 00:00–00:01.5 기기 상승·scale 방식; 기기 외형 제외 |
| `screen-transition` / 같은 5장 | 새 화면 래퍼에 `transition`; PNG 안의 부품을 개별 편집 가능한 요소로 취급하지 않음 | R2 00:16.0–00:16.5 가림막 전환 |
| `brand-reveal` / `brand-icon.html`, `logo.html` | 래퍼에 `enter` 또는 alpha만 적용; 원본 도형·서체·색 유지, 도착 후 정지 | R2 00:22–00:24.5 엔딩의 정지된 브랜드 표시 |

부품 파일 경로는 모두 `src/parts/` 아래다. 화면 파일은 `01-home.png`, `02-skills.png`, `03-mission.png`, `04-approved.png`, `05-assets.png`이며 이 목록은 재생 순서가 아니다.
부품의 `data-state` 변경은 실제 제공된 상태로 한정한다. `src/assets/` 원본은 감싸서 움직이고 다시 그리지 않는다.

## 4 머무는 시간

시간은 동작이 끝나 읽을 수 있게 된 시점부터 센다. 진입·퇴장 시간을 포함하지 않는다.

| 토큰 | 최소값 | 출처·적용 |
|---|---|---|
| `hold-short` | `dur-m` = 12f / 0.4s | R2 00:16.6–00:17.0 짧은 단위의 정지 관찰·채택. 짧은 상태·낱말 |
| `hold-read` | `dur-xl` = 45f / 1.5s | R1 00:02.4–00:04.0 카드 안착 후 정지 약 1.6s를 보수적으로 정규화. 짧은 안내·완료 상태 |
| `hold-detail` | `dur-xl` + `dur-l` = 75f / 2.5s | R2 00:07.5–00:10.0 상세 UI를 유지하는 구간 관찰·채택. 카드·화면 읽기 |
| `hold-end` | `dur-l` × 2 = 60f / 2s | R2 00:22.5–00:24.5 엔딩 유지 관찰·채택. CTA·마지막 문구 |

긴 문구를 맞추려고 글자를 줄이거나 읽기 시간을 자동 단축하지 않는다. 위 값으로 부족한 경우 비트 시트에서 더 긴 hold를 요청하고, 새 수치면 새 후보로 확인한다.

## 5 카메라

기본은 고정. 아래 동작은 선택 가능한 어휘이며 특정 화면·방향·배치를 강제하지 않는다.

| 토큰 | 시간 · 곡선 · 범위 | 출처·조건 |
|---|---|---|
| `cam-in` | `dur-l` + `ease-out`, 래퍼 scale 0.8→1 | R2 00:15–00:16 확대 방식 관찰; 수치 범위는 R1 `scale-reveal`을 재사용한 서비스 채택값 |
| `cam-pull` | `dur-l` + `ease-out`, 래퍼 scale 1→0.8 | R1 `scale-reveal` 역방향 파생. 실제 레퍼런스에서 측정한 pull-out은 아님 |
| `cam-rise` | `dur-xl` + `ease-out`, 로컬 y 이동과 선택적 `tilt-reveal` | R1 00:00–00:01.5. 이동량은 `move-part`만 허용; 원본의 큰 이동량을 그대로 이식하지 않음 |

카메라는 원본 바깥 래퍼에만 적용하고 글자·로고를 따로 늘리지 않는다. R1의 기기 기울임은 피사체 변형이며 실제 3D 카메라 데이터가 아니다.
**새 후보**: 눌림 scale, spring·bounce·overshoot, blur 강도, 추가 이동량·각도·perspective, 새 곡선·duration·hold는 현재 미정이다.
필요해지는 시점에 근거·값·기존 토큰으로 해결되지 않는 이유를 제시해 사용자에게 묻고, 승인된 것만 이 문서에 등록한 뒤 사용한다.
