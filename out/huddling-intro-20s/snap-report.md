# Snapshot build report

revision: **snap-A-r2** · 승인 입력: **scenes-A-r1**.
이 문서는 builder의 제작·관찰 기록이다. 독립 critic 판정이나 사용자 스냅보드 승인이 아니다.

## 산출물

- 편집 가능한 HyperFrames 소스: `video/index.html`, `video/compositions/a1.html`~`a6.html`.
- 장면별 실제 렌더: `snap/A1.png`~`snap/A6.png` (각 1920×1080).
- 장면·시각 라벨이 있는 모아보기: `snap/board.png`. CLI 원본 모아보기: `snap/contact-sheet.jpg`.
- 실제 캡처 시각: A1 1.5s / A2 5s / A3 9s / A4 13s / A5 16s / A6 19s.
- 등장·확대·가림막·마지막 프레임 증거: `snap/evidence/`의 실제 PNG 12장과 contact sheet 2장.
- 소스·이미지 해시: `snap/manifest.json`. 실제 구성 HTML과 선언된 로컬 의존성·필수 설정 25개만 바인딩한다. `.thumbnails`·`.hyperframes`·node_modules·미사용 에셋·검사 sidecar·안내문·빌드 helper는 제외한다.
- 렌더 입력 aggregate SHA256: `13e4891e97304b1b5a582a644500b458e2d732778b8b6f8cf6753e9d7ea43098` (정렬된 `source_hashes` JSON의 SHA256).
- 토큰과 원본 연결: `token-bindings.json`. 실제 클립 해석: `timeline-snap-r1.json`.
- 로컬 편집 미리보기: `http://127.0.0.1:3002/#project/video`. 관리형 background preview는 검토 동안 유지한다.

## 원본과 구현 방법

`src/`를 `video/assets/source/`에 바이트 그대로 복사했다. PNG 내부 UI는 재작성하지 않았다.
버튼·배지·자산 카드는 원본 HTML을 같은 출처 iframe으로 로드하고 바깥 래퍼만 등비 fit했다. 원본 CSS·문구·상태·에셋 경로를 유지했다.
모션 텍스트와 카메라 래퍼는 분리했다. 폰트 세 굵기는 원본 로컬 OTF이며 캡처 CLI가 3개 폰트 로드를 확인했다.
전체 화면과 부품, 제목의 위치·크기는 승인 장면표의 L1~L6 산식을 사용했다. 신규 표현 수치를 제안하거나 기준 문서를 수정하지 않았다.
원본 docs/src/harness와 승인 당시 51개 입력의 해시는 제작 전후 동일하다. state/승인 기록은 builder가 수정하지 않았다.

## 실제 관찰

- 전체 해상도 A1~A6 PNG를 열어 글자·줄바꿈·원본 비율·프레임 내 표시를 보았다. 잘림·제목과 부품의 겹침은 관찰되지 않았다.
- A2 상단 제목은 승인된 배치대로이며 전체 PNG와 분리된다. 일반 안전영역 가이드로 임의 이동하지 않았다.
- A3 제출 버튼·A4 승인 배지는 실제 원본 HTML의 렌더다. 클릭·업로드 성공·즉시 자동 승인을 추가하지 않았다.
- A5는 16초 전체 PNG에서 제목·원본 카드·메타데이터의 정상 표시를 직접 확인했다.
- 2.5/2.7/3.25/4초 표본에서 A2의 확대·안착을, 6.9/7.4/7.9초 표본에서 실제 버튼 래퍼의 확대를 확인했다.
- 14.7/14.9/15.3초 표본에서 들어오는 A5와 원본 카드의 표시를 확인했다. 연속 영상의 전환 속도 검증을 대신하지 않는다.
- 마지막 출력 프레임에 해당하는 19.966667초 표본에서도 원본 로고가 고정돼 보인다.

## 자동 검사와 범위

`check-snap-r2.json`: HyperFrames 0.8.198, browserSkipped=false, ok=true. r1 로그도 보존했다.
lint/runtime/layout/contrast 오류 0, 자동 contrast 확인 4개 통과. r2 레이아웃 표본은 1.5·5·9·11.1·13·15.1·16·19초다.
**motion.enabled=false, samples=0**이다. 하위 composition sidecar는 이 CLI의 프로젝트 루트 검색에서 실행되지 않았다. 동작 자동 검증을 통과했다고 주장하지 않는다.
contrast의 자동 표본에는 A5 16초가 없으므로 A5 대비는 다음 원본 CSS의 상대 휘도 계산과 실제 PNG 관찰로 보완했다.

| A5 대상 | 근거 색 | 계산 대비 | 실제 표시 크기·적용 기준 |
|---|---|---|---|
| 독립 제목 | video-text / video-bg | 17.592:1 | video-head, 큰 글자 기준 이상 |
| 카드 제목 | color-text / color-surface | 16.432:1 | 원본 15px × 780/350 ≈ 33.43px, 큰 글자 기준 이상 |
| 카드 메타데이터 | color-text-muted / color-surface | 4.186:1 | 원본 12px × 780/350 ≈ 26.74px, 큰 글자 기준 이상 |

이는 `docs/video.md`, 원본 `src/parts/asset-card.html`의 색을 계산한 결과이며 A5가 자동 contrast 검사에 포함됐다는 뜻은 아니다.

검토한 craft 경고:
- `first_frame_empty`: 승인된 A1의 alpha 진입 시작점 때문에 프레임 0은 배경이다. 승인된 구성은 바꾸지 않았다.
- `film_silent`: 현재는 스냅보드 소스 단계이며 음악·효과음·MP4는 제작하지 않았다. 영상 단계에서 처리할 범위다.

## snap-A-r2 단일 원인 수정

독립 critic의 첫 불합격 원인 MOVE-UNIT을 두 곳에서 수정했다. 기존 등비 fit은 iframe에 적용되므로 그 바깥 motion wrapper의 y도 원본 이동에 fit을 곱해야 한다.
- `video/compositions/a4.html:29`: `move-part × (390/108)` = 약 86.6667 출력 px.
- `video/compositions/a5.html:29`: `move-part × (780/350)` = 약 53.4857 출력 px.
- 새로운 토큰·길이·곡선·정적 배치 변경은 없다. 같은 계산을 빌드 helper에도 반영했으며 다른 구성 파일은 재생성하지 않았다.
- 실제 새 중간 표본: `snap/r2-verification/frame-03-at-11.1s.png`, `frame-05-at-15.1s.png`. 두 파일을 열어 확인했다.
- 이전 표본과의 차이 영역은 A4 `[1245,601,1635,725]`, A5 `[570,733,1350,902]`로 해당 부품 등장 위치에 한정된다.
- 여섯 안정 시점을 다시 캡처해 기존 `snap/A1.png`~`A6.png`와 RGB 픽셀을 비교했고 전부 동일하다. 기존 6장과 board 파일은 변경하지 않았다.
- 비교 결과: `snap/r2-verification/verification.json`. 이전 manifest·보고서·토큰 바인딩·영향 소스·등장 표본은 `snap/audit/snap-A-r1/`에 보존했다.
- 렌더 입력 25개의 경로 집합은 그대로이며 변경된 입력은 a4.html·a5.html 두 개뿐이다. 캐시는 계속 해시 바인딩에서 제외한다.

## 이후 검증

스냅보드의 외형과 source 해시에 대해 독립 critic 및 사용자의 확인이 필요하다.
승인 후 영상 단계에서 실제 동작·곡선·전환 연속성·오디오·인코딩·600프레임 출력을 다시 검증한다.
현재 MP4는 생성하지 않았다.
