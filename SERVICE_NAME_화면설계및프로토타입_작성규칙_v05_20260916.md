# {SERVICE_NAME} 검진서비스 화면설계 및 프로토타입 작성규칙

> v5 | 2026-09-16
> **`{SERVICE_NAME}` 플레이스홀더**: 서비스명·프로젝트명·회사명 접두어는 아직 확정 전이므로 본 문서와 산출물 전체에서 `{SERVICE_NAME}` 그대로 쓴다(이전 접두어 `EAP` 폐기). 확정되면 파일명·피그마 레이어명·화면 ID 접두어·검토 담당 표기를 일괄 치환한다. 화면 ID처럼 소문자 컨텍스트에서는 확정값을 소문자로 치환한다(예: `{SERVICE_NAME}-login-01`).
> 피그마용 HTML(화면설계서)과 인터랙티브 프로토타입(HTML) 모두에 공통 적용.
> v4(2026-08-13) 대비 변경: **접두어 `EAP` → `{SERVICE_NAME}` 플레이스홀더로 전면 대체**, **§8-2 좌측 화면 목록 사이드바 규칙 신설**(화면설계서에 사이드바가 항상 생성되도록 구조·데이터·동작 정의), §4 사이드바 규격, §2 `--pages-w` 토큰, §8 레이아웃 구조에 `aside.pages`·`#screen-{화면ID}` 추가, §12 사이드바 표시 여부 열, §14 검증 항목 7~8 추가. 상세는 §16 버전 이력 참조.
> v3(2026-08-12) → v4: §5 번호 뱃지 z-index 100 이상·left -10px 조정.
> 본 문서는 **전체 화면 공통 UX/UI 가이드**만 담는다. 특정 메뉴·화면에 종속된 콘텐츠 정의(히어로 배너 동작, 카드 구성 등)는 각 화면설계서·기능정의서에서 관리한다.

---

## 1. 디바이스 기준

| 항목 | 값 |
|------|-----|
| 기준 기기 | 스마트폰 (모바일 앱) |
| 프레임 사이즈 | 393 × 852px |
| 프레임 border-radius | 40px |
| Dynamic Island | 없음 |
| Status Bar 높이 | 44px |
| Home Indicator | width 134px, height 5px, color #222222, 하단 10px margin |
| 네비게이션 바 높이 | 56px |
| 탭바 높이 | 82px (padding-bottom 24px 포함) |

---

## 2. 컬러 시스템

```css
:root {
  /* Primary */
  --p:   #0066CC;   /* 버튼, 활성 상태, 포커스 테두리 */
  --pd:  #004FA3;   /* hover, 헤더 강조 */
  --pdd: #003380;   /* pressed */
  --pl:  #E8F2FF;   /* 배경 강조, 배너, 힌트박스, 선택 칩 */
  --plm: #D0E6FF;   /* 아바타 배경, 카드 선택 배경 */
  --acc: #00A3E0;   /* 포인트 하늘색, step 화살표 */

  /* Text */
  --t1: #1A1A1A;   /* 본문, 제목 */
  --t2: #444444;   /* 라벨, 서브 텍스트 */
  --t3: #888888;   /* 설명, placeholder, 캡션 */
  --t4: #BBBBBB;   /* disabled, 비활성 */

  /* Semantic */
  --success: #00B86E;
  --sbg:     #E8F7F0;   /* 예약확정 배경 */
  --st:      #006B40;   /* 예약확정 텍스트 */
  --warn:    #FF8C00;
  --wbg:     #FFF3E0;   /* 신청완료 배경 */
  --wt:      #8A4D00;   /* 신청완료 텍스트 */
  --danger:  #CC2200;
  --dbg:     #FFF0EE;   /* 에러·필수 배경 */

  /* Background & Border */
  --bg:     #E8EDF5;   /* 앱 전체 배경 */
  --white:  #FFFFFF;
  --border: #DDE3EE;   /* 인풋 테두리, 구분선 */

  /* Partner */
  --welit:  #00B86E;   /* 웰잇고 브랜드 (스위칭 탭·프레임) */

  /* Document shell (v5 신설) */
  --pages-w: 264px;    /* 좌측 화면 목록 사이드바 폭 (§8-2) */
}
```

---

## 3. 타이포그래피

- 폰트: `Noto Sans KR`
- CDN: `https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;600;700`

| 용도 | Size | Weight |
|------|------|--------|
| 페이지 대제목 | 24px | 700 |
| 섹션 타이틀 | 22px | 700 |
| 화면 타이틀 | 20px | 700 |
| 네비게이션 타이틀 | 17px | 600 |
| 블록 타이틀 | 15px | 600 |
| 본문 / 인풋 텍스트 | 15px | 400 |
| 라벨 | 13px | 500 |
| 서브 텍스트 | 13px | 400 |
| 힌트·캡션 | 12px | 400 |
| 뱃지·칩 텍스트 | 11px | 600 |
| 탭바 텍스트 | 10px | 400(비활성) / 600(활성) |

---

## 4. 컴포넌트 규격
### Component ID (컴포넌트 ID)
1. 컴포넌트 ID 구조 : 페이지명-영역명-컴포넌트명+번호 로 되어있다.
2. 컴포넌트 ID 적용 : **컴포넌트 ID를 HTML의 Class 명에 추가한다.**
3. 화면수정 요청 : **화면수정 요청시 컴포넌트 ID를 참고하여 해당 컴포넌트만 업데이트 한다.**
4. 컴포넌트 ID가 없는 경우 : 화면ID(Screen ID)에서 표시내용 컬럼의 번호를 기준으로 해당 컴포넌트(또는 요소)만 업데이트 한다.
5. 기능정의서와의 연결 : 화면설계서 description에 표기하는 컴포넌트 ID는 기능정의서 Component ID(`{ScreenID}-{short}{no}`)와 동일한 값을 쓴다. (§10 참조)

### 버튼
| 종류 | Height | Radius | 색상 |
|------|--------|--------|------|
| Primary | 52px | 14px | bg #0066CC, text #FFFFFF, 16px/600 |
| Secondary | 52px | 14px | bg #FFFFFF, border 1.5px #0066CC, text #0066CC, 15px/600 |
| Ghost/Dashed | 48px | 14px | border 1.5px dashed #0066CC, text #0066CC, 14px/500 |
| 뒤로가기 | - | - | ‹ 유니코드 32px/300 — 아이콘 사용 금지 |

- 필수 입력 항목 모두 입력 시 Primary 버튼 활성화

### 입력 필드
| 항목 | 값 |
|------|-----|
| Height / Radius | 50px / 10px |
| 기본 테두리 | 1px solid #DDE3EE |
| 포커스 테두리 | 1.5px solid #0066CC |
| 에러 테두리 | 1.5px solid #CC2200 |
| 패딩 | 0 16px |
| Placeholder | 13px / #BBBBBB / 미설정 시 "{레이블} 입력" 자동 적용 |
| 에러 처리 | 하단 서포팅 텍스트 12px / #CC2200 + 해당 필드 자동 포커싱 |
| 최대 글자수 | 기본 100자 |
| 상태 | 활성(Enabled) / 호버(Hovered) / 포커스(Focused) / 에러(Error) / 비활성(Disabled) |

### 입력 필드 유형별 규칙
| 유형 | 규칙 |
|------|------|
| 비밀번호 | 8~20자리 / 영문+숫자+특수문자 3종 조합 필수 / 마스킹 + 눈 아이콘 토글 |
| 전화번호 | 010-XXXX-XXXX 자동 포맷 / 숫자만 입력 / 자릿수 부족 시 에러 |
| 생년월일 | YYYYMMDD 형식 / 숫자 키패드 / 존재하지 않는 날짜 오류 처리 |
| 주소 | 우편번호 검색 + 기본주소 + 상세주소 3단 구성 / 주소 검색 웹뷰 호출 |

### 카드
| 항목 | 값 |
|------|-----|
| Radius / 테두리 | 14px / 1px solid #DDE3EE |
| 배경 / 패딩 | #FFFFFF / 14px 16px |
| hover 테두리 | 1px solid #0066CC |


### 뱃지 / 칩
| 종류 | Radius | Padding |
|------|--------|---------|
| 상태 뱃지 | 20px | 4px 10px |
| 선택 칩 / 필터 칩 | 20px | 8px 16px |

### 상태 뱃지 색상
| 상태 | 배경 | 텍스트 |
|------|------|--------|
| 예약확정 | #E8F7F0 | #006B40 |
| 신청완료 | #FFF3E0 | #8A4D00 |
| 미신청 | #F2F2F2 | #888888 |
| 취소 | #FFF0EE | #CC2200 |

### 모달 / 팝업
| 항목 | 값 |
|------|-----|
| Radius / 패딩 | 20px / 24px |
| 오버레이 | rgba(0,0,0,0.55) |
| 버튼 | height 46px / radius 12px |

### 힌트박스
| 항목 | 값 |
|------|-----|
| Radius / 배경 | 10px / #E8F2FF |
| 텍스트 | 12px / #004FA3 / 패딩 12px 14px |

### 네비게이션 바
- 높이 56px, position: relative
- 뒤로가기: ‹ (유니코드 32px/300) — 아이콘 사용 금지
- 타이틀: 17px/600/#1A1A1A, position absolute 중앙 고정 (left: 50%, translateX(-50%))
- [내비] 없는 화면: 로그인(진입점) / 메인 홈(탭바) / 예약 완료(완료 화면)

### 탭바
- 높이 82px, padding-bottom 24px
- 상단 구분선 1px solid #DDE3EE
- 아이콘 22px / 활성 #0066CC / 비활성 #BBBBBB
- 라벨 11px, `white-space: nowrap`
- 스위칭 탭(웰잇고)은 우측 끝 고정, 아이콘·라벨 컬러 --welit 계열

### 스텝 인디케이터 (도트형)
- 비활성 도트: 7px 원, #DDE3EE
- 활성 도트: 18px × 7px pill, #0066CC

### 토스트 메시지
- Background: #1A1A1A, Color: #fff, Font: 13px
- Border-radius: 20px, 하단 탭바 위 16px 여백
- 자동 사라짐: 3초

### 좌측 화면 목록 사이드바 (v5 신설)
> 화면설계서 문서 자체의 UI(피그마 프레임 밖)이며, 생성 규칙·데이터·동작은 §8-2에서 정의한다. 여기서는 규격만 둔다.

| 요소 | 규격 |
|------|------|
| `aside.pages` | position fixed / left 0 / top 0 / bottom 0 / width `var(--pages-w)`(264px) / 배경 #FFFFFF / 우측 테두리 1px solid #C9D4E4 / z-index 500 / flex column / transition transform .2s |
| `.pages.collapsed` | `transform: translateX(calc(-1 * var(--pages-w) + 34px))` — 접힘 시 34px만 남김 |
| `.pages-head` | flex · 좌 "화면 목록" 12.5px/700 --t1 · 우 접기 버튼 / 패딩 12px 10px 10px 14px / 하단 테두리 1px solid #E3E8F0 |
| `.pages-toggle` | 24×24px / 테두리 1px solid #D5DDE8 / radius 6px / 배경 #FFFFFF / 글자 ‹ 14px --t2 / 접힘 시 `rotate(180deg)` — 아이콘 폰트 사용 금지 |
| `.pages-body` | flex 1 / overflow-y auto / 패딩 6px 0 20px |
| `.pg-grp-t` (그룹 제목) | position sticky top 0 / 배경 #F5F8FC / 11px/700 --pd / 패딩 8px 14px 5px / 상단 테두리 1px solid #EEF1F6 / flex space-between — 우측 `<span>`에 항목 개수 500 --t4 |
| `.pg-item` (화면 항목) | flex column / 패딩 6px 14px 6px 22px / cursor pointer / 좌측 테두리 3px transparent / line-height 1.3 |
| `.pg-item b` | 화면 ID 11.5px/700 --t1 |
| `.pg-item span` | 화면명 10.5px --t3 |
| `.pg-item:hover` | 배경 #F5F8FC |
| `.pg-item.active` | 배경 --pl / 좌측 테두리 --p / `b` 컬러 --p |
| 본문 여백 | `.doc-head`, `.figma-canvas`에 `margin-left: var(--pages-w)`; `body.pages-collapsed`일 때 34px |
| 이동 하이라이트 `.mm-target` | 이동한 프레임의 `.figma-label` 컬러 --p, 2초 후 해제 |

---

## 5. 번호 뱃지 규칙

| 항목 | 값 |
|------|-----|
| 모양 / 크기 | 원형 22×22px |
| 기본 색상 | #CC2200 |
| 미확정 항목 | #FF8C00 |
| 서브 항목 (X-2 등) | #CC2200 opacity 0.75 |
| 테두리 | 2px solid #FFFFFF |
| 쌓임 순서 | **항상 최상단** — 화면 요소 중 z-index 최상위로, 어떤 요소(텍스트)에도 가려지지 않아야 함 (권장 z-index: 100 이상) |
| 기본 위치 | 컴포넌트 좌측 상단 top -10px, left -10px (모서리 바깥 걸침) |
| 폰트 | 10px / 700 / #FFFFFF |
| description 번호 | 동일한 스타일로 통일 |

### 5-1. 텍스트 가림 방지 (v3 신설)
번호 뱃지는 어떤 경우에도 **화면 요소의 텍스트·아이콘을 가리지 않아야 한다.** 가림이 발생하면 뱃지를 **좌측으로 이동**시키는 것을 원칙으로 한다.

| 상황 | 처리 |
|------|------|
| 카드·배너 등 배경 모서리에 걸치는 경우 | 기본 위치 유지 (배경만 덮으므로 허용) |
| 첫 줄이 여백 없는 제목 텍스트인 래퍼 | 뱃지를 제목 **좌측 바깥**으로 이동: `top:-1px; left:-28px` (`.bdg.out`). 컬럼 거터·패딩 안에 위치해 클리핑되지 않아야 함 |
| 탭바의 탭 (아이콘·라벨 존재) | 탭 상단 중앙 배치 금지. **탭 좌측 경계 + 세로 중앙**으로 이동: `top:50%; left:-10px; translateY(-50%)` (`.bdg.tab-end`). 이웃 탭 라벨과 겹치지 않는지 확인 |
| 우측이 비어 있는 가로 바(헤더·띠배너) | 우측 끝 세로 중앙 배치 허용: `top:50%; right:8px; translateY(-50%)` (`.bdg.bar`) |

- 산출 전 §14 검증에서 뱃지 가림 여부를 육안 확인 항목으로 포함한다.
- 사이드바(`aside.pages`)의 z-index(500)는 문서 UI용이며, 프레임 안 번호 뱃지와는 쌓임 맥락이 다르므로 서로 간섭하지 않는다.

---

## 6. 아이콘 & 외부 리소스

| 항목 | 값 |
|------|-----|
| 아이콘 라이브러리 | Tabler Icons |
| CDN 버전 | **@2.44.0 고정** (`@latest` 사용 금지) |
| CDN URL | `https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/dist/tabler-icons.min.css` |
| 엑박 발생 시 | 유니코드 문자로 대체 (← `‹`, → `›`, ↺) |

```css
i[class^="ti"] {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  line-height: 1;
}
```

---

## 7. scroll-body 기본 설정

```css
.scroll-body {
  flex: 1;
  overflow-y: auto;
  overflow-x: visible;
  padding: 16px 24px 80px 32px;
  /* 상단 16px: 첫 번째 번호 뱃지 클리핑 방지 */
  /* 하단 80px: 탭바·플로팅 버튼 가림 방지 */
  /* 좌측 32px: 번호 뱃지 클리핑 방지 */
}
```

---

## 8. 피그마용 HTML 생성 규칙

> 산출물: 모든 화면을 가로로 나열한 HTML 단일 파일 (HTML to Figma 플러그인 임포트용). **좌측에 화면 목록 사이드바를 항상 포함한다(§8-2).**

### 레이아웃 구조 (v5 변경)
```
body
 ├── aside.pages#pages                            ← v5 신설: 좌측 화면 목록 사이드바 (§8-2)
 │     ├── .pages-head  ("화면 목록" + button.pages-toggle#pagesToggle ‹)
 │     └── .pages-body
 │           └── .pg-grp × N                      (그룹 = 캔버스 그룹과 1:1)
 │                 ├── .pg-grp-t   (그룹명 <span>항목 수</span>)
 │                 └── .pg-item[data-key="{화면ID}"] × N  (<b>{화면ID}</b><span>{화면명}</span>)
 ├── .doc-head                                    (문서 제목 · 버전 · 날짜 · 변경 이력 — 있을 경우)
 └── .figma-canvas [data-figma-layer-name="{SERVICE_NAME}/화면설계"]
       └── .vpair × N                             ← v5 신설: 그룹 박스 (사이드바 .pg-grp와 1:1)
             ├── .vpair-label  "{그룹명}"
             └── .figma-screen-wrap × N [data-figma-layer-name="screen/{화면ID}"]
                   ├── .figma-screen-col
                   │     ├── .figma-label       (화면 ID)
                   │     ├── .figma-sublabel    (화면명)
                   │     └── .phone-frame [data-figma-layer-name="frame/{화면명}"]
                   │           ├── .status-bar
                   │           └── .phone-screen#screen-{화면ID}     ← v5 신설: 사이드바 이동 타깃
                   │                 ├── .nav-bar     (해당 화면에 [내비] 있는 경우)
                   │                 ├── .scroll-body
                   │                 └── .home-indicator
                   └── .annotation-panel        (우측 description)
script: 사이드바 동작 스크립트 (§8-2 「동작」) — 파일 하단 <script> 1개
```

### data-figma-layer-name 규칙
| 레벨 | 형식 | 예시 |
|------|------|------|
| 최상위 | `{SERVICE_NAME}/화면설계` | |
| 화면 | `screen/{화면ID}` | `screen/{SERVICE_NAME}-login-01` |
| 프레임 | `frame/{화면명}` | `frame/로그인` |
| 컴포넌트 | `{번호}/{컴포넌트명}` | `1/field-name` |
| 조건부 | `{번호}/{컴포넌트명} [조건부: 조건내용]` | |
| 미확정 | `{번호}/{컴포넌트명} [미확정]` | |

- 사이드바는 `aside.pages` 전체에 `data-figma-layer-name`을 **붙이지 않는다** (피그마 임포트 대상 아님). 임포트 시 캔버스만 선택되도록 사이드바는 `.figma-canvas` 밖에 둔다.

### 레이아웃 원칙
- `display: flex` 우선 (`position: absolute` 지양). 사이드바는 `position: fixed` 허용(문서 UI, §13).
- 화면 간 간격: `gap: 60px`
- 캔버스 배경: #E8EDF5
- `.doc-head`·`.figma-canvas`에 `margin-left: var(--pages-w)`를 두어 사이드바에 가려지지 않게 한다. `body.pages-collapsed`일 때 34px로 전환.

### 8-1. 간략(요약) 프레임 · 플로우 다이어그램 규칙 (v3 신설)
스위칭·연동 등 **두 서비스 간 흐름**을 표현할 때는 실물 프레임 대신 간략 프레임 + 화살표 다이어그램을 사용할 수 있다.

**간략 미니 프레임 (`.sw-mini`)**
| 항목 | 값 |
|------|-----|
| 폭 / 라운드 | 270px / 24px |
| 헤더 | 높이 56px, 18px/700 흰색 텍스트, 서비스 브랜드 컬러 배경 (웰잇고 --welit / New biz --p) |
| 바디 | 콘텐츠 자리 표시 블록 3개 (높이 56 / 86 / 86px, 라운드 12px), 서비스 틴트 배경 (#E8F7F0 / #E8F2FF) |
| 탭바 | 높이 50px, 라벨 12px 텍스트만. 활성(스위칭) 탭은 브랜드 컬러 배경 + 흰색 라벨, 우측 끝 고정 |
| 번호 뱃지 | 탭 좌측 경계·세로 중앙 (§5-1 탭 규칙 적용) |

**흐름 화살표**
- 선(2px)과 삼각형 촉을 한 몸으로 결합해 그린다. 아이콘 글리프를 선 끝에 얹는 방식 금지 (선·촉 어긋남 발생).
- 촉: border 삼각형 — `border-top/bottom: 6px transparent` + 진행 방향 쪽 `border: 10px solid {컬러}`.
- 방향별 컬러: 진입(→) --p / 복귀(←) --welit.
- 각 화살표 아래 캡션: 첫 줄 `굵게 {출발} → {도착}`, 둘째 줄 동작 요약 (11.5px, --t2).

**전체 프레임 축소 배치 (기존 `.sw-nb` 방식)**
- 실물 프레임을 흐름도 안에 넣어야 할 경우 max-height로 바디를 잘라(내용 잘림 허용) 배치할 수 있으나, 간략 프레임 방식을 우선한다.

### 8-2. 좌측 화면 목록 사이드바 생성 규칙 (v5 신설)

> 화면설계서 HTML을 **신규 생성·업데이트할 때 항상** 사이드바를 함께 생성한다. 사이드바가 없는 화면설계서는 산출하지 않는다(§14 항목 7).

**데이터 출처 — 기능정의서에서 가져온다**
| 사이드바 요소 | 출처 |
|------|------|
| 그룹(`.pg-grp`) 및 순서 | 기능정의서 1Depth › 2Depth 순서. 2Depth(화면 그룹) 1개 = 그룹 1개. 2Depth가 없으면 1Depth 단위 |
| 그룹명(`.pg-grp-t`) | 2Depth 명(없으면 1Depth 명). 필요 시 `{화면ID} · … — {그룹명}` 형식 허용 |
| 항목(`.pg-item`) 및 순서 | 그룹 안 화면을 기능정의서 no 순으로. **캔버스에 프레임이 있는 화면만** 항목으로 올린다 |
| `data-key` / `<b>` | 화면 ID (I열 Screen ID). `data-figma-layer-name="screen/{화면ID}"`·`.figma-label`·`#screen-{화면ID}`와 **같은 값** |
| `<span>` | 화면명(H열 Screen Name). 상태 변형 프레임은 `{화면명} — {상태}` (예: `병원 목록 — 필터 결과 없음 빈 상태`) |
| 항목 수(`.pg-grp-t span`) | 해당 그룹 `.pg-item` 실제 개수 |

- 상태 변형 프레임(빈 상태·바텀시트 열림·팝업 등)도 프레임이 따로 있으면 항목으로 올린다. 이때 `data-key`는 `{화면ID}` 뒤에 상태 접미사를 붙인 키(예: `{SERVICE_NAME}-mcr-03-empty`)를 쓰고, 그 프레임의 `id="screen-{키}"`와 맞춘다.
- §12에서 독립 화면이 아닌 종류(블록·팝업·바텀시트·버튼·필터)는 상위 화면 프레임에 포함되므로 별도 항목을 만들지 않는다. 단 별도 프레임으로 그린 경우에는 위 변형 프레임 규칙을 따른다.

**마크업**
```html
<aside class="pages" id="pages">
  <div class="pages-head"><span>화면 목록</span><button class="pages-toggle" id="pagesToggle" title="접기/펼치기">‹</button></div>
  <div class="pages-body">
    <div class="pg-grp"><div class="pg-grp-t">로그인<span>3</span></div>
      <div class="pg-item" data-key="{SERVICE_NAME}-login-01"><b>{SERVICE_NAME}-login-01</b><span>로그인</span></div>
      <div class="pg-item" data-key="{SERVICE_NAME}-login-02"><b>{SERVICE_NAME}-login-02</b><span>로그인 — 계정 선택 팝업</span></div>
      <div class="pg-item" data-key="{SERVICE_NAME}-login-03"><b>{SERVICE_NAME}-login-03</b><span>비밀번호 재설정</span></div>
    </div>
    <!-- .pg-grp 반복 -->
  </div>
</aside>
```
- `aside.pages`는 `<body>`의 **첫 번째 자식**으로 둔다.
- 캔버스 쪽 대응 요소: 그룹마다 `.vpair`(+ `.vpair-label`) 1개, 화면마다 `.phone-screen`에 `id="screen-{화면ID}"` 1개. 이 id가 없으면 클릭 이동·현재 위치 표시가 동작하지 않는다.

**동작 (파일 하단 `<script>` 1개로 구현)**
| 동작 | 규칙 |
|------|------|
| 항목 클릭 → 이동 | `#screen-{key}`가 속한 `.figma-screen-col`의 좌상단으로 `window.scrollTo({behavior:'smooth'})`. 가로 보정: 사이드바 폭(접힘 시 34px) + 32px, 세로 보정: 24px |
| 이동 하이라이트 | 대상 `.figma-screen-wrap`에 `.mm-target` 부여, 2초 후 제거 |
| 현재 위치 표시 | `IntersectionObserver(threshold 0.2)`로 뷰포트에 들어온 `.phone-screen[id^="screen-"]`의 키를 찾아 해당 `.pg-item`에 `.active`, 나머지는 해제. 활성 항목이 목록 밖이면 `scrollIntoView({block:'nearest'})` |
| 접기/펼치기 | `#pagesToggle` 클릭 → `aside.pages.collapsed` + `body.pages-collapsed` 토글 |
| 문서 헤더 연동(선택) | `.doc-head` 변경 이력 표의 화면 ID 문자열을 클릭 가능한 링크로 바꿔 같은 이동 함수를 호출 |

```javascript
(function () {
  const go = key => {
    const el = document.getElementById('screen-' + key); if (!el) return;
    const wrap = el.closest('.figma-screen-wrap') || el;
    const col = el.closest('.figma-screen-col') || wrap;
    const pagesW = document.body.classList.contains('pages-collapsed') ? 34 : (document.getElementById('pages')?.offsetWidth || 0);
    const r = col.getBoundingClientRect();
    window.scrollTo({ left: window.scrollX + r.left - pagesW - 32, top: window.scrollY + r.top - 24, behavior: 'smooth' });
    wrap.classList.add('mm-target'); setTimeout(() => wrap.classList.remove('mm-target'), 2000);
  };
  const items = [...document.querySelectorAll('.pg-item')];
  items.forEach(it => it.addEventListener('click', () => go(it.dataset.key)));
  const setActive = key => {
    items.forEach(it => it.classList.toggle('active', it.dataset.key === key));
    const a = items.find(it => it.dataset.key === key); if (a) a.scrollIntoView({ block: 'nearest' });
  };
  if ('IntersectionObserver' in window) {
    const io = new IntersectionObserver(es => { es.forEach(e => { if (e.isIntersecting) setActive(e.target.id.replace('screen-', '')); }); }, { root: null, threshold: 0.2 });
    document.querySelectorAll('.phone-screen[id^="screen-"]').forEach(el => io.observe(el));
  }
  const pg = document.getElementById('pages'), tg = document.getElementById('pagesToggle');
  if (pg && tg) tg.addEventListener('click', () => { pg.classList.toggle('collapsed'); document.body.classList.toggle('pages-collapsed'); });
})();
```

**업데이트 시**
- 화면 추가·삭제·이름 변경이 있으면 캔버스와 사이드바를 **같은 작업에서 함께** 갱신한다. 항목 순서·개수·`data-key`가 캔버스와 어긋나면 §14 검증에서 실패로 본다.
- 기존 화면설계서에 사이드바가 없으면(v4 이전 산출물) 업데이트 시 위 규칙대로 **사이드바를 추가**하고, 각 프레임에 `id="screen-{화면ID}"`를 부여한다. 프레임 마크업 자체는 변경하지 않는다.

**프로토타입과의 관계**
- §9 프로토타입의 좌측 `.sidebar`(플로우별 화면 목록)와 같은 그룹·같은 항목·같은 순서를 쓴다. 프로토타입은 항목 클릭 시 `goTo(key)`로 화면을 전환하고, 화면설계서는 캔버스 스크롤로 이동한다는 점만 다르다.

---

## 9. 프로토타입 생성 규칙

> 산출물: 좌측 사이드바 + 중앙 폰 프레임 + 우측 어노테이션 패널 구조의 HTML 단일 파일

### 레이아웃 구조
```
.layout
  ├── .sidebar              (좌측 — 플로우별 화면 목록. 그룹·항목·순서는 §8-2 화면설계서 사이드바와 동일)
  ├── .main-area
  │     ├── .topbar         (뒤로가기 / 처음으로 버튼)
  │     └── .canvas
  │           ├── .phone-frame (393×852px)
  │           └── .annotation-panel (우측 description)
```

### 화면 전환 패턴
```javascript
let history = [], current = 'login01';

function goTo(key) {
  document.querySelectorAll('.phone-screen').forEach(s => s.classList.remove('active'));
  document.getElementById('screen-' + key).classList.add('active');
  renderAnno(key);           // 우측 어노테이션 자동 업데이트
  renderScreenBadges(key);   // 화면 번호 뱃지 렌더링
  if (current !== key) history.push(current);
  current = key;
}
```

### 어노테이션 패널 구성
- 번호 뱃지 + 컴포넌트 태그 + 설명
- 미확정 칩 (#FF8C00, 해당 항목만)
- 규칙 박스 (입력 필드 정책 등)
- 인터랙션 인라인 (트리거 → 조건 → 결과)
- 검토 사항 카드 (정책확인·개발확인·미확정 유형)

### 인터랙션 구현 기준
| 기능정의서 내용 | 구현 방법 |
|----------------|----------|
| 버튼 탭 → 화면 이동 | `goTo('화면키')` 호출 |
| 조건 분기 | 분기 조건별 버튼·탭 각각 구현 |
| 모달 / 바텀시트 | `display: none/flex` 토글 |
| 탭 전환 | 탭 클릭 시 콘텐츠 영역 교체 |
| 토글 ON/OFF | 클래스 토글 |
| 플로팅 바 | `position: sticky; bottom: 0` |
| 토스트 메시지 | 3초 후 자동 소멸 |

---

## 10. Description 패널 작성 규칙

### 번호 체계
| 항목 | 스타일 |
|------|--------|
| 메인 컴포넌트 (1, 2, 3…) | 빨간 원형 뱃지 #CC2200 |
| 서브 항목 (2-1, 2-2…) | 동일 색상, opacity 0.75 |
| 미확정 항목 | 주황 뱃지 #FF8C00 |

### 컴포넌트 ID 칩 (v3 신설)
- description 항목 제목 우측에 기능정의서 Component ID를 칩으로 표기한다: 예 `switching-flow-sw1`
- 스타일 (`.cid`): 배경 --pl / 텍스트 --pd / 10px 600 / 라운드 6px / monospace / 패딩 2px 7px
- ID 값은 기능정의서와 **완전 일치**시켜, 화면설계서 ↔ 기능정의서 상호 추적 키로 쓴다.

### 태그 종류
| 태그 | 의미 |
|------|------|
| [조건부] | 특정 조건에서만 노출 |
| [미확정] | 정책·개발 미확정 항목 |

- `[미확정]` 칩 스타일: 배경 #FFF3E0 / 텍스트 #B85C00 / 10px 700 / 라운드 6px

### 검토 사항 유형
| 유형 | 담당 |
|------|------|
| 정책확인 | {SERVICE_NAME}팀·법무팀 |
| 개발확인 | 개발팀 |
| 미확정 | 미정 |

### 인터랙션 기술 방식
```
[트리거] | [조건(선택)] | [결과]
예: "로그인 버튼 탭" | "이름·비밀번호 불일치" | "인라인 에러 표시"
```
- 인라인 박스 스타일 (`.ix`): 테두리 --border / 라운드 9px / 11px / 텍스트 --pd / 패딩 8px 11px. 트리거·결과는 굵게.

---

## 11. 화면 ID 체계

| 분류 | ID 패턴 |
|------|---------|
| 로그인 | {SERVICE_NAME}-login-XX |
| 메인 | {SERVICE_NAME}-main-XX |
| 검진예약 | {SERVICE_NAME}-mcr-XX |
| 검진결과 | {SERVICE_NAME}-result-XX |
| 건강자료실 | {SERVICE_NAME}-health-XX |
| 플로우(흐름도) | {흐름명}-flow (예: switching-flow) |

- `{SERVICE_NAME}` 접두어는 서비스명 확정 시 소문자로 일괄 치환한다.
- 화면 ID는 사이드바 `data-key`·`<b>`, `data-figma-layer-name="screen/…"`, `.figma-label`, `#screen-…` 네 곳에 **같은 값**으로 쓴다(§8-2). 기능정의서가 별도 Screen ID 체계(예: Newbiz 기능정의서 v05의 PascalCase)를 쓰는 경우에는 그 값을 그대로 화면 ID로 사용한다.

---

## 12. 화면 종류별 규칙

| 종류 | [내비] | 독립 화면 | 사이드바 항목 (v5) |
|------|--------|----------|----------|
| 페이지 | 필수 (진입점·홈 제외) | O | O |
| 블록 | 없음 | X (상위 페이지에 포함) | X |
| 팝업 | 없음 | X | X (별도 프레임으로 그리면 변형 항목 O) |
| 바텀시트 | 없음 | X | X (별도 프레임으로 그리면 변형 항목 O) |
| 버튼 | 없음 | X (상위 화면 명세에 통합) | X |
| 필터 | 없음 | X | X (별도 프레임으로 그리면 변형 항목 O) |
| 플로우(흐름도) | 없음 | O (간략 프레임 사용, §8-1) | O |

---

## 13. 금지 사항

- `@latest` CDN 사용 금지 → 버전 고정 (@2.44.0)
- `position: absolute` 남용 금지 → `display: flex` 우선. 허용: 번호 뱃지(.bdg) / FAB(.fab). `position: fixed`는 사이드바(`aside.pages`)에만 허용
- Dynamic Island 추가 금지
- 외부 이미지 URL 직접 삽입 금지 → placeholder 처리
- 브라우저 저장소(localStorage 등) 사용 금지 → 메모리 상태만 사용 (사이드바 접힘 상태도 저장하지 않는다)
- 이모지 사용 금지 → 아이콘/텍스트로 대체 (`★` 등 특수문자 포함, `※`·`❙` 등 일반 기호는 허용)
- 화살표를 아이콘 글리프로 선 끝에 얹는 방식 금지 → §8-1 선+촉 결합 방식 사용
- 사이드바 없이 화면설계서 산출 금지 (v5) → §8-2

---

## 14. 산출물 검증 절차 (v3 신설 · v5 항목 추가)

모든 편집 후 bash `python3`로 아래 항목을 검증하고, 통과 후 산출한다.

| # | 항목 | 기준 |
|---|------|------|
| 1 | div 균형 | `<div>` 개수 == `</div>` 개수 (span 등 추가 태그도 동일) |
| 2 | 넘버링 매핑 | 화면별 번호 뱃지(.bdg) 집합 == description 번호(.an) 집합. 상태 뱃지(미신청·신청완료·예약확정)는 제외 |
| 3 | 이모지 없음 | 유니코드 이모지 블록 검출 0건 |
| 4 | 금지사항 | `@latest` 0건 / `position:absolute` 사용처가 .bdg·.fab 뿐 / `position:fixed` 사용처가 .pages 뿐 / 외부 이미지 src 0건 / localStorage·sessionStorage 0건 |
| 5 | 뱃지 가림 | §5-1 기준 육안 확인 (탭·제목 좌측 배치 적용 여부) |
| 6 | 기준값 | 393px 프레임 / border-radius 40px / Status Bar 44px 유지 |
| 7 | 사이드바 존재 (v5) | `<body>` 첫 자식이 `aside.pages#pages` / `#pagesToggle` 존재 / 하단 `<script>`에 `.pg-item` 클릭 처리 존재 |
| 8 | 사이드바 정합 (v5) | `.pg-grp` 수 == `.vpair` 수 / 각 그룹 `.pg-item` 수 == `.pg-grp-t span` 숫자 / `.pg-item[data-key]` 집합 == `#screen-{key}` 집합 == `screen/{화면ID}` 집합 / 그룹·항목 순서가 캔버스 순서와 동일 |

- 파일명: `{SERVICE_NAME}_{산출물명}_v{버전}_{YYYYMMDD}` — 날짜는 항상 오늘로 갱신, 버전 업은 사전 확인 후 진행. 서비스명 확정 전에는 `{SERVICE_NAME}` 문자열을 그대로 둔다.

---

## 15. Claude에게 전달할 작업 지시 템플릿

```
[작업 유형] 신규 생성 / 업데이트
[기준 파일] (현재 작업 중인 파일명)
[변경 기능정의서] (구글 시트 URL 또는 내용 붙여넣기)
[변경 범위] 예: {SERVICE_NAME}-main-01 히어로 배너 수정, {SERVICE_NAME}-main-07 신규 추가
[버전] 유지 / 올리기
[날짜] 오늘 날짜 자동 적용
[참고 디자인] (참고 사이트 URL, 있을 경우)
[사이드바] 기본 생성 (§8-2). 기준 파일에 사이드바가 없으면 추가
```

---

## 16. 버전 이력

| 버전 | 날짜 | 변경 내용 |
|------|------|-----------|
| v1 | 2026-06-10 | 최초 작성 |
| v2 | 2026-06-12 | 히어로 배너·스텝 인디케이터·토스트·계정 선택 팝업 케이스 추가 / Description 패널 규칙·검토사항 유형·인터랙션 기술 방식 추가 / 화면 ID 체계·화면 종류별 규칙 추가 / 프로토타입 인터랙션 구현 기준 추가 / 작업 지시 템플릿 추가 / 금지사항 추가 |
| v3 | 2026-08-12 | §5 번호 뱃지 **최상단(z-index 최상위)** 규칙 명문화 + §5-1 텍스트 가림 방지 규칙 신설(탭 뱃지 좌측 경계·세로 중앙, 제목 래퍼 뱃지 좌측 바깥 `.bdg.out`, 가로 바 우측 배치 `.bdg.bar`) / §8-1 간략 미니 프레임(270px)·흐름 화살표(선+삼각형 촉 결합, 아이콘 글리프 금지) 규칙 신설 / §10 컴포넌트 ID 칩(`.cid`)·[미확정] 칩·인터랙션 인라인 박스(`.ix`) 스타일 명문화 / §2 --welit(웰잇고 #00B86E) 컬러 변수 추가 / §11·§12 플로우(흐름도) 화면 종류 추가 / §13 화살표 글리프 금지 추가 / §14 산출물 검증 절차 신설 / 탭바 라벨 11px·nowrap 명시 / **공통 가이드 범위 정리**: 화면 종속 내용 제거 — 히어로 배너(예약 홈 전용), 검진 안내 카드 전화 버튼 메모, 모달 계정 선택 케이스(로그인 전용) → 각 화면설계서·기능정의서에서 관리 |
| v4 | 2026-08-13 | §5 번호 뱃지 z-index 권장값 100 이상, 기본 위치 left -10px로 조정 |
| v5 | 2026-09-16 | **접두어 `EAP` 폐기 → `{SERVICE_NAME}` 플레이스홀더로 전면 대체**(문서 제목·파일명·`data-figma-layer-name` 최상위·화면 ID 접두어·검토 담당·§14 파일명 규칙) / **§8-2 좌측 화면 목록 사이드바 생성 규칙 신설** — 데이터 출처(기능정의서 1Depth›2Depth·no 순, 화면 ID·화면명), 마크업(`aside.pages` › `.pg-grp` › `.pg-item[data-key]`), 동작(클릭 스크롤 이동·`.mm-target` 하이라이트·IntersectionObserver 현재 위치·접기 토글) 및 스크립트 / §8 레이아웃 구조에 `aside.pages`·`.vpair`·`#screen-{화면ID}` 추가, 레이아웃 원칙에 `margin-left: var(--pages-w)` / §4 사이드바 규격 표 신설 / §2 `--pages-w: 264px` 토큰 / §9 프로토타입 `.sidebar`와 그룹·항목 동일 원칙 / §11 화면 ID 네 곳 동일값 규칙 / §12 사이드바 항목 표시 여부 열 / §13 `position: fixed` 사이드바 한정 허용·사이드바 없는 산출 금지 / §14 검증 항목 7(사이드바 존재)·8(사이드바 정합) 추가 / §15 템플릿 [사이드바] 항목 |
