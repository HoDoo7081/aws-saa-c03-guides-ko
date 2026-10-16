# 18. EventBridge와 Step Functions

## 개념 이해

EventBridge는 이벤트를 보고 목적지로 보내는 배차 담당자, Step Functions는 정해진 여정을 단계별로 진행하는 관리자입니다. **이벤트 라우팅**과 **워크플로 오케스트레이션**을 구분하세요.

## 핵심 개념

### EventBridge

AWS 서비스·파트너 SaaS·사용자 앱의 이벤트를 버스로 받고, JSON 이벤트 패턴 규칙으로 대상을 선택합니다. Lambda·SQS·Step Functions·ECS 등 지원 대상을 호출할 수 있습니다.

- Partner event bus: 지원 SaaS 이벤트 연결.
- Archive·Replay: 이벤트 보관과 재처리.
- Cross-account bus: 여러 계정 이벤트 중앙화.
- Schema registry: 이벤트 구조 이해·코드 연동 지원.
- EventBridge Scheduler: cron·rate·일회성 일정 등 예약 실행. 기존 scheduled rule도 있으나 다른 기능임.

SNS도 본문 기반 필터링을 지원하므로 “콘텐츠 필터링이면 무조건 EventBridge”라고 외우지 않습니다. AWS·SaaS 이벤트 통합, 보관·재생, 대상 서비스와 fan-out·알림 채널 요구를 함께 비교하세요.

### Step Functions

상태 머신으로 Lambda·ECS·AWS API 등을 단계별 조정합니다.

| 요소 | 기능 |
|---|---|
| Task | 작업 수행 |
| Choice | 조건 분기 |
| Wait | 대기 |
| Parallel | 병렬 분기 |
| Map | 항목별 반복 |
| Retry·Catch | 재시도·오류 처리. 별도 상태 유형이 아닌 처리 필드 |

| 유형 | 적합한 상황 |
|---|---|
| Standard | 최대 1년의 장기 작업, 승인·감사, 상태 전환 기준 과금 |
| Express | 최대 5분의 짧고 빈번한 실행, 실행 횟수·시간·메모리 기준 과금 |

함수 A가 B를 호출하는 복잡한 체인 대신 워크플로에 순서·재시도를 드러낼 수 있습니다. 다만 Step Functions가 개별 Lambda의 15분 제한을 늘리는 것은 아닙니다. 긴 단일 작업은 ECS·Batch 등으로 실행합니다. 실행 이력·로그·보존 기간과 IAM 권한도 구성해야 합니다.

## 문제 유형

1. SaaS 이벤트를 조건에 따라 AWS로 연결 → EventBridge.
2. 매일 새벽 작업 → Scheduler.
3. 며칠 걸리는 승인과 재시도 → Standard Workflow.
4. 배열의 각 항목 처리 → Map.
5. 수정 후 지난 이벤트 재처리 → Archive + Replay.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 이벤트 수신·라우팅 | EventBridge |
| 일정 실행 | Scheduler |
| 단계·분기·재시도 | Step Functions |
| 장기·사람 승인 | Standard |
| 짧고 빈번한 실행 | Express |
| 반복 / 조건 분기 | Map / Choice |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/18-EventBridge-StepFunctions.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
