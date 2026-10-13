# ERP UI 리뉴얼 하네스

## 목적

이 하네스는 기존 프로젝트 ERP 업무 화면의 임의 구조를 찾아 기존 DOM·클래스·공통 컴포넌트 구조로 교정하고 검증한다.

새 클래스를 만들지 않는다. ERP 업무 규칙과 데이터 모델을 재설계하지 않는다.

## 실행 트리거

다음 조건을 모두 만족할 때만 실행한다.

1. 사용자가 ERP 화면을 기존 공통 컴포넌트 기준으로 리뉴얼해 달라고 요청한다.
2. 대상 화면 또는 파일이 1개 이상 지정되어 있다.

단순 설명·분석·검토 요청에는 대상 프로젝트를 수정하지 않는다.

## 기준 문서

작업 전에 다음 문서를 읽는다.

- 서비스 맥락: `docs/story-service.md`
- 실제 작업 흐름: `docs/story-work.md`
- 목적과 완료 조건: `harness/purpose.md`
- 단계별 파이프라인: `harness/pipeline.md`
- 산출물과 재개 규칙: `harness/artifacts.md`
- 자동·사람 게이트: `harness/gates.md`
- 역할과 편집 범위: `harness/roles.md`
- 오케스트레이션: `harness/orchestration.md`
- 검증과 리뷰: `harness/validation.md`
- 규칙 SSOT: `docs/design.md`

`reference.md`는 QA 체크리스트와 정상 화면 후보를 확인할 때 보조 자료로 사용한다.

규칙 적용 순서는 다음과 같다.

1. 사용자의 현재 요청
2. `docs/design.md`의 필수 규칙
3. 현재 프로젝트의 실제 구현
4. `reference.md`

## 구현 근거 우선순위

1. `WebContent/webapps/DeveloperGuide/UiStandardRenewal.jsp`
2. `reference.md`에 기록된 정상 후보
3. 대상과 같은 유형의 실제 업무 화면

비교 프로젝트나 오래된 문서의 코드를 현재 구현보다 우선하지 않는다.

## 실행 폴더

실행마다 `runs/<작업ID>/`를 사용한다.

`status.json`에는 다음 값을 기록한다.

- `current_stage`
- `completed_stages`
- `last_failure`
- `updated_at`
- `edit_root`
- `approval`

`edit_root` 기본값은 다음과 같다.

`D:\project\erp-web\WebContent\`

`edit_root`는 실행마다 절대경로 1개만 허용한다. 모든 대상 프로젝트 수정은 정규화된 `edit_root` 내부에서만 수행한다.

## 역할

### 조사 에이전트

- 1~3단계를 수행한다.
- `runs/<작업ID>/`만 편집한다.
- 대상 프로젝트를 수정하지 않는다.

### 작업 에이전트

- H1 승인 후에만 실행한다.
- 승인된 매핑 범위만 수정한다.
- 실행별 `edit_root` 1개 안에서만 편집한다.

### 읽기 전용 판정자

- 5단계와 G1~G4를 판정한다.
- 하네스와 외부 프로젝트를 읽을 수 있다.
- 어떤 파일도 수정하지 않는다.
- 검사 전후 변경 파일 증가량은 0개여야 한다.

두 에이전트가 동시에 파일을 수정하지 않는다.

## 파이프라인

1. `SCOPING`: 대상 범위와 표준 예시를 `01-scope.md`에 기록하고 G1을 판정한다.
2. `ANALYZING`: 임의 구조와 표준 차이를 `02-gap-analysis.md`에 기록한다.
3. `MAPPING`: 각 차이를 기존 구조에 연결하고 `03-component-map.md`를 작성한 뒤 G2를 판정한다.
4. `AWAITING_APPROVAL`: 매핑표를 사용자에게 보여주고 중단한다.
5. `IMPLEMENTING`: H1 승인 후 최소 수정하고 `04-change-report.md`를 작성한다.
6. `JUDGING`: `05-validation.md`를 작성하고 G3·G4를 판정한다.
7. 모든 조건을 통과하면 `COMPLETED`로 종료한다.

## H1 사람 승인

H1은 실행당 유일한 사람 승인 지점이다.

승인 전 대상 프로젝트 수정 파일 수는 반드시 0개여야 한다. 승인 범위는 `03-component-map.md`의 SHA-256으로 고정한다.

승인 후 다음 중 하나가 바뀌면 승인을 무효화하고 H1으로 돌아간다.

- `03-component-map.md`
- `edit_root`
- 승인된 수정 대상

## 필수 게이트

- G1: 대상 파일, 변경 요청, 표준 예시가 각각 1개 이상
- G2: 모든 차이 항목에 기존 구조와 실제 사용 근거가 각각 1개 이상
- G3: 프로젝트에 없던 class 토큰과 신규 CSS 클래스 선택자가 각각 0개
- G4: 필수 검사 `FAIL` 0개, 사유 없는 필수 `미확인` 0개

G3는 수정 전 프로젝트 전체의 HTML·JSP·JS·CSS 클래스 목록을 기준으로 판정한다.

게이트 실패 시 `harness/gates.md`에 지정된 단계로 돌아간다. 자동 수정·재검증은 최대 2회 허용하며, 세 번째 실패 시 `FAILED`로 종료하고 원인을 기록한다.

## 완료 조건

다음을 모두 만족해야 완료로 보고한다.

- 신규 클래스 0개
- 표준 구조로 매핑되지 않은 임의 구조 0개
- 각 구조 변경에 연결된 표준 예시 또는 정상 페이지 근거 1개 이상
- 필수 검사 `FAIL` 0개
- 사유 없는 필수 `미확인` 0개
- 요청 범위 밖 수정 0개
- 단계별 산출물과 `status.json`의 불일치 0개

실제 브라우저 검증을 수행하지 못했다면 완료로 보고하지 말고 필요한 환경과 다음 확인 방법을 기록한다.
