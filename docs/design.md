# 프로젝트 ERP 디자인 규칙

> 대상: `D:\project\erp-web`의 업무 화면과 UI 컴포넌트 매뉴얼.
> 작성일: 2026-10-13. CSS·JSP 소스 조사 기준이며 실제 브라우저 렌더링은 검증하지 않았다.
> `[기존]`은 코드에서 확인한 값, `[제안]`은 AI 생성 결과를 일관되게 만들기 위한 통일 규칙이다. 제안은 기존 브랜드의 확정 규정으로 취급하지 않는다.
> `[필수·WORKLOG]`는 사용자가 실제 작업에서 강조했거나 문제를 복구하며 기록한 보존·적용 규칙이다. 출처는 프로젝트 `WORKLOG.md`의 프로젝트 기준과 2026-09-01~09-15 작업 기록이다. 기록에 담긴 과거 검증 결과를 현재 검증 완료로 간주하지 않는다.
> AI 적용 순서: `[필수·WORKLOG]` → 현재 프로젝트 `UiStandardRenewal.jsp`의 해당 예시 및 실제 업무 화면 → 공통 CSS → `[제안]`. 기존 화면 전체를 제안값으로 일괄 변경하지 않는다. 2026-10-13에 이 대화에서 추가한 작업 요약은 이번 필수 규칙 추출의 근거에서 제외했다.

## 1. 개요

프로젝트 ERP는 흰 바탕과 짙은 글씨, 얇은 회색 구분선으로 정보를 명확하게 구획하는 업무 중심 디자인이다. 작은 버튼과 입력창, 밀도 있는 검색·테이블 구조로 반복 작업과 데이터 확인을 지원한다. 네이비는 제목·탐색의 강조에, 주황은 조회 등 주요 액션에 사용하고 저장은 회색 버튼으로 구분한다. UI 컴포넌트 매뉴얼은 같은 업무 컴포넌트를 보여주되 넓은 여백과 둥근 패널을 사용한다. 업무 화면과 매뉴얼 외곽의 스타일을 혼합하지 않는다. 이 성격 설명은 확인한 구현을 해석한 것이다.

### 범위 및 근거

아래 경로는 모두 프로젝트 루트 기준이다. 실제 CSS 우선순위와 위젯 초기화까지 확인하고 적용한다.

### AI가 먼저 지켜야 하는 작업 원칙 — [필수·WORKLOG]

- 컴포넌트 디자인·DOM·클래스·상태의 우선 기준은 **현재 프로젝트의 `UiStandardRenewal.jsp`**다. 비교 원본인 `D:\agp_tbf\WASFProject`는 읽기 전용이며 실제 수정은 현재 프로젝트에만 한다.
- `#heder`, `util_fav`, `util_fav_tit_wrap`, `util_fav_list`와 메인 레이아웃은 현재 프로젝트 고유 구조를 보존한다. 비교 프로젝트의 메인 CSS·JS를 공통이라는 이유로 덮어쓰지 않는다.
- 사용자가 수정한 내용과 요청 범위 밖의 파일을 임의 정리·원복하지 않는다. 필요한 부분만 최소 수정한다. 작업 로그도 사용자가 기록을 요청한 경우에만 갱신한다.
- 가이드 예시만 맞추고 끝내지 않는다. 실제 업무 화면의 include 순서와 영향 범위를 확인한다. `GwbInclude.jsp`에서는 `renewal.css` 다음에 `ui_standard_renewal_components.css`를 로드하는 순서를 유지한다.
- 사용 중인 공통 클래스·유틸리티는 이름이 어색하거나 중복처럼 보여도 바로 삭제하지 않는다. 정리할 때는 `WebContent/webapps/gaia`의 실제 사용처를 먼저 조사하고 사용처와 CSS를 함께 변경한다.

| 근거 파일 | 용도 |
|---|---|
| `WebContent/html/css/root.css` | 컬러·글꼴·기본 높이 토큰 |
| `WebContent/html/css/common.css` | 기본 글꼴·버튼·상태·포커스 규칙 |
| `WebContent/html/css/renewal.css` | 입력창·콤보·날짜·상태 리뉴얼 |
| `WebContent/html/css/ui_standard_renewal_components.css` | 공통 버튼·탭 리뉴얼 |
| `WebContent/html/css/layout.css` | 업무 화면과 버튼 그룹 구조 |
| `WebContent/html/css/ui_standard_renewal.css` | 매뉴얼 전용 패널·검색·코드·반응형 |
| `WebContent/html/css/font.css` | 로컬 Pretendard 폰트 선언 |
| `WebContent/webapps/DeveloperGuide/UiStandardRenewal.jsp` | 표준 마크업·상태별 예시 |
| `WebContent/html/js/DeveloperGuide/UiStandardRenewal.js` | 검색·복사·소스 표시·탐색 |
| `WebContent/html/js/include/GwbCommon.js` | 공통 상태·개인정보 잠금 처리 |

## 2. 컬러

### 공통 토큰 — [기존]

CSS 변수 이름을 그대로 사용한다. 짧은 `primary` 하나로 네이비·브랜드색·액션색을 합치지 않는다.

| 토큰 | 값 | 용도 |
|---|---|---|
| `--primary-200` | `#011E41` | 네이비 강조, 매뉴얼 섹션 제목·활성 탐색 |
| `--primary-50` | `#F8F9FD` | 연한 테마 배경 |
| `--primary-100` | `#F2F2FC` | 테마 배경 변형 |
| `--color-brand-primary` | `#828CE9` | 기존 브랜드 토큰; 화면 용도 확인 후 사용 |
| `--color-brand-tab` | `#3B92E5` | 탭 관련 브랜드색 |
| `--color-surface` | `#FFFFFF` | 기본 표면 |
| `--color-surface-muted` | `#FAFAFA` | 비활성 입력 배경 |
| `--color-surface-hover` | `#F7F8FA` | 일반 버튼 hover |
| `--color-surface-table-head` | `#EDEFF2` | 테이블 헤더 |
| `--color-surface-focus` | `#EEFAFF` | 입력 포커스 배경 |
| `--color-text-primary` | `#111111` | 기본 글씨 |
| `--color-text-body` | `#333333` | 본문 글씨 |
| `--color-text-strong` | `#444444` | 기존 강조 글씨 토큰 |
| `--color-text-secondary` | `#666666` | 보조·비활성 글씨 |
| `--color-text-muted` | `#999999` | 약한 보조 글씨 |
| `--color-placeholder` | `#788991` | 입력 힌트 |
| `--color-border` | `#D9DADB` | 입력·일반 구분선 |
| `--color-border-default` | `#D3D4D6` | 기본 구분선 |
| `--color-border-subtle` | `#E8E9EB` | 약한 구분선 |
| `--color-border-strong` | `#AAAAAA` | 체크박스·라디오 경계 |
| `--color-action` | `#FF7C33` | 주요 액션 `.tyo` |
| `--color-action-hover` | `#FD6F21` | 주요 액션 hover·active |
| `--color-action-disabled` | `#FFB085` | 주요 액션 disabled |
| `--color-danger` | `#FF6767` | 오류 |
| `--color-danger-surface` | `rgba(255,242,242,0.90)` | 오류 입력 배경 |
| `--btn-tyg-base` | `#646464` | 저장 계열 `.tyg` 배경 |
| `--btn-tyg-hover` | `#777777` | 저장 계열 hover·active |
| `--btn-bd-color` | `#A0A8BA` | 일반 버튼 경계 |
| `--btn-tyg-disabled` | `#E8E8E8` | 일반 비활성 버튼 배경 |
| `--btn-org-disabled` | `#ADADAD` | 저장 비활성 배경 |

### 매뉴얼 전용 별칭 — [제안 이름 / 기존 값]

아래 이름은 기존 CSS 변수로 선언되어 있지 않다. 새 구현에서 필요할 때만 변수로 정의한다.

| 제안 토큰 | 기존 값 | 용도 |
|---|---|---|
| `--guide-border` | `#DFE4EB` | 매뉴얼 패널·행 경계 |
| `--guide-preview-surface` | `#F0F2F4` | 예시 외곽 배경 |
| `--guide-code-surface` | `#282C34` | 코드 영역 |
| `--guide-code-text` | `#FFFFFF` | 코드 글씨 |
| `--guide-toast-surface` | `#172033` | 복사 알림 |

## 3. 타이포그래피

- [기존] 기본 폰트: `"Pretendard Variable", Pretendard, sans-serif`. 로컬 폰트를 우선 사용하며 새 외부 폰트 의존성을 추가하지 않는다.
- [기존] 기본 크기 14px, 굵기 400. `root.css`의 행간 토큰은 1.5, 자간 토큰은 -0.02em이다. 토큰 선언만으로 모든 요소에 적용되었다고 가정하지 않는다.
- [기존] 숫자는 `tabular-nums`를 사용한다.
- [제안] 새 본문은 14px / 400 / 1.5 / -0.02em을 기본으로 한다. 버튼·입력·위젯의 고정 행간은 별도 규칙을 따른다.

| 역할 | 크기 | 굵기 | 행간 | 기준 |
|---|---:|---:|---:|---|
| 업무 화면 제목 | 20px | 700 | 1.4 | 제안; 기존 화면 제목 클래스 우선 |
| 업무 섹션 제목 | 16px | 600 | 1.5 | 제안 |
| 기본 본문·필드 라벨 | 14px | 400 | 1.5 | 기존 기본 크기·굵기 + 제안 행간 통일 |
| 보조 설명 | 12px | 400 | 1.5 | 제안 |
| 작은 버튼 `.btn_s` | 13px | 400 | 23px | 기존 |
| 매뉴얼 제목 | 28px | 700 | 1.35 | 기존; 600px 이하에서 23px |
| 매뉴얼 소개 본문 | 14px | 400 | 1.65 | 기존 |
| 매뉴얼 탐색 | 13px | 400 / 활성 700 | 기존 클래스 유지 | 기존 |
| 코드 | 12px | 400 | 1.7 | 기존; `Consolas, "Courier New", monospace` |

## 4. 레이아웃·모양

### 업무 화면

- [기존] `.head_area`에 액션, `.data_table.search`에 검색 조건, `.title_area`에 영역 제목·보조 액션을 배치하는 구조를 재사용한다.
- [기존] 상단 액션 영역은 35px 높이, 버튼 그룹 간격은 8px이다. 일반 입력·콤보 높이는 30px, 작은 입력은 28px이다.
- [기존] 일반·작은 버튼 모서리는 2px이다. 도움말 버튼 등 기존 둥근 예외는 유지한다.
- [제안] 신규 간격 단위는 4px로 한다: `space-1 4px`, `space-2 8px`, `space-3 12px`, `space-4 16px`, `space-5 20px`, `space-6 24px`, `space-8 32px`.
- [제안] 신규 업무 패널은 흰 배경, 1px 구분선, 반경 2px, 기본 padding 16px, 그림자 없음으로 한다. 기존 입력 padding 6px 9px·버튼 padding 13px 등은 4px 단위로 강제 변경하지 않는다.
- [제안] 좁은 화면은 액션 줄바꿈과 테이블 영역 내부 스크롤로 대응한다. 기존 업무 화면의 모바일 지원이 검증되었다고 표시하지 않는다.

### 긴 콘텐츠·팝업·스크롤 — [필수·WORKLOG]

- DHTMLX `openDhxWindow()` 팝업은 `#Layer > .pop_con` 구조를 유지한다. 일반 layer 팝업의 `.layer_pop_wrap`, `.pop_area`, `.pop_cont`, `.pop_wrap`을 그대로 추가하지 않는다. 높이 `auto`의 불필요한 중간 래퍼가 그리드 높이 계산을 끊을 수 있다.
- 긴 팝업은 `.pop_con.content_expand`로 실제 콘텐츠 끝까지 하단 패딩을 감싼다. 긴 문서 화면은 `.content_area.content_expand`의 `height:auto; min-height:100%` 구조를 사용한다.
- `scroll_wrap`은 일부 영역을 의도적으로 독립 스크롤할 때만 사용한다. 하단 패딩 문제를 해결하려고 새 스크롤 래퍼를 넣지 않는다.
- 생보·장기 계약 화면 `CTCntrRegMng.jsp`, `CTCntrRegMngPop.jsp`는 `#document`에서 스크롤한다. 우측 `.wrap_right`에 `scroll_wrap`을 넣지 않고 내용형 탭은 `detail-pg-tabs`를 유지한다. 일반적인 테이블 내부 스크롤 제안보다 이 화면별 규칙이 우선한다.
- DHTMLX가 생성하는 `.dhx_cell_tabbar`, `.dhx_cell_cont_tabbar`는 제거하지 않는다. 높이·overflow 문제는 부모 경로와 실제 computed style·인라인 높이를 확인한다. 폐기했던 `tab_document_flow`, `body_document_scroll` 보정을 재도입하지 않는다.
- 로그인 본문과 해당 비밀번호 변경·초기화 팝업은 `GaiaMaiPot01.css`의 화면·ID 범위를 유지하고 공통·리뉴얼 CSS 뒤에서 로드한다. 여러 화면이 사용하는 `.custom_popup` 골격은 공통에 유지한다.

### 매뉴얼 외곽 — [기존]

- 콘텐츠 최대 너비 1480px, 가운데 정렬. 좌우 여백은 기본 40px, 900px 이하 20px, 600px 이하 12px이다.
- 상단 고정 탐색 높이를 계산해 본문 위 여백에 반영한다.
- 소개 패널: 반경 10px, padding 26px 28px, 그림자 `0 8px 24px rgba(17,17,17,.05)`.
- 예시 패널: 반경 8px, 아래 간격 18px. 헤더 padding 16px 20px, 미리보기 padding 10px.
- 매뉴얼 검색·도구 버튼: 높이 38px, 반경 6px. 소스 액션: 높이 30px.
- 그림자는 상단 탐색·소개 패널·토스트처럼 이미 사용하는 곳에 한정한다. 매뉴얼 안에 들어가는 ERP 예시 자체는 업무 컴포넌트 규칙을 유지한다.

## 5. 컴포넌트

### 버튼

- [기존] 일반 액션은 `.btn-darum.btn_m`: 높이 28px, 좌우 padding 13px, 행간 26px, 반경 2px, 흰 배경, 1px 버튼 경계.
- [기존] 좁은 영역은 `.btn_s`: 높이 24px, 최소 너비 34px, 좌우 padding 7px, 13px 글씨, 반경 2px.
- [기존] 조회 등 강조 액션은 `.tyo`, 저장 계열은 `.tyg`. 기존 업무 의미를 유지한다.
- [기존] hover·active·disabled 스타일과 기존 아이콘 자산을 재사용한다. 색·경계 전환은 공통 버튼의 0.2초 규칙을 따른다.
- [제안] 버튼 문구는 조회·저장·복사처럼 행동을 직접 명시한다. 클릭 불가 상태는 실제 `disabled` 또는 공통 차단 로직으로 보장하고 스타일만 바꾸지 않는다.

### 입력창·콤보·날짜

- [기존] `.gwbInput`, `.gwbCombo`, `.class_datepicker`, `.class_monthpicker`와 공통 초기화를 재사용한다.
- [기존] 일반 텍스트 입력은 높이 30px, padding 6px 9px이다. 기본 리뉴얼 입력은 `border: none`이며 경계는 line-input·검색·날짜 래퍼 등 해당 예시 구조에 따라 결정한다.
- [기존] focus 배경은 `--color-surface-focus`, `.is-error` 배경은 `--color-danger-surface`다.
- [기존] 필수 표시는 매뉴얼의 대상 요소에 `.mandatory`를 적용한다. 읽기 전용 `.readonly-ty2`, 비활성 `.gwbDisabled` / `disabled`는 해당 예시를 각각 사용한다.
- [기존] `.readonly-mask`는 개인정보 공통 코드가 처리한다. 화면에서 직접 선언해 잠금·권한 동작을 흉내 내지 않는다.
- [제안] placeholder는 입력 힌트로만 사용하고 라벨을 대신하지 않는다. 오류는 색상과 설명을 함께 제공한다.
- [제안] 새로운 키보드 조작 요소에는 보이는 포커스 표시를 제공한다. 기존 CSS의 outline 제거를 근거로 포커스 표시를 생략하지 않는다.
- [필수·WORKLOG] 로그인 비밀번호 입력에 상시 가짜 포커스 하이라이트 `.input_wrap_pw::after`를 다시 넣지 않는다. 실제 focus·자동완성 스타일은 유지하고, 저장 사원번호 복원 시 `USER_PW` 자동 포커스를 재도입하지 않는다.
- [필수·WORKLOG] `.search_area.clear_on_value`는 선택형 옵션이다. 직계 입력에 placeholder를 두고 값이 있을 때만 `.btn_delete`를 표시한다. 주소·상세주소 등 연관 데이터의 기존 삭제 로직은 유지한다.
- [필수·WORKLOG] 날짜·월·연도·기간 달력은 `suite.custom.css`의 `transform:translate(-2px, 2px)` 공통 보정을 유지한다. DHTMLX가 계산한 inline `top`·`left`를 임의의 절대 좌표로 덮어쓰지 않는다.

### 카드·패널

- [제안] 일반 업무 정보는 테이블·영역 구분을 우선한다. 신규 업무 패널은 2px 반경과 얇은 경계, 그림자 없는 규칙을 따른다.
- [기존] 매뉴얼 소개·예시 패널의 10px / 8px 반경은 매뉴얼 외곽에만 적용한다.
- [제안] 동일한 종류의 패널은 제목 위치·padding·배경을 통일한다.

### 테이블·탭

- [기존] `.data_table`, `.title_area`, 공통 DHTMLX·탭 래퍼의 마크업과 초기화 방식을 유지한다.
- [제안] 숫자는 오른쪽 정렬, 라벨·설명은 왼쪽 정렬한다. 업무 의미상 예외는 기존 규칙을 따른다.
- [필수·WORKLOG] **탭 활성화 전후 글자 굵기를 바꾸지 않는다.** 일반 상단 탭은 양쪽 모두 600, 하위 pill 탭·`.second_tab`은 모두 400, 메인 `.main_tab`은 모두 500이다. 활성 상태는 기존 글자색·배경·테두리로 구분한다. 매뉴얼 섹션 바로가기의 활성 700은 DHTMLX 탭과 다른 요소다.
- [필수·WORKLOG] 굵기 차이 때문에 발생하는 탭 너비 흔들림은 CSS로 해결한다. 이 문제만을 위해 JavaScript 너비 보정을 추가하지 않는다.
- [필수·WORKLOG] 메인 탭 이동 보정은 메인 인스턴스에만 적용한다. 좌우 화살표는 처음·끝 탭으로 이동하고 실제 표시 영역·탭 좌표로 범위를 계산한다. 고정 200px 이동을 재도입하지 않는다. 우측 액션 영역 126px 예약과 추가·닫기·리사이즈 후 활성 탭 전체 표시를 보존한다.
- [제안] 탭 키보드 접근을 지원하되 위 굵기·크기 보존 규칙을 따른다.

### 검색 테이블·그리드 — [필수·WORKLOG]

- `.data_table.search`의 `tr`, `th`, `td`와 checkbox·radio·hidden을 제외한 input의 `height:30px !important`를 유지한다. 단순 중복·강제 선언처럼 보여도 제거하지 않는다. 과거 제거 후 입력·셀렉트·행 배치가 깨져 복구한 규칙이다.
- 행 테두리를 없애거나 inset shadow로 대체하지 않는다. 입력·셀렉트의 배경색 경계가 가려진다.
- `dhxSuiteGrid.js` 기본 `headerRowHeight:30`, `rowHeight:30`, `footerRowHeight:35`를 유지한다. 페이지가 별도 지정한 특수 그리드 값은 보존한다.
- 조회·페이징 후 `scrollGridToTop()` 동작, `edit.type:"button2"`의 `dhx_grid-cell-button-value`와 셀 우측 버튼 배치를 유지한다.
- 팝업 그리드 폭을 채울 때 모든 열을 무조건 늘리지 않는다. 화면 설정에 따라 `ratio:true` 또는 다른 열 `autoWidth:false` + 지정한 가변 열을 사용한다. 선택 체크박스·숨김 열·마스킹 키·최소 너비는 유지한다.
- 남은 폭을 채우는 텍스트는 기존 `.txt_fill {flex:1;min-width:0;}`를 사용한다. 도움말은 실제 버튼 바로 뒤에 `.help_con`을 배치하고 공통 토글을 사용한다. 숨겨진 대리 버튼, 페이지 전용 11px 글꼴·15px 행 높이·인라인 보정을 재도입하지 않는다.

### 로더·아이콘·공통 초기화 — [필수·WORKLOG]

- 공통 문서 로더 `$.showDocProgressBar()`는 `.doc-loader > .spinner`의 주황색 CSS 스피너 `#FF7C33`만 표시한다. 녹색 inline SVG와 `Loading` 문구를 다시 넣지 않는다.
- `FileCommon.js`의 파일 업로드 로더는 별도 기능이다. 문서 로더 요청에 묶어서 변경하지 않는다.
- 이미지·아이콘은 현재 프로젝트에 존재하는 경로를 사용한다. 공통 CSS 수정·include 추가 시 참조 이미지 누락을 검사한다.
- `$.gwbMaskIsUnmask`, `$.resolveMaskFormat`, `$.gwbMaskReveal`과 현재 `GwbCommon.js` 구조를 보존한다. 마스킹 공통 함수 누락으로 그리드·콤보·트리 초기화가 함께 중단되지 않도록 확인한다.

### 매뉴얼 검색·소스·알림

- [기존] 검색 결과 수, 초기화, 결과 없음 안내를 함께 제공한다.
- [기존] 예시 구조는 `.guide-header` → `.guide-body` → `.code-block`이다. 각 예시에 소스 보기·복사를 제공한다.
- [기존] 소스는 초기 미리보기 마크업과 동기화하며 Chip 섹션은 예외 처리한다. 예외 코드를 임의로 덮어쓰지 않는다.
- [기존] 복사 결과 토스트는 우측 하단, 반경 6px, 흰 글씨·짙은 배경이며 약 1.6초 표시된다.
- [필수·WORKLOG] 새 컴포넌트 추가 시 컴포넌트명과 `(yy/mm/dd)`를 최신순 번호 1~5로 최근 업데이트에 표시한다. 현재 `data-guide-added`와 자동 목록 생성 방식을 확인해 반영하고 수동·자동 목록을 중복 생성하지 않는다.

## 6. Do / Don't

아래 항목은 AI 결과 검토 게이트다. WORKLOG에서 가져온 필수 규칙과 이 문서의 제안을 포함한다. 기존 구현 전체가 이미 충족했다는 뜻은 아니다.

### Do — 모두 충족해야 함

1. **현재 프로젝트 기준을 재사용한다.** `UiStandardRenewal.jsp`를 먼저 대조하고 실제 업무 include 순서와 화면을 함께 확인한다. 신규 색은 위 토큰을 참조한다. 공통 클래스·사용자 변경·메인 고유 구조를 보존한다.
2. **상태와 행동을 함께 검증한다.** 대상 요소의 기본·hover·focus·disabled·readonly·error 중 해당 상태를 확인한다. 필수값 검증, 키보드 포커스, 개인정보 잠금과 기존 조회·저장 동작을 유지한다.
3. **실제 회귀를 검증한다.** 검색 행·입력 30px, 그리드 기본 높이와 화면별 예외, 활성 전후 탭 너비, 팝업 높이·하단 패딩·기존 스크롤 위치를 확인한다. JavaScript 수정 시 `node --check`, 공통 CSS 수정 시 이미지 경로 검사, 브라우저에서는 캐시 제거·강력 새로고침 후 확인한다. 정적 검사만 했다면 실제 동작은 미확인으로 기록한다.

### Don't — 발견 시 수정 필요

1. **장식 목적으로 새로운 스타일을 추가하지 않는다.** 승인된 기존 예외 외의 그라데이션·대형 그림자·전면 pill 버튼·임의 브랜드색·외부 폰트를 추가하지 않는다.
2. **상태를 겉모양으로만 구현하지 않는다.** 비활성처럼 보이는데 클릭되는 버튼, placeholder만 있는 필드, 설명 없는 오류, 수동 `.readonly-mask`로 만든 잠금 표시를 만들지 않는다.
3. **문제를 복구하며 남긴 규칙을 되돌리지 않는다.** 검색 높이 강제 규칙 삭제, 행 테두리의 그림자 대체, 탭 활성 시 굵기 변경, 팝업의 불필요한 래퍼·중복 스크롤, DHTMLX 생성 클래스 제거를 금지한다. 매뉴얼 전용 스타일·제안값·비교 프로젝트 메인 스타일을 업무 화면 전역에 덮어쓰지 않는다.
