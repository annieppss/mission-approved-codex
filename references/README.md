# 모션 기준 분석 자료

2026-10-25에 사용자가 지정한 두 공개 레퍼런스를 읽기·분석 목적으로 받았다.
원본 영상은 구현에 포함하거나 저장소에 업로드하기 위한 에셋이 아니다.

## R1 — Remotion Device Rise

- 페이지: https://www.remotion.dev/prompts/apple-style-device-rise-animation
- 공개 플레이어 원본: https://stream.mux.com/01xyau8x7RJaSfRC9R00IP9GUlIkTwStYqKrI6XEC3wro.m3u8
- 로컬 문서: `remotion-device-rise/source.html`
- 원본: `remotion-device-rise/original.mp4`, 1280×720, 30fps, 121 video frames. 컨테이너 길이 4.117333초.
- `contact-sheet.jpg`: 0초부터 0.1초 간격, 왼쪽→오른쪽·위→아래. 6열, 마지막 빈 칸 제외.
- 원문은 모션 시스템 규격이 아닌 프롬프트 문서다. 명시된 움직임 값과 시각적으로 보이는 원본을 구분했다.
- 0–1.5초: 감속하는 기기 상승. 1.5초 이후: 카드가 차례로 나타남. 약 2.4초 이후: 모두 안착 후 정지.

## R2 — Skillry Nafa Original

- 페이지: https://skillry.dev/ai-videos/opus-5-5?category=motion&video=saadpastadev-347341
- 공개 Original 소스: https://media.skillry.dev/opus-5-5/saadpastadev-347341/original.mp4
- 원본: `skillry-nafa/original.mp4`, 1280×720, 30fps, 736 video frames. 컨테이너 길이 24.554초.
- `contact-sheet.jpg`: 0초부터 0.5초 간격, 5열.
- `opening-detail.jpg`: 0초부터 0.2초 간격, 6열.
- `transition-detail-offset12s.jpg`: 12초부터 0.2초 간격, 6열.
- `typing-offset5.2s-step1f.jpg`: 5.2초부터 1f 간격, 6열.
- `wipe-offset16.3s-step2f.jpg`: 16.3초부터 2f 간격, 6열.
- `swipe-offset13s-step2f.jpg`: 13초부터 2f 간격, 6열.
- 샘플 시각은 대략 시작시각 + 표본번호×간격이며 fps 필터의 표본 선택으로 최대 1f 정도 차이 날 수 있다.
- 약 5.37 / 5.50 / 5.63 / 5.77초: 입력 글자가 늘어남. 약 4f 간격.
- 약 13.3–13.7초: 확인 컨트롤 진행. 약 12f 동작으로 정규화.
- 약 15–16초: UI를 향한 확대. 약 1초를 서비스 카메라 토큰으로 채택.
- 약 16.0–16.5초: 가림막 전환. 짧은 전환 토큰은 원본을 복제하지 않고 12f로 정규화.
- 약 16.43–16.63초: 짧은 문구 등장. 약 6f를 빠른 교체 토큰으로 채택.
- 약 22.5–24.5초: 엔딩 유지. 최소 2초로 정규화.

## 해석 한계

- R2의 소스 코드·easing·정확한 키프레임은 공개 플레이어만으로 알 수 없다. 관찰값을 코드값으로 표기하지 않았다.
- 원본에서 숫자를 직접 확인할 수 없는 곡선·크기는 출처 재사용/파생/채택으로 명시하거나 새 후보로 보류했다.
- `docs/motion.md`의 참조 시각은 근거를 찾는 위치다. 재생 순서와 화면 배치는 이 자료나 motion.md에서 지정하지 않는다.
