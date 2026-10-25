# 영상 제작 기록 — video-A-r1

최종 파일: `final.mp4` · **무음** · 독립 critic의 최종 영상 판정 전 제작·검증 자료다.
승인된 A 전체 경험 소개를 그대로 렌더했다. 새 문구·UI·배치·효과·오디오를 추가하지 않았다.

## 승인 및 입력 바인딩

- 장면표 `scenes-A-r1`: 사용자 “승인 · 스냅보드 제작”, 현재 scenes SHA256 일치.
- 스냅 `snap-A-r2`: 사용자 “승인 · 영상 제작”, 현재 manifest/board/6 PNG/25 렌더 입력 SHA256 일치.
- 시작 확인: `approval-verification-video-r1.json`. 렌더 후에도 baseline 51개와 렌더 입력 25개 모두 불변.
- 승인 manifest SHA256: `83fa940af9ae22405b303cb8dcb3ad75ce1e5d2f31a84cc8d593d0d61eb4c12c`.
- 승인 source aggregate SHA256: `13e4891e97304b1b5a582a644500b458e2d732778b8b6f8cf6753e9d7ea43098`.
- 렌더 입력·기준 docs/src/harness·state·승인·실패 기록을 수정하지 않았다. 캐시는 입력 바인딩에서 제외한다.
- A4 `video/compositions/a4.html:29`는 `move-part × 390/108`, A5 `a5.html:29`는 `move-part × 780/350`을 유지한다. 기존 `token-bindings.json` 참조.

## 실제 렌더

```sh
# video/에서 실행
HYPERFRAMES_SKIP_SKILLS=1 HYPERFRAMES_NO_UPDATE_CHECK=1 npx --yes hyperframes@0.8.198 render --output ../final.mp4 --quality delivery --fps 30 --format mp4 --strict
```

- 로그: `render-video-r1.log` · exit 0 · 26.1초. `delivery`는 CLI의 `high` 품질로 해석됐다.
- 로컬 HeadlessChrome screenshot 캡처·Apple GPU·3 workers. iframe 호환성으로 screenshot 선택과 worker 4→3 조정 경고가 기록됐으며 실패가 아니다.
- 원본 HTML parts/로컬 Pretendard 폰트의 로드를 기다린 뒤 600프레임을 캡처했다. 소스를 재작성하지 않았다.
- 미리보기는 사용자가 이미 확인한 `http://127.0.0.1:3002/#project/video`이며 편집 가능한 소스는 `video/`에 있다.

## 출력·디코딩 검증

- ffprobe `-count_frames -show_format -show_streams`: `ffprobe-video-r1.json`.
- 실제 출력: 16:9 · 1920×1080 · 30/1fps · 20.000000초 · `nb_frames=600`, 디코딩 계수 `nb_read_frames=600`.
- H.264 High / yuv420p / BT.709 / progressive / 비디오 1개 / 오디오 0개 / 974,430 bytes.
- FFmpeg로 전체 파일을 null 출력까지 디코딩했다. exit 0, `decode-all-video-r1.log` 오류 없음.
- 최종 MP4에서 직접 40개 표본을 디코딩했다. HTML 캡처를 영상 증거로 재사용하지 않았다. 인덱스/시각/개별 SHA256: `verification-video-r1.json`.
- `select=eq(n,...)`로 정확한 프레임을 선택했다. 파일은 `decoded-video-r1/fNNN-tSS.SSSSSS.png`, 오류 로그는 `extract-video-r1.log`.
- `decoded-sheet-1.jpg`, `decoded-sheet-2.jpg`를 열어 전체 표본을 확인했다. A2 5초·A3 9초·마지막 프레임은 전체 해상도로 별도 확인했다.
- 안정 6장면(f45/150/270/390/480/570)을 승인 PNG와 비교했다. H.264 손실 압축 때문에 byte 동일성은 요구하지 않았다. RGB 평균 절대 오차는 채널별 최대 0.289/255 미만이며 승인 구성·텍스트·원본 재료가 보인다.
- 마지막 f599(19.966667초)는 정지된 원본 로고다. 승인 A6 PNG와의 압축 오차 및 실제 전체 크기 이미지도 확인했다.

## 동작과 레퍼런스 대응 증거

| 구간·의도 | 실제 MP4 표본 | 확인 범위 |
|---|---|---|
| REF-01 큰 구성 | f45→150→270→390→480→570 | 도입→스킬→미션→검토 후 결과→자산→브랜드 순서 |
| REF-02 단일 세로 화면 | f75/81/98/120/150 | 원본 전체 화면의 정면 구도와 종횡비 유지 |
| REF-03 실제 버튼 주목 확대 | f207/222/237/270 | `cam-in` 진입 중/안착 후를 비교, 원본 버튼만 확대 |
| REF-04 세로 가림막 | f435/441/447 | A4→A5 상단부터 가리는 중간/완료 프레임 |
| REF-05 중앙 읽기 | f45/480 | A1·A5 중앙 정렬 문구와 정지 상태 |
| REF-06 화면 상승·안착 | f75/81/98/120/150 | 아래→원위치·0.8→1·감속·안착 후 정지 |
| A4/A5 원본좌표 이동 | f327/333/339 및 f447/453/459 | 등장 전·중간·후. r2 독립 캡처와 중간 프레임 비교도 JSON에 기록 |
| 마지막 고정 | f522/570/599 | 로고 등장 후 위치·배율 유지, 마지막 출력 프레임에 존재 |

소스 타임라인의 토큰·곡선·프레임 경계와 위 디코딩 표본을 연결했다. 이 표는 critic에게 제공하는 증거이며 자기 PASS·반영률 최종 판정이 아니다.
자동 motion 검사는 `check-snap-r2.json`에서 `enabled=false, samples=0`이다. 자동 동작 검증을 통과했다고 표시하지 않는다. 연속 재생을 사람이 직접 본 것으로도 주장하지 않는다.
동일 승인 소스의 lint/runtime/layout 오류 0 검사는 스냅 r2 결과를 참조했다. 소스를 바꾸지 않아 목적 없는 재검사를 반복하지 않았고 새 출력 디코딩·승인 일치 여부를 확인했다.

## 무음 및 남은 검토 범위

- `.agents/skills/hyperframes/references/sound.md` §3의 무음 납품 fallback을 따른다. 사용자 제공 음원·사용 가능한 heygen CLI·기존 로컬 음악 공급원이 없다.
- 음악/효과음/내레이션/새 자막은 없고 ffprobe에서도 오디오 스트림 0개다. 음량·청음 검증을 수행했다고 주장하지 않는다.
- 추후 제공되는 사용 권한 있는 음원을 편집 소스에 별도 오디오 트랙으로 연결할 수 있다. 현재 승인된 시각 타이밍을 임의로 바꾸지 않았다.
- 첫 프레임이 배경만인 경고는 승인된 A1 alpha 등장 방식 때문에 유지했다. 무음 경고는 위 fallback으로 명시했다.
- A2 상단 제목은 승인 L2 위치를 유지한다. 작은 화면에서는 원본 UI 세부 텍스트보다 별도 헤드라인을 읽는 영상이다. 미세 UI 전체를 필수 읽기 대상으로 간주하지 않는다.
- 독립 critic이 실제 영상·소스·이 기록으로 최종 판정한다. 새 수치 후보 없음.

최종 MP4 SHA256: `f005af1ffe9e04a7c426a837fe395c11f019fa07551c45e6e7779a18cba330c5`.
