# R4 산출물

## 단계별 산출물

| 단계 | 산출물 | 완료 판정 |
|---|---|---|
| 상태 관리 | `runs/<작업ID>/status.json` | 필수 키 4개가 존재하고 JSON 파싱 성공 |
| 1. 대상·기준 확인 | `runs/<작업ID>/01-scope.md` | 대상 파일 1개 이상, 변경 요청 1개 이상, 표준 예시 1개 이상 |
| 2. 구조 비교 | `runs/<작업ID>/02-gap-analysis.md` | 발견 항목마다 파일 위치·현재 구조·차이 내용이 존재 |
| 3. 기존 구조 매핑 | `runs/<작업ID>/03-component-map.md` | 각 차이 항목에 기존 DOM·클래스·사용 근거 1개 이상 연결 |
| 4. 최소 수정 | `runs/<작업ID>/04-change-report.md` | 수정 파일·변경 내용·보존 동작 기록, 신규 클래스 0개 |
| 5. 검증·기록 | `runs/<작업ID>/05-validation.md` | 각 필수 항목에 결과와 증거가 존재하고 `FAIL` 0개 |

## 기준과 경로

| 항목 | 값 |
|---|---|
| 실행별 폴더 | `runs/<작업ID>/` |
| 규칙 SSOT | `docs/design.md` 1개 |
| 구현 근거 | `UiStandardRenewal.jsp`와 동종 정상 페이지 |
| 재개 가능 | 예 |

## 상태 파일

`runs/<작업ID>/status.json`은 다음 키를 모두 포함해야 한다.

- `current_stage`
- `completed_stages`
- `last_failure`
- `updated_at`

## 재개 판정

- 단계 파일이 없거나 필수 항목이 비어 있으면 해당 단계는 완료로 인정하지 않는다.
- `status.json`과 단계 파일이 충돌하면 단계 파일의 실제 존재 여부와 필수 항목 충족 결과를 우선한다.
