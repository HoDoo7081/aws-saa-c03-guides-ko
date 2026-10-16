# 12. 기타 데이터베이스와 분석 서비스

## 개념 이해

전문 도구는 용도에 맞게 고릅니다. 서비스 이름을 암기하기 전에 **트랜잭션·분석·관계·검색·시계열** 중 무엇이 핵심인지 판단하세요.

OLTP는 주문·주소 변경 같은 작고 빈번한 트랜잭션, OLAP는 수년 매출 집계 같은 대규모 분석입니다. 운영 DB에 무거운 분석을 집중시키면 앱 성능이 저하될 수 있습니다.

## 핵심 개념

| 서비스 | 주요 용도 |
|---|---|
| Redshift | 데이터 웨어하우스·BI·복잡한 분석, 열 기반 처리 |
| Redshift Spectrum | Redshift에서 S3 외부 데이터 조회 |
| Athena | S3 데이터에 서버리스 SQL, 임시·탐색 쿼리 |
| Neptune | 그래프 관계, 소셜 연결·사기 관계망·지식 그래프 |
| DocumentDB | MongoDB 호환 API를 제공하는 문서 DB. 완전 동일 동작은 아니므로 호환성 평가 |
| Keyspaces | Cassandra 호환 관리형 DB |
| OpenSearch | 전문·유사 검색, 로그 분석·대시보드 |
| Timestream 계열 | 시계열 요구. 제품별 신규 고객 제공 여부 확인 |
| Managed Blockchain | 지원 블록체인 네트워크 접근·구성 등 |
| QLDB | 원문에 있는 검증 가능한 원장 개념. 종료된 서비스이므로 신규 구축 대상으로 선택하지 않기 |

### Athena와 Redshift

S3에 있는 파일을 가끔 SQL로 조회하면 Athena, 반복적인 BI·웨어하우스 분석이면 Redshift를 비교합니다. 기존 Redshift에서 S3를 직접 읽으려면 Spectrum을 검토합니다. 실제 선택은 데이터 구조·쿼리 빈도·성능·운영 요구를 함께 봅니다.

Athena의 대표 최적화는 **Parquet·ORC 같은 열 기반 형식 + 압축 + 파티셔닝**입니다. 필요한 열과 파티션만 읽어 스캔량을 줄입니다. 요금은 리전·과금 방식별로 달라지므로 원문의 고정 달러 금액을 보편 규칙으로 외우지 않습니다.

DynamoDB는 범용 전문 검색 엔진이 아닙니다. 변경 피드를 OpenSearch로 전달하는 통합을 검토하며, 원문의 Streams → Lambda → OpenSearch는 가능한 패턴 중 하나입니다.

## 문제 유형

1. S3의 ALB 로그를 서버 없이 SQL로 분석 → Athena.
2. 대규모 반복 BI·복잡한 집계 → Redshift.
3. 친구의 친구·사기 거래 관계망 → Neptune.
4. 상품명의 오타를 허용하는 검색 → OpenSearch.
5. MongoDB·Cassandra 앱 이전 → DocumentDB·Keyspaces의 호환성과 제한 확인.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| OLAP·웨어하우스 | Redshift |
| SQL on S3 | Athena |
| S3를 Redshift에서 조회 | Spectrum |
| 관계 그래프 | Neptune |
| 전문·유사 검색 | OpenSearch |
| Cassandra 호환 | Keyspaces |
| MongoDB 호환 | DocumentDB |

> QLDB·Timestream 등 원문의 일부 항목은 서비스 수명 주기를 확인해야 합니다. [정정 안내](../TRANSLATION-NOTES.md)에 공식 문서를 연결했습니다.

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/12-Other-Databases.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
