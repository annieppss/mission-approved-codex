# video.md

영상 룩 기준 · 2026-10-25.
사용자 선택: **밝은 배경 / 단정한 헤드라인 / 밝은 엔드 카드**.
색·폰트·원본 외형의 출처는 `design.md`뿐이다. 영상용 글자 크기는 해당 Figma 스타일의 **2.8배**다.
장면 순서·흐름·화면 배치·구성은 이 문서에서 정하지 않고 영상별 레퍼런스와 비트 시트에서 정한다.
움직임·전환·머무는 시간은 `motion.md`를 참조한다.

| 출력 토큰 | 값 | 출처 |
|---|---|---|
| `video-ratio` | 16:9 | 사용자 지정 |
| `video-w` | 1920px | 사용자 지정 |
| `video-h` | 1080px | 사용자 지정 |
| `video-fps` | 30 | 사용자 지정; `motion.md`의 기준과 일치 |
| `text-scale` | 2.8 | 사용자 지정; 별도로 만드는 모션용 텍스트에 적용 |

## 1 텍스트 최소 크기

| 토큰 | 값 | 출처·적용 |
|---|---|---|
| `video-min` | 30.8px | `Text/eyebrow` 11px × `text-scale`; 별도 모션 텍스트의 최솟값 |
| `video-body` | 42px | `Text/body` 15px × `text-scale`; 설명 문구의 기본 크기 |
| `video-head` | 78.4px | `Text/display` 28px × `text-scale`; 단정한 헤드라인의 기본 크기 |

최소값은 최종 1920×1080 출력에서의 크기다. 텍스트 래퍼를 축소해 실제 표시 크기가 `video-min` 아래로 내려가지 않게 한다.
폰 화면 이미지·원본 parts 내부의 글자는 원본 UI의 일부이므로 개별적으로 이 배율을 적용하지 않는다. 영상에서 꼭 읽어야 할 정보는 별도의 모션 텍스트로 제공한다.

## 2 배경

| 토큰 | 디자인 토큰 | 값 | 사용 의미 |
|---|---|---|---|
| `video-bg` | `color-bg` | `#ffffff` | 사용자가 선택한 밝은 기본 배경 |
| `video-surface` | `color-surface` | `#f7f7f8` | 필요한 경우 구분에 쓰는 표면 |
| `video-end-bg` | `color-bg` | `#ffffff` | 사용자가 선택한 밝은 엔드 카드 바탕 |
| `video-line` | `color-border` | `#e9eaed` | 필요한 경우 경계를 나타내는 선 색 |

모두 `design.md` §1의 별칭이다. `video-end-bg`는 엔드 카드가 있을 때의 외형만 정의하며 엔딩 장면의 존재·순서·구성을 지정하지 않는다.
디자인에 없는 배경색·그라데이션·텍스처는 추가하지 않는다. 기존 parts 내부 표면과 일러스트 색은 원본 그대로 유지한다.

## 3 글자

`video-font = Pretendard`. 원본 스타일의 굵기(400·600·700)와 자간(0%)을 유지한다.
크기와 px 행간을 함께 `text-scale`로 곱해 원본 비율을 유지하며, 소수점 값을 반올림해 새 크기로 바꾸지 않는다.

| 토큰 | Figma 스타일 | 원본 크기 / 행간(px) | 영상 크기 / 행간(px) | 굵기 |
|---|---|---|---|---|
| `video-display` | `Text/display` | 28 / 34 | 78.4 / 95.2 | 700 |
| `video-title` | `Text/title` | 24 / 32 | 67.2 / 89.6 | 600 |
| `video-compact` | `Text/compact-title` | 22 / 28 | 61.6 / 78.4 | 700 |
| `video-section` | `Text/section` | 20 / 28 | 56 / 78.4 | 700 |
| `video-heading` | `Text/heading` | 18 / 28 | 50.4 / 78.4 | 600 |
| `video-body` | `Text/body` | 15 / 24 | 42 / 67.2 | 400 |
| `video-list` | `Text/list-title` | 15 / 22 | 42 / 61.6 | 600 |
| `video-label` | `Text/label` | 14 / 20 | 39.2 / 56 | 600 |
| `video-caption` | `Text/caption` | 12 / 16 | 33.6 / 44.8 | 400 |
| `video-eyebrow` | `Text/eyebrow` | 11 / 16 | 30.8 / 44.8 | 600 |

출처: `design.md` §2의 모든 로컬 텍스트 스타일. `video-head`는 `video-display`, `video-min`은 `video-eyebrow`의 크기 별칭이다.
‘단정하게’는 원본 서체·비율·굵기를 유지하는 선택이다. 폭 맞춤을 위한 글자 늘이기·새 자간·초대형 임의 크기를 사용하지 않는다.

| 색 토큰 | 디자인 토큰 | 값 |
|---|---|---|
| `video-text` | `color-text` | `#18191b` |
| `video-muted` | `color-text-muted` | `#75777c` |
| `video-accent` | `color-primary` | `#3759de` |
| `video-end-text` | `color-text` | `#18191b` |
| `video-on-accent` | `color-on-primary` | `#ffffff` |

색의 출처는 `design.md` §1이다. `video-on-accent`는 `video-accent` 표면 위의 글자용이며 밝은 기본 배경에서는 `video-text`를 사용한다.
폰트는 `design.md` §3의 `src/assets/fonts/Pretendard-Regular.otf`·`Pretendard-SemiBold.otf`·`Pretendard-Bold.otf`를 사용한다.
참조 경로는 HTML 위치에 맞춰 해석하고 렌더링 전에 `document.fonts.ready`를 기다린다. 다른 폰트로의 자동 대체·합성 굵기를 사용하지 않는다.

## 4 폰 프레임

| 토큰 | 값 | 출처·해석 |
|---|---|---|
| `phone-frame` | `screen-only` | `design.md` §6에는 화면 PNG만 있으므로 추가 기기 외곽 없이 원본 화면을 감싼 래퍼 사용 |
| `phone-w` | 390px | 같은 절의 Figma 화면 논리 폭 |
| `phone-h` | 844px | 같은 절의 Figma 화면 논리 높이 |
| `phone-ratio` | 390:844 | 위 원본 크기의 종횡비 |
| `phone-src` | 780×1688px, @2x | `src/screens/*.png` 원본 해상도 |

논리 크기는 원본을 다루는 기준이며 영상 속 고정 표시 크기나 위치가 아니다. 표시 크기는 레퍼런스가 정하되 전체를 등비 확대·축소한다.
PNG 원본을 그대로 사용하고 화면 안의 글자·상태 바·버튼을 다시 그리지 않는다. `text-scale`로 화면 이미지 전체를 강제로 2.8배 확대하지 않는다.
별도 베젤·노치·기기 그림자·테두리 두께·추가 모서리 규격은 원본에 없으므로 현재 정의하지 않는다. 움직임은 원본 바깥 래퍼에 적용한다.

## 5 parts

`parts-source = src/parts/`, `parts-scale = uniform`. HTML의 원본 치수·글자·여백·선·모서리를 유지하고 바깥 래퍼를 등비 변환한다.
모션 텍스트의 `text-scale`은 parts 내부 글자에 다시 적용하지 않는다. 부품의 영상 속 표시 배율·위치는 레퍼런스가 정한다.

| 부품 파일 | 원본 크기(px) | 재사용 범위 |
|---|---|---|
| `mission-card.html` | 350×253 | 기존 미션 카드·진행 바 |
| `skill-card-1.html`, `skill-card-2.html`, `skill-card-3.html` | 각 350×88 | 기존 스킬 카드·캐릭터 |
| `asset-card.html` | 350×74 | 기존 `data-state` 4개 |
| `button-submit.html` | 350×44 | 기존 문구 상태; `apply-case` 폭 318은 원본 HTML 예외 |
| `badge-approved.html` | 108×32 | 기존 승인 배지 |
| `stat-card.html` | 350×101 | 기존 수익 카드·숫자 대상 |
| `brand-icon.html` | 98×98 | 기존 앱 아이콘·내부 원본 심볼 |
| `logo.html` | logotype 149×19 / symbol 100×100 | 기존 black·white 변형; 밝은 바탕에는 black 변형 |

원본 목록·치수·에셋은 `design.md` §6 기준이며 부품별 예외는 실제 HTML을 확인했다.
로고·캐릭터는 `src/assets/` 원본을 그대로 사용한다. 새 도형으로 대체하거나 로고를 텍스트로 재작성하지 않는다.
움직임은 `motion.md`의 해당 Pattern을 연결한다. 이 파일의 토큰은 영상 외형만 정의하며 레퍼런스의 구성 결정을 대신하지 않는다.
