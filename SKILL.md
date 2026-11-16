---
name: hongdo-pencil
description: 김홍도 풍속화 + 연필 스케치 융합 화풍으로 그림을 그릴 때 사용 — "김홍도 스타일로 그려줘", "연필 그림체로 그려줘", "풍속화 그림", "한지 그림", "pencil sketch drawing of X" 등. 자기완결 HTML/SVG 아티팩트로 한지 바탕 + 수묵 산수 배경 + 연필 선묘 주제 그림을 만든다. 도면/대시보드/웹사이트 등 다른 아티팩트 요청에는 사용하지 말 것.
---

# 김홍도 풍속화 × 연필 스케치 융합 그림

한지(韓紙) 바탕 위에 **배경은 수묵 산수화**, **주제는 연필 선묘**로 그려 두 화풍을 한 장에 융합한 그림을 만든다. 산출물은 오프라인 자기완결 HTML 아티팩트 한 장.

## 진행 순서

1. **먼저 artifact-design 스킬을 로드**한다 — 아티팩트 호스트 규칙(artifacts/&lt;name&gt;.html 경로, 오프라인 CSP, 테마 리스너, 토큰 규칙)을 따라야 한다.
2. 소재(무엇을 그릴지)와 배경 요청(강/산/마을 등)이 없으면 한 번만 질문한다. 소재는 현실 사물(기차, 배, 시장, 소, 파도 등)이어야 하며 시대가 섞여도 좋다 — 조선 풍속화 소재 + 근대 사물의 조우가 이 화풍의 재미다.
3. `artifacts/<소재명>.html`에 작성한다. `<title>`은 전통화 제목처럼 짧은 한 단어 명사(예: 기차도)로 한다.
4. 아래 레시피대로 그린다. **참고 구현(원형)**: `C:\Users\KYH\Documents\artifacts\gichado.html` — 기차도(기본), 같은 폴더에 렌더 사진 `gichado.png`(1600×1200). 변주 원형: `mongcheoldo.html`(초현실), `cheolmasimsang.html`(추상). 애매하면 기차도 파일을 읽고 좌표·필터값을 참고한다. 완성한 그림을 사진 파일로 달라고 하면 아래 "사진 파일(PNG) 내보내기" 절차를 따른다.

## 화풍 DNA — 두 극과 융합 규칙

**A. 김홍도(단원) 풍속화 쪽**: 한지 바탕(따뜻한 담황색), 넓은 여백, 평면적이고 단순한 윤곽선, 활기찬 인물 한두 명의 일상 동작, 채색 포인트, 세로 한자 제목 「○○圖」 + 4자 낙관 시구 + 붉은 사각 도장.

**B. 연필 스케치 쪽**: 모노크롬 흑연 톤, 떨리는 손선, 해칭(음영선), 길게 흐르는 관찰선, 사실적 명암, 종이의 거친 질감.

**융합 규칙**:
- **주제**(기차·배·사람 묶음 등)는 연필 선묘로, **배경**(산·안개·하안·하늘)은 수묵 세척으로. 화면 안에 두 매체가 명확히 나뉘되 한지 바탕이 묶는다.
- 종이 질감(grain 필터)과 가장자리 비네트는 필수다 — 둘을 같은 종이 위에 놓는 근거.
- 전통 요소(제목·낙관·도장)로 A의 정체성을 확보한다. 도장 한자는 소재에서 2자를 뽑는다: 기차→鐵馬, 배→漁舟, 파도→波濤, 소·마을→田野. 도장은 붉은 사각(#b23a26)에 흰 글자 2자 세로.
- 인물은 단원 풍처럼 단순 윤곽(먹선)으로 그리고 채색 포인트는 **최대 1곳**(옷 한 군데 등)까지만 허용한다. 나머지는 무채 수묵+흑연만.
- 채색을 늘리지 않는다 — 이 화풍은 "먹과 흑연"이며 색은 종이색과 도장 붉은색뿐이 기본.

## 캔버스와 레이어 순서 (viewBox 1600×1200)

아래 순서대로 아래→위로 쌓는다:

1. 한지 바탕 rect (`--paper`)
2. 원근 산 3겹: far/mid/near — 각각 linearGradient 수묵(wash 톤→투명) + `soft4` blur, 능선은 같은 곡선을 열린 path로 `brush` 필터 스트로크. near 산 얼굴에는 준법(cun) 스트로크(짧은 곡선, 능선 따라 경사각)
3. 안개: `--paper-hi` 타원 + blur 28, 산기슭·수면 위 2~3개 (`.mist`, 천천히 좌우 drift)
4. 하안선 2겹(구불구불한 가로선) + 물결 — 짧은 `q` 곡선 스트로크 30개 내외(JS 생성, 소재/전경 회피)
5. 전통 소품: 강 위 작은 배+낚시꾼(죽모 갓), 갈대, 잔디 tuft — 산수화의 척도(스케일) 역할
6. 다리·기반물 (주제가 필요로 하면; 예: 석교 2아치)
7. **주제 (연필)** — 여기부터 `pencil` 필터 그룹. 해칭은 `hatch()`로, 바퀴·연기·깃발 등 반복 부품은 JS 생성
8. 전경 바위+소나무 (화면에서 가장 진한 먹 톤, 바위 위 소나무 1~2그루)
9. 제목·낙관·도장 — 우상단, 세로쓰기는 `tspan`으로 글자 하나씩 x/y 지정(제목 x≈1470, 낙관 시구 x≈1398/1352, 도장 58px)
10. grain rect + vignette rect (최상단)

구도 원칙: 주제는 화면 가로를 가로지르며(달리는/흐르는 방향), 강은 대각선 여백, 산은 상단 1/3, 전경 바위는 하단 한 구석. 여백을 채우려 들지 않는다.

## 필수 defs (그대로 복사해 쓰는 값)

- `pencil`: feTurbulence `turbulence` bf .045 octaves 2 → feDisplacementMap scale 4.5 (주제·트랙)
- `pencilLt`: bf .06 scale 3 (가벼운 선·제목)
- `brush`: bf "0.012 0.02" scale 8 (수묵 능선·배)
- `soft2/soft4/soft28`: feGaussianBlur
- `grain`: feTurbulence `fractalNoise` bf .5 stitchTiles → feColorMatrix alpha 0.06 (종이 입자)
- `washFar/washMid/washNear`: 세로 linearGradient — 상단 수묵색(op .5~.6) → 하단 투명
- `vig`: radialGradient, 가장자리 `--edge` op .18

## 팔레트 토큰 (:root, 모든 색은 여기서)

```
--paper:#f2e9d7  --paper-hi:#f8f1e2  --sand:#d9cdaf  --edge:#3a2f1c
--ink:#33302a    --pencil:#3b3a36    --pencil-2:#6f6b60  --pencil-3:#9c968a
--rock:#34372f   --pine:#31342c
--wash-far:#787b6e  --wash-mid:#5d6154  --wash-near:#3c4238
--seal:#b23a26
```

SVG 안 색은 인라인 `style="fill:var(--…)"` 또는 페이지 `<style>`의 클래스(`.ln .c-pen .f-rock` 등)로만 적용한다. 속성 attribute에 `var()`는 안 먹는다.

## 반복 요소는 JS 생성 (시드 고정)

`createElementNS` + 시드 난수(예: `seed=(seed*1664525+1013904223)>>>0`)로 그린다 — 재실행해도 같은 그림. 생성 대상: 침목·발라스트, 물결, 해칭 `hatch(cx,cy,w,h,angle,n,gap)`, 소나무 `pine(x,y,h,lean)`(가지 3 + 부채꼴 잎 fan 9선), 새(2곡선), 준법, 갈대, 잔디. 전체를 `try{...}catch`로 감싸 JS가 죽어도 정지 화면이 완성되게 한다.

## 움직임 (소재가 "달린다/흐른다"일 때만, 절제)

연기 상승(puff: translate+scale+fade, 음수 delay로 연속), 바퀴 회전(`.rot`, `transform-box:fill-box; transform-origin:50% 50%`), 속도선 dash 흐름, 열차 bob 1px, 안개 drift 26s. CSS keyframes + `@media (prefers-reduced-motion: reduce)`에서 전부 정지. **정지 상태도 완성된 그림이어야 한다** — 애니메이션은 양념.

## 마감 검증

- `getElementById` 대상 그룹 id가 마크업에 모두 존재(정확히 1개씩)
- 원격 URL 없음(CSP), `<title>`이 첫 8KB 안에, 토큰은 전부 bare `:root`에 선언
- 웹폰트 없음 — 제목/도장 한자는 `Gungseo, Batang, Georgia, serif` 스택(궁서체 느낌)
- 완성 후 사용자에게 아티팩트 링크 전달

## 초현실 변주 ("초현실 풍으로" 요청 시)

기본 레시피 위에 얹는 4개 요소 — 원형 구현: `artifacts/mongcheoldo.html`(몽중철마):

1. **떠 있는 산** — 산을 지면에서 분리해 공중의 섬처럼 배치(밑면을 울퉁불퉁한 암반 곡선으로 닫고, 아래 `soft28` 안개 타원). 큰 산 기슭에서 폭포 스트로크가 흘러내려 사라진다. 강에는 섬마다 옅은 반영 대시
2. **공중 주제** — 주제(기차 등)를 강/지면 위에 띄우고, 레일·길은 리본처럼 물결치다 허공에서 끝나게 한다(끝부분 나선 컬). `rotate(-4°)` 기울임 + 바퀴 아래와 리본 사이 종이 여백을 남겨 뜬 느낌 강조
3. **연기의 변신** — 굴뚝 연기 구름이 단계적으로 주제의 형상(잉어·새·용 등)으로 변해 하늘로 간다. 구름 3개(작→큰) → 반쯤 변한 형태 1개 → 완성 형태 1개, 점점 op 낮춤
4. **거대 도장** — 소재 한자 2자 도장을 170px 크기로 하늘에 떠뜨린다(`rotate(-3°)`, `--seal` 채움, 강 위에 `sealSh` 그라데이션 그림자). 구석 낙관 도장은 2자(예: 夢)로 바꿔 중복을 피한다

제목·낙관 시구도 꿈 언어로 바꾼다(예: 「夢中鐵馬」/「雲上鐵馬 江天一夢」). 전경 바위도 지면에서 살짝 띄우고 그 아래 종이에 대시 그림자. 움직임은 bob/hover로 절제.

## 추상 변주 ("추상화로", "연필 추상" 요청 시)

형상을 걷어내고 화풍의 형식 요소만 남긴다 — 원형 구현: `artifacts/cheolmasimsang.html`(철마심상). 운보 김기창의 한자 추상과 같은 문법:

1. **수묵 색면** — 산·안개의 기억을 soft28 번진 wash blob 3개(좌상단 진하게, 우상단 옅게, 하단 왼쪽 짙게)로. brush 필터로 가장자리 흘림
2. **거대 심상 한자** — 소재 한자 2자를 400px `ghost` 클래스(op .13)로 화면에 깐다. 유일하게 "읽을 수 있는" 앵커
3. **제스처 스트로크** — 주제의 운동 기억을 굵은 대각선 제스처(펜 w 13 + 에코 w 5)로 화면을 횡단시키고, 끝에 짧은 꼬리 + 속도 대시 8개
4. **떠 다니는 원** — 바퀴의 기억: 큰 원 쌍(스포크 JS 생성), 작은 원, 채운 원, 파선 원(stroke-dasharray로 "기억 파편"). `.floaty`로 bob
5. **나선** — 연기의 기억을 자유 곡선 나선 하나로
6. **질감 밴드** — hatch()로 텍스처 블록 3개(산·하안의 기억) + 하단 짧은 대시 장(강의 기억)
7. 붉은 도장 포인트는 제스처 끝 + 전경 소형 1개씩만. 제목은 「○心象」, 낙관 시구는 형이상 언어(예: 「形滅意存 馬去跡在」), 구석 도장 한 글자

여백이 주인공 — 채워지지 않은 공간을 남기고, 요소들은 겹치지 않게 비대칭 배치한다. 움직임은 원 bob과 나선 느린 회전(30s)만.

## 사진 파일(PNG) 내보내기 ("사진으로 저장해줘" 요청 시)

프레임·캡션 없이 그림 본체만 PNG로 캡처한다. headless Chrome 절차 — **이 절차에서 한 번 놓치면 흰 백지가 나온다, 특히 1번**:

1. 내보내기 페이지는 원본을 복사해 `</head>` 앞에 CSS를 주입해 만든다. 이때 PowerShell `Get-Content`에 **반드시 `-Encoding UTF8`** — 생략하면 한글이 CP949로 깨져 `</title>` 닫는 `<`가 바이트로 삼켜져 문서 전체가 title 텍스트로 흡수되고 body가 텅 빈 **흰 백지**가 캡처된다. 쓸 때도 `[System.Text.Encoding]::UTF8`로.
2. 주입 CSS(1:1 캡처용):
   `body{background:var(--paper)!important} main{padding:0!important;min-height:0!important} figure{width:1600px!important;max-width:none!important;gap:0!important} .sheet{border:none!important;box-shadow:none!important;width:1600px!important} #art{width:1600px!important;height:1200px!important} figcaption{display:none!important}`
3. 캡처(`$chrome='C:\Program Files\Google\Chrome\Application\chrome.exe'` 확인):
   `& $chrome --headless=new --disable-gpu --hide-scrollbars --no-first-run --virtual-time-budget=8000 --window-size=1600,1200 --user-data-dir="실행마다 고유한 임시 폴더" --screenshot="C:\Users\KYH\Documents\artifacts\<이름>.png" "file:///C:/.../export.html" 2>$null`
   - `--user-data-dir`은 실행마다 **고유**하게(재사용/동시 실행 시 프로필 잠금으로 실패)
   - **`--force-device-scale-factor` 금지** — 이 환경에서 빈 백지를 유발. 2배 해상도(3200×2400)가 필요하면 주입 CSS의 figure/.sheet/#art를 3200px로, `--window-size=3200,2400`으로 확대
4. 성공 판정: PNG가 수십 KB면 백지(실패 — 1번 인코딩 의심), 1MB 내외면 정상. 크기는 `[System.Drawing.Image]::FromFile`로 확인, 내용은 vision으로 확인한다.
5. 예: 기차도 → `C:\Users\KYH\Documents\artifacts\gichado.png`. 기차도 HTML에는 이 PNG가 base64 data URI로 박혀 페이지 안에서 바로 내려받기 링크가 되어 있다(2MB 내외로 페이지가 무거워지는 것은 감수).
