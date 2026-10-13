# R7 오케스트레이션

## 상태 흐름

| 순서 | 상태 | 실행 주체 | 다음 조건 |
|---:|---|---|---|
| 1 | `SCOPING` | 조사 에이전트 | `01-scope.md` 작성 후 G1 통과 |
| 2 | `ANALYZING` | 조사 에이전트 | `02-gap-analysis.md` 작성 |
| 3 | `MAPPING` | 조사 에이전트 | `03-component-map.md` 작성 후 G2 통과 |
| 4 | `AWAITING_APPROVAL` | 오케스트레이터·사용자 | H1 승인 전 즉시 중단 |
| 5 | `IMPLEMENTING` | 작업 에이전트 | 승인된 범위와 `edit_root` 안에서만 수정 |
| 6 | `JUDGING` | 읽기 전용 판정자 | G3·G4 통과 |
| 7 | `COMPLETED` | 오케스트레이터 | 모든 단계 파일과 게이트 통과 확인 |
| 실패 | `FAILED` | 오케스트레이터 | 실패 원인과 복귀 단계 기록 후 중단 |

## 운영 규칙

- 에이전트는 순차 실행하며 동시에 둘 이상이 파일을 수정하지 않는다.
- `AWAITING_APPROVAL`에서는 `03-component-map.md`만 사용자에게 보여주고 중단한다.
- H1 승인 전에는 작업 에이전트를 시작하지 않는다.
- 자동 게이트 실패 후 자동 수정·재검증은 최대 2회 허용한다.
- 최초 실패와 재시도 2회를 합쳐 세 번째 실패가 발생하면 `FAILED`로 전환한다.
- 실패 시 `status.json`에 게이트명, 실패 조건, 시도 횟수, 복귀 단계를 기록한다.

## `AGENTS.md` 참조 문서

루트 `AGENTS.md`는 세부 규칙을 중복 복사하지 않고 다음 문서를 참조한다.

- `harness/purpose.md`
- `harness/pipeline.md`
- `harness/artifacts.md`
- `harness/gates.md`
- `harness/roles.md`
- `docs/design.md`
