# layout-demos

웹에서 자주 쓰이는 레이아웃 패턴을 직접 만들어 본 데모 모음이다. 같은 구조를 여러 분위기로 바꿔 보며 디자인 참고용으로 쓴다.

사이트: https://layout-demos-ivory.vercel.app

데모에 나오는 작품, 인물, 행사, 건축물, 수치는 모두 지어낸 것이다. 실제 지명이 들어가도 실재하는 대상을 가리키지 않는다.

## 데모

| 데모 | 레이아웃 | 문서 |
|---|---|---|
| [cinema-reel](cinema-reel/) | 세로 무한 릴을 어두운 영화제 포스터 카드로. 넘기는 동안 글자가 흐려졌다가 멈추면 맺힌다 | [docs/cinema-reel.md](docs/cinema-reel.md) |
| [reel-exhibition](reel-exhibition/) | 같은 릴을 밝은 사진전 도록으로. 사진은 잘려 들어오고 글은 줄마다 올라온다 | [docs/reel-variants.md](docs/reel-variants.md) |
| [reel-festival](reel-festival/) | 같은 릴을 축제 라인업으로. 화면 가득 듀오톤, 이름이 가로로 휩쓸려 온다 | [docs/reel-variants.md](docs/reel-variants.md) |
| [reel-index](reel-index/) | 같은 릴을 건축 아카이브로. 목록이 드럼처럼 돌고 사진이 위에서 열린다 | [docs/reel-variants.md](docs/reel-variants.md) |
| [reel-minimal](reel-minimal/) | 흰 바탕 세로 스택. 가운데 사진만 넓게 펼쳐지고 상세 보기에서 크게 넘겨 본다 | [docs/reel-minimal.md](docs/reel-minimal.md) |

## 로컬에서 보기

저장소 루트에서 정적 서버를 띄우고 `http://localhost:4411/`을 연다. `file://`로 열면 일부 브라우저가 로컬 이미지나 글꼴을 막는다.

```bash
python3 -m http.server 4411
```

## 구성

- 폴더 하나가 데모 하나다. 빌드 도구 없이 `index.html` 한 파일로 돈다. 외부 의존은 웹 글꼴 CDN뿐이다
- 이미지는 `<데모>/images/`에 WebP로 둔다
- 구조·동작·바꿔 쓸 곳·사진 출처는 `docs/<데모>.md`에 있다
- 데스크톱, 모바일(375px), `prefers-reduced-motion`을 지원한다

## 사진

모두 [Unsplash License](https://unsplash.com/license) 무료 사진이다. 사진가 표기는 각 문서의 「사진 출처」에 있다.
