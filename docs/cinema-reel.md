# Cinema Reel — 세로 무한 릴

- 데모: [`cinema-reel/index.html`](../cinema-reel/index.html)
- 원본: [Motionin · Cinema Reel](https://motionin.design/gallery/cinema-reel). 구조와 동작만 가져왔고 작품·문구는 새로 썼다

## 구조

화면 높이(100dvh) 섹션 하나 안에서 80dvh 포스터 카드가 세로로 끝없이 돈다. 카드 한 장이 작품 하나다.

```txt
┌ section 100dvh ─────────────────────────────────────┐
│ ▒ 위 페이드 10dvh                                     │
│  ┌ 카드 80dvh, radius 48px ─────────────────┐  01   │
│  │ 연도                            ★★★★★    │  ●    │
│  │ 영화제 · 부문                   상 이름   │  ·    │
│  │ (수상작)                        "한 줄 평" │  ·    │
│  │ 장르                                      │  ·    │
│  │ 큰 명조 제목                              │  ·    │
│  │ 감독 / 장르                     [자세히 →] │  06   │
│  └───────────────────────────────────────────┘       │
│ ▒ 아래 페이드 10dvh       (다음 카드가 살짝 보인다)    │
└──────────────────────────────────────────────────────┘
```

[자세히]를 누르면 오른쪽에서 상세 패널(카드와 같은 높이·둥근 정도)이 밀려 나온다. 뒤의 카드는 막(blur 14px, 어둡게 35%)에 덮이고 레일은 숨는다. 패널 순서는 포스터, 영화제, 제목, 감독·개봉·장르, 시놉시스, 수치 표, 평, 사진 출처다.

## 동작

- **글자 레이어:** 카드가 중앙에서 멀어진 거리 d(카드 단위, 0~1)에 따라 `opacity 1 → 0`, `blur 0 → 16px`가 되고 카드와 반대로 0.4배 밀린다. 넘기는 동안은 포스터만 지나가고, 멈추면 글자가 다시 맺힌다
- **넘기기:** 자동 넘김은 없다. 드래그(놓으면 가까운 카드로 붙고, 0.5px/ms보다 빨리 튕기면 한 장), 휠 한 번에 한 장(650ms 잠금), ↑↓·PageUp/Down, 레일 점
- **무한 반복:** 카드 i의 위치를 `i - pos`로 계산해 `[-N/2, N/2)`로 감싼다. 카드를 복제하지 않는다
- **모션 블러:** 스냅으로 프레임당 6px 넘게 움직일 때만 SVG `feGaussianBlur`의 세로 값을 키운다(최대 12px)
- **포스터:** 카드와 반대로 0.06배 움직여 깊이를 준다
- **커서 링:** 마우스가 카드 위에 있을 때만 104px 링이 따라온다. 끄는 동안은 위아래 화살표로 바뀐다. 커서는 숨기지 않는다
- **상세 패널:** 700ms `cubic-bezier(.22,1,.36,1)`. ×, Esc, 뒤의 막으로 닫는다. 열리면 닫기 버튼으로 포커스가 가고, 닫혀 있을 때는 `inert`다
- **reduced motion:** 블러·밀림·링 없이 즉시 이동한다. 글자는 활성 카드에만 보인다
- **모바일(720px 이하):** 평 목록과 링을 숨기고, 레일이 겹치지 않게 오른쪽 여백(`--pad-r`)을 넓힌다

## 서체

| 역할 | 서체 |
|---|---|
| 제목·평 (`--display`) | 나눔명조 800/700. 라틴 글자와 따옴표는 Cormorant Garamond가 먼저 잡는다 |
| 시놉시스·수치 (`--myeongjo`) | 나눔명조 |
| 연도·레일 번호 (`--num`) | Cormorant Garamond 이탤릭 |
| 라벨 (`--sans`) | Pretendard 12~13px, 자간 0~0.04em |

- 한글에는 이탤릭을 주지 않는다. 한글 글꼴에는 이탤릭이 없어서 브라우저가 억지로 기울인다
- 「1시간 34분」처럼 숫자와 한글이 섞인 값은 한 서체로 쓴다. Cormorant 숫자는 한글보다 작아 보인다
- 한글 라벨에 라틴 대문자용 넓은 자간(0.12em 이상)을 옮기지 않는다

## 바꿔 쓸 곳

- `FILMS` 배열: 작품 정보, 상세 패널의 `stats`, 사진(`photo` = `images/` 안 파일 이름), 출처(`credit` = `[사진가, 사진 페이지 ID]`), 사진이 없을 때 보일 그라디언트(`bg`)
- `:root` 토큰: 배경, 글자색, 포인트색(`--gold`), 여백(`--pad-x`, `--pad-r`), 둥근 정도, 이징, 서체 역할
- `frame()` 계수: 글자 사라지는 속도 `1.6`, 블러 최대 `16`, 반대로 밀리는 비율 `0.4`, 스냅 속도 `0.085`
- Next.js로 옮길 때는 페이지 스크롤 연출이 아니라 섹션 안의 자체 애니메이션이라 GSAP 없이 `requestAnimationFrame`으로 충분하다

## 주의

- 섹션은 `overflow: clip`이다. `hidden`이면 화면 밖 요소에 포커스가 갈 때 섹션이 스크롤돼 릴 전체가 옆으로 밀린다(실제로 452px 밀렸다)
- 섹션에 `touch-action: none`과 휠 `preventDefault()`가 걸려 있다. 상세 패널은 휠 핸들러에서 돌려보내고, `.d-scroll`에 `touch-action: pan-y`와 `overscroll-behavior: contain`을 줘야 스크롤된다
- `requestAnimationFrame`은 화면에 그려지지 않는 탭에서 멈춘다. 미리보기 창이 숨어 있으면 데모가 멈춘 것처럼 보인다

## 사진 출처

[Unsplash License](https://unsplash.com/license) 무료 사진. 가로 1800px WebP로 줄여 `cinema-reel/images/`에 두었다. 실제 서비스에 쓸 때는 교체한다.

- `tide.webp` — [@tdeley](https://unsplash.com/photos/U--xvXIPwgI)
- `alley.webp` — [@dawidtkocz](https://unsplash.com/photos/etPPKLHtcGo)
- `station.webp` — [@timoun](https://unsplash.com/photos/-gsb-Q4RrGU)
- `forge.webp` — [@joszwaan](https://unsplash.com/photos/vRUIHN1iW2s)
- `cave.webp` — [@iswanto](https://unsplash.com/photos/KXtMGheovdw)
- `lighthouse.webp` — [@timberfoster](https://unsplash.com/photos/luyqrtZaWmg)
