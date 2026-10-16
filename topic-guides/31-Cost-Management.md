# 31. 비용 관리 도구

## 개념 이해

Cost Explorer는 지출 내역 앱, Budgets는 예산 경보기, 비용 보고서는 원본 영수증, Anomaly Detection은 평소와 다른 지출을 찾는 탐지기입니다. 문제에서 원하는 동사를 먼저 찾으세요.

## 핵심 개념

| 도구 | 목적 |
|---|---|
| Cost Explorer | 과거 지출 분석·필터·예측·지원 약정 권장 |
| AWS Budgets | 실제·예측 비용/사용량 임계값 알림, 지원 작업 |
| CUR·Data Exports | 상세 비용·사용량 데이터를 S3 등으로 내보내 분석 |
| Cost Anomaly Detection | 평소 패턴과 다른 비용 탐지 |
| Compute Optimizer | 지원 리소스 크기·구성 최적화 권장 |
| Cost Allocation Tags | 팀·프로젝트별 비용 배분 |
| Billing Conductor | 조직의 맞춤 청구 관점·요율 표시 |

상세 보고서는 Athena·QuickSight 등으로 분석합니다. Budgets는 알림을 설정했다고 지출을 무조건 실시간 차단하는 기능이 아니며 데이터 갱신 지연·지원 조치 범위를 고려합니다. 이상 탐지도 즉시 모든 초과 비용을 막는 것은 아닙니다.

비용 배분 태그는 리소스에 태그를 붙이고 Billing에서 활성화해야 합니다. **원문의 “과거 소급 불가”는 오래된 설명**입니다. 관리 계정에서 최대 12개월까지 backfill을 요청할 수 있지만 과거 실제 리소스에 존재했던 태그 값에 한정됩니다.

RI·Savings Plans는 약정 할인이고, Cost Explorer는 구매·활용 상태를 분석하는 도구입니다. 약정은 무조건 적게 보이는 가격이 아니라 사용률·유연성·중복 약정까지 검토합니다. Billing Conductor의 표시 금액은 AWS의 실제 청구 금액 자체를 바꾸는 것이 아닙니다.

## 문제 유형

1. 월 예산의 예상 80% 도달 시 알림 → Budgets.
2. 어떤 서비스가 지난달 비용을 늘렸나 → Cost Explorer.
3. 세부 항목을 SQL로 분석 → CUR·Data Exports + Athena.
4. 예상 밖 비용 급증 → Cost Anomaly Detection.
5. 과도하게 큰 EC2 크기 조정 → Compute Optimizer.
6. 팀별 비용 배분 → 활성화된 비용 배분 태그, 필요 시 backfill.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 분석·예측 | Cost Explorer |
| 임계값 알림 | Budgets |
| 상세 원시 데이터 | CUR·Data Exports |
| 이상 지출 | Anomaly Detection |
| 적정 크기 | Compute Optimizer |
| 부서·프로젝트 비용 | Cost Allocation Tags |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/31-Cost-Management.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
