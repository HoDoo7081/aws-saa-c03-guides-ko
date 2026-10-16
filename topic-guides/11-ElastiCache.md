# 11. ElastiCache — 인메모리 캐시

## 개념 이해

도서관 직원이 반복 질문의 답을 메모해 두면 매번 서고를 뒤질 필요가 없습니다. 캐시는 자주 읽는 결과를 메모리에 저장하여 지연과 원본 DB 부하를 줄입니다. 애플리케이션에서 조회·갱신·무효화 로직을 설계해야 합니다.

## 핵심 개념

원문은 Redis와 Memcached를 비교하지만 현재 ElastiCache에는 **Valkey**도 있습니다.

| 요구 | 검토할 엔진·기능 |
|---|---|
| 복제·장애 조치·백업 | Valkey·Redis OSS의 지원 구성 |
| 실시간 순위표 | Sorted Sets |
| Pub/Sub | Valkey·Redis OSS |
| 단순하고 분산된 임시 캐시 | Memcached |

복제가 있다고 모든 장애에서 데이터 무손실이 보장되지는 않습니다. 노드 기반·Serverless·엔진별 지원 기능을 구분하세요. “Redis는 언제나 단일 스레드”, “Memcached는 어떤 구성에서도 백업 불가” 같은 절대 규칙은 피합니다.

### 세션 외부화

ASG 인스턴스의 메모리에만 세션을 저장하면 축소 시 사용자가 로그아웃될 수 있습니다. 세션을 ElastiCache나 DynamoDB로 옮겨 웹 서버를 상태 비저장(Stateless)으로 만듭니다. ALB 고정 세션은 세션 데이터의 영속성을 대신하지 않습니다.

### 캐싱 전략

| 전략 | 동작 | 고려점 |
|---|---|---|
| Lazy loading / Cache-aside | 캐시 미스 시 DB 조회 후 채움 | 필요한 데이터만 캐시, 미스 지연·오래된 데이터 가능 |
| Write-through | 쓰기 경로에서 캐시도 갱신 | 쓰기 비용과 원자성·실패 처리 필요 |
| TTL | 일정 시간 뒤 만료 | 오래된 데이터의 최대 존속 시간 조절, 엄격한 최신성 보장은 아님 |

**Write-through만 도입하면 데이터가 절대로 낡지 않는다는 설명은 과도합니다.** DB·캐시 이중 쓰기의 실패, 동시성, 다른 쓰기 경로를 고려해야 합니다.

### 읽기 부하 감소 도구 비교

반복되는 동일 조회는 캐시, 매번 다른 복잡한 보고서는 읽기 복제본·분석 시스템, DynamoDB 전용 읽기 캐시는 DAX를 검토합니다. DAX도 클라이언트 연동 변경이 필요하므로 “코드 변경 0”은 정확하지 않습니다.

## 문제 유형

1. 같은 인기 상품 조회가 DB를 압박 → ElastiCache.
2. 게임 점수 순위표 → Sorted Sets.
3. ASG 축소마다 사용자 로그아웃 → 세션 외부화.
4. 매번 다른 분석 쿼리 → Read Replica·분석 DB 검토.
5. 오래된 가격이 허용되지 않음 → 쓰기·무효화·일관성 설계를 검토, 단순 TTL만으로 해결하지 않기.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 같은 조회 반복 | 캐시 |
| 순위·점수 | Sorted Sets |
| 서버 교체 시 세션 소실 | 세션 외부화 |
| 미스 시 적재 | Cache-aside |
| 쓰기와 함께 갱신 | Write-through |
| DynamoDB 전용 캐시 | DAX |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/11-ElastiCache.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
