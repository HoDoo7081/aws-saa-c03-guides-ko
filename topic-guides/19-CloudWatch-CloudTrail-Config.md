# 19. CloudWatch, CloudTrail과 Config

## 개념 이해

CloudWatch는 환자의 상태 모니터, CloudTrail은 누가 무엇을 했는지 적는 방문 기록, Config는 과거 설정과 규정 준수 기록입니다. 문제의 동사를 먼저 봅니다. **느리다·경보 → CloudWatch, 누가 바꿨나 → CloudTrail, 어떤 설정이었나 → Config**입니다.

## 핵심 개념

### CloudWatch

지표·로그·경보·대시보드를 다룹니다. EC2 기본 지표에는 CPU·네트워크 등이 있지만 일반적인 메모리 사용률·파일시스템 사용량에는 **CloudWatch Agent 또는 별도 수집**이 필요합니다.

- Metric filter: 로그 패턴을 지표로 변환.
- Logs Insights: 로그를 대화형으로 조회.
- Subscription filter: 로그를 Lambda·Kinesis·Firehose 등 지원 대상으로 전달.
- Composite alarm: 여러 경보 상태를 묶어 잡음 감소.
- Alarm action: SNS·Auto Scaling·지원 EC2 작업 등.

### CloudTrail

지원되는 API 활동의 신원·시각·소스·요청을 감사합니다. 기본 이벤트 이력은 최근 **90일의 관리 이벤트**입니다. 객체 읽기·쓰기 같은 데이터 이벤트까지 모두 자동 기록되는 것은 아닙니다.

장기 보관은 Trail→S3 또는 적절한 CloudTrail Lake 구성을 검토합니다. 여러 리전·계정의 기록 범위를 확인하세요. S3 객체 접근자는 **데이터 이벤트 활성화** 여부가 핵심입니다. 로그 파일 무결성 검증은 변조 여부를 탐지하는 수단이며 로그 삭제를 물리적으로 막는 기능과 다릅니다. Insights는 비정상적인 API 활동 탐지에 사용합니다.

### AWS Config

지원 리소스의 구성 이력·관계·규정 준수를 기록·평가합니다. 레코더 범위와 기록 빈도를 설정해야 하며 모든 리소스가 무조건 실시간 기록되는 것은 아닙니다. 관리형·사용자 규칙과 SSM Automation 기반 교정(Remediation)을 사용할 수 있습니다.

Config는 기본적으로 **탐지·사후 교정** 도구입니다. 작업 자체를 예방하려면 IAM·SCP 등 적용 가능한 권한 통제를 검토합니다. Guardrails 제품에 Config가 포함될 수 있다는 사실과 개별 작업의 예방 기능을 혼동하지 마세요.

마이크로서비스 요청의 어느 구간이 느린지 추적하려면 분산 추적(X-Ray·관련 관찰 도구)을 검토합니다.

## 문제 유형

1. 누가 EC2를 종료했나 → CloudTrail.
2. EC2 메모리 80% 경보 → Agent 수집 + 지표 + Alarm.
3. 로그 ERROR 건수 경보 → Metric filter + Alarm.
4. 지난 화요일 보안 그룹 설정 → Config 이력.
5. 전 세계 SSH 공개 설정을 탐지하고 수정 → Config 규칙 + Remediation.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 성능·상태·경보 | CloudWatch |
| API 주체 감사 | CloudTrail |
| 구성 이력·준수 평가 | Config |
| 객체 단위 접근자 | CloudTrail 데이터 이벤트 |
| 로그 패턴 수치화 | Metric filter |
| 실시간 로그 전달 | Subscription filter |
| API 작업 사전 제한 | IAM·SCP |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/19-CloudWatch-CloudTrail-Config.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
