# 35. 재해 복구 — DR

## 개념 이해

**RPO**는 허용 가능한 데이터 손실의 시간 범위, **RTO**는 허용 가능한 서비스 복구 시간입니다. 어제 백업으로 되돌아가면 손실 범위가 크고, 복원에 오래 걸리면 중단 시간이 길어집니다. 서로 다른 지표입니다.

## 핵심 개념

| 전략 | DR 쪽 상태 | 일반적인 절충 |
|---|---|---|
| Backup & Restore | 백업으로 재구성 | 낮은 상시 비용, 긴 복원·기동 시간 |
| Pilot Light | 데이터·핵심 기능 유지, 추가 앱 용량은 복구 시 준비 | 작게 유지, 복구 자동화 중요 |
| Warm Standby | 축소된 완전한 환경이 실행 중 | 확장해 전환, 상시 비용 증가 |
| Multi-site Active-Active | 여러 사이트가 실제 요청 처리 | 빠른 전환에 유리, 데이터·충돌·비용 복잡성 |

표의 시간·비용은 상대적인 경향입니다. **Active-Active라고 RPO·RTO가 무조건 0은 아니고, Pilot Light가 반드시 10분 안에 복구 불가능한 것도 아닙니다.** 데이터 복제 방식·앱 준비·장애 탐지·클라이언트 전환·실제 복구 훈련으로 검증합니다.

### 지원 서비스

| 서비스 | 역할 |
|---|---|
| Aurora Global Database | 관계형 DB의 리전 간 읽기·DR |
| DynamoDB Global Tables | 다중 리전 NoSQL 읽기·쓰기 |
| S3 CRR | 다른 리전 버킷 복제 |
| AWS Backup | 지원 서비스의 중앙 백업 계획·복사·보존 |
| Backup Vault Lock | 모드·유예 기간·규칙에 따른 보존 보호 |
| Elastic Disaster Recovery | 서버 블록 수준 지속 복제와 복구 |
| Route 53 Failover | DNS 기반 사이트 전환 |
| DMS | 지원 DB 변경 데이터 복제·이전 |

Multi-AZ는 보통 AZ 장애 대응이며 리전 전체 장애에 대한 DR과 다릅니다. 복제는 실수·악성 삭제도 전파할 수 있으므로 복제만으로 백업을 대체하지 않습니다. 글로벌 DB의 안내 RPO·RTO를 모든 장애에서의 보장값으로 해석하지 마세요.

## 문제 유형

1. 몇 시간 중단 허용, 최소 상시 비용 → Backup & Restore부터 비교.
2. DB는 복제 중이고 앱 서버는 재해 때 시작 → Pilot Light.
3. 축소된 앱 전체가 항상 실행 → Warm Standby.
4. 여러 리전에서 실제 요청 처리 → Active-Active, 데이터 일관성 검토.
5. 많은 온프레미스 서버의 AWS DR → DRS 지원 조건 검토.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 데이터 손실 허용 | RPO |
| 중단 시간 허용 | RTO |
| 백업으로 재구성 | Backup & Restore |
| 핵심만 유지 | Pilot Light |
| 작지만 전체 환경 실행 | Warm Standby |
| 여러 사이트 실서비스 | Active-Active |
| 중앙 백업 정책 | AWS Backup |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/35-Disaster-Recovery.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
