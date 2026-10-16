# 09. RDS와 Aurora

## 개념 이해

RDS는 DB의 기반 운영을 맡아 주는 관리형 관계형 데이터베이스입니다. 일반 RDS에는 OS에 SSH로 접속하는 권한이 없습니다. 지원 범위 밖의 OS·엔진 제어가 필요하면 EC2 자체 운영이나 지원되는 RDS Custom을 검토합니다.

**고가용성은 고장에 대비하는 예비 장치, 읽기 복제본은 추가 계산대**라는 비유를 기억하세요.

## 핵심 개념

| 구분 | 전통적인 Multi-AZ DB 인스턴스 | Read Replica |
|---|---|---|
| 주 목적 | 가용성·자동 장애 조치 | 읽기 확장·보고서 분리 |
| 복제 | 동기식 | 일반적으로 비동기식 |
| 읽기 | 대기 인스턴스에서 제공하지 않음 | 읽기 제공 |
| 범위 | 다른 AZ의 대기 인스턴스 | 엔진에 따라 같은 AZ·다른 AZ·다른 리전 |
| 승격 | 자동 장애 조치 | 독립 DB로 승격하는 별도 절차 |

**주의:** RDS **Multi-AZ DB 클러스터**는 읽기 가능한 대기 인스턴스가 있습니다. “Multi-AZ는 읽기 불가”는 전통적인 DB 인스턴스 배포에 한정됩니다. 복제본 수와 장애 조치 시간도 엔진·배포 유형에 따라 다릅니다.

### 백업·암호화·연결

- 자동 백업·PITR: 지원되는 보존 기간 안의 시점으로 복원. 대표 최대 보존 기간은 35일.
- 수동 스냅샷: 직접 삭제할 때까지 보관. 장기 보관에는 AWS Backup 정책도 검토.
- DB 삭제 시 자동 백업 보존 여부는 옵션에 따라 달라짐.
- 기존 비암호화 DB 암호화: **스냅샷 → 암호화 복사 → 복원 → 앱 연결 변경**.
- **RDS Proxy**: DB 연결 풀링. Lambda 동시 실행으로 연결이 급증하는 상황에 유용하지만 Lambda+RDS라는 이유만으로 항상 필수는 아님.
- 스토리지 자동 조정: 지원 엔진에서 저장 공간 증가에 대응.

### Aurora

MySQL·PostgreSQL 호환 엔진으로 컴퓨팅과 공유 스토리지를 분리합니다. 여러 AZ에 저장 복제본을 유지하며, 쓰기 엔드포인트와 리더 엔드포인트를 구분합니다. 리더 엔드포인트는 연결을 분산하며 쿼리 한 건마다 자동 분산하는 기능은 아닙니다.

| 기능 | 요구 상황 |
|---|---|
| Serverless v2 | 변동하는 부하에 컴퓨팅 용량 자동 조절 |
| Global Database | 다른 리전 읽기·DR. 일반적인 구조는 주 리전 쓰기 |
| Cloning | Copy-on-write로 테스트용 복제본 생성 |
| Backtrack | 지원되는 Aurora MySQL에서 DB 상태를 과거로 되돌림 |

Serverless v2의 0 ACU·자동 일시 중지 지원은 엔진 버전과 설정에 따라 확인합니다. Global Database의 낮은 RPO·RTO는 설계 목표이며 모든 장애에서 일정한 시간이나 데이터 무손실을 보장하지 않습니다. 원문의 용량·시간 수치는 고정 암기보다 최신 지원 조건을 우선하세요.

## 문제 유형

1. AZ 장애에 자동 대응 → 적절한 Multi-AZ 배포.
2. 다양한 보고서 쿼리가 운영 DB를 압박 → Read Replica 등으로 분리.
3. 같은 조회가 반복됨 → ElastiCache도 비교.
4. Lambda의 DB 연결 고갈 → RDS Proxy.
5. 빠르고 저렴한 테스트용 복제 → Aurora Cloning.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 가용성·자동 장애 조치 | Multi-AZ |
| 읽기 확장 | Read Replica |
| 연결 폭증 | RDS Proxy |
| 장기 백업 | 수동 스냅샷·AWS Backup |
| 글로벌 관계형 DB DR | Aurora Global Database |
| 지원 MySQL의 되돌리기 | Backtrack |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/09-RDS-Aurora.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
