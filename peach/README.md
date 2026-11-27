# 🍑 준혁 & 미미 — 웨딩피치 MB-3 감성 모바일 청첩장

- 주소: https://mimipark97.github.io/wedding/peach/
- 기존 청첩장(`/wedding/invite/`)은 그대로 두고, 별도 폴더로 추가했습니다.

## 구성

| 경로 | 내용 |
|---|---|
| `index.html` | 완성본 (웹폰트 base64 내장 · 이 파일 하나로 열림) |
| `images/` | 사진 23장(10번·18번 제외, 전부 원본 해상도) + 히어로 1장 + 몬치치 부부 폴라로이드(webp) + 약도 + 아기 사진 스티커(webp) + 종이 텍스처 2장 |
| `audio/bgm.mp3` | 배경음악 (기존 청첩장과 동일한 곡) |

## 손보는 곳

### 부모님(가족) 사진 넣기
`index.html`에서 `images/gallery-01.jpg` 를 쓰는 `figure.ph.ph-a` 한 곳입니다.

```html
<!-- [부모님 사진 자리] 아래 img 의 src 를 양가 부모님(또는 가족) 사진으로 바꾸면
     이 자리에 그대로 들어갑니다. 세로 사진이 가장 잘 맞습니다. -->
<figure class="ph ph-a">
  <img src="images/gallery-01.jpg" alt="나란히 누워 웃는 두 사람">
  <figcaption>just us</figcaption>
</figure>
```

- 사진을 `peach/images/parents.jpg` 로 올리고 `src="images/parents.jpg"` 로 바꾸면 끝입니다.
- 아기 사진 스티커(`images/sticker-groom.webp`, `images/sticker-bride.webp`)은 그대로 두면 됩니다.

### 사진 출처 (원본)
모든 사진은 퍼스트레터 라이브 청첩장(`https://ourfirstletter.com/w/270130-6AKDkf`)의 **원본 고해상도 파일**을 그대로 받아 씁니다(보정본 아님).
- `hero.jpg` = 원본 1749×2624 → 1600px 폭, JPEG q92
- `gallery-*.jpg` = 원본 2000~3000px → 폭 1200px(가로 사진은 1800px), JPEG q92
- 원본이 853px 폭인 사진 2장(`gallery-15`, `gallery-17`)은 원본 자체가 그 해상도입니다.
- 다시 받으려면 `/workspace/work/peach-build/rebuild_photos.py` (원본은 `/workspace/work/peach-hires/orig/`).
- 하트 로켓 클로즈업 2장(`locket-bride.jpg`, `locket-groom.jpg`)은 2026-11-28에 `attachments/2026-11-28/IMG_1996`에서 얼굴 중심 700×700 흑백으로 다시 잘랐습니다 — 크롭 좌표는 `/workspace/work/peach-build/crop_locket_1996.py`, 예전 크롭 스크립트는 `make_lockets.py`. **2026-11-28에 하트 로켓 목걸이 장면을 걷어내면서 페이지에서는 더 이상 쓰지 않습니다**(파일만 남겨 둠).
- 중단 '우리의 순간' 장면은 `images/monchhichi.webp`(원본 `downloads/monchhichi-wedding-couple.png`, 700×649 q90)를 마스킹테이프를 붙인 흰 테두리 폴라로이드(`figure.monchhi`)로 배치하고, 사진 아래 여백에 `가장 귀여운 우리의 순간` 문구를 넣습니다.

### 아기 사진 바꾸기
`downloads/baby-demo/` 의 원본을 흰 배경 제거 + 외곽선 스티커로 만든 파일입니다.
원본을 바꾸려면 작업 폴더 `/workspace/work/peach-build/gen_assets.py` 의 `make_sticker()` 를 다시 돌리면 같은 모양으로 다시 만들어집니다(핫핑크 = `(255,46,111)`, 피치 = `(255,138,76)`).

### 문구·계좌·연락처
`index.html` 안에서 그대로 검색해 고치면 됩니다. 계좌 6건 · 전화 2건은 기존 청첩장 값과 동일합니다.

## 원본 소스
편집용 원본은 `/workspace/work/peach/index.src.html` (폰트는 `fonts/` 파일 참조)이고,
`python3 /workspace/work/peach-build/build.py` 로 폰트를 내장해 `index.html` 을 다시 만듭니다.

### 문구를 고쳤을 때 (중요)
내장 폰트는 페이지에 나오는 글자만 담은 **서브셋**입니다. 문구를 추가·수정하면 새 글자의 글리프가 없어
아이폰 사파리에서 그 글자만 시스템 고딕으로 떨어집니다(글씨체가 들쭉날쭉해 보이는 원인).
그래서 `index.src.html` 을 고친 뒤에는 **반드시 이 순서로** 다시 빌드하세요.

```
python3 /workspace/work/peach-build/build_fonts.py   # 새 문구 기준으로 7개 폰트 서브셋 재생성 (누락 0건 검사)
python3 /workspace/work/peach-build/build.py         # 서브셋을 base64로 index.html 에 내장
```
