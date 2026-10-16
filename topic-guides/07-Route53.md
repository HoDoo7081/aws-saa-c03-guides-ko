# 07. Route 53 — DNS와 라우팅 정책

## 개념 이해

DNS는 이름을 주소로 바꾸는 전화번호부입니다. Route 53은 주소를 알려준 뒤 실제 앱 트래픽 경로에서 빠집니다. 모든 요청을 직접 전달하는 ELB와 다릅니다. DNS 답은 TTL 동안 캐시될 수 있어 장애 조치에도 캐시 지연을 고려해야 합니다.

## 핵심 개념

| 레코드 | 의미 |
|---|---|
| A / AAAA | 이름 → IPv4 / IPv6 |
| CNAME | 이름 → 다른 이름. 존 최상위(apex)에는 사용할 수 없음 |
| Alias | 지원 AWS 리소스·일부 같은 존 레코드를 가리키는 Route 53 기능. apex에 사용 가능 |

`example.com`을 ALB로 연결하려면 Alias를 검토합니다. ALB의 현재 IP를 일반 A 레코드에 고정하지 않습니다. Alias의 요금 특례는 대상에 따라 확인하세요.

퍼블릭 호스팅 영역은 인터넷에서, 프라이빗 호스팅 영역은 연결된 VPC와 적절히 구성된 하이브리드 DNS 경로에서 조회합니다.

| 라우팅 정책 | 선택 단서 |
|---|---|
| Simple | 단순 DNS 응답. 하나의 레코드에 여러 값도 가능 |
| Weighted | 90:10 등 가중치, 카나리·A/B 테스트 |
| Latency | 사용자에게 지연이 낮은 AWS 리전 |
| Failover | 기본·보조, Active-Passive |
| Geolocation | 국가·대륙 등 사용자 위치에 따른 응답 |
| Geoproximity | 리소스와 사용자 거리, bias로 담당 영역 조절 |
| Multivalue Answer | 최대 8개 정상 레코드 응답. ELB 대체품은 아님 |

**속도**가 기준이면 Latency, **지역별 콘텐츠 규칙**이면 Geolocation, **bias**가 단서면 Geoproximity입니다. “트래픽 비율을 늘린다”만으로 Geoproximity라고 단정하지 말고 고정 가중치인지 구분하세요. Geolocation에는 미일치 지역을 처리할 기본 레코드를 고려합니다. DNS 위치 판별만으로 엄격한 법적 데이터 경계를 보장하지는 못합니다.

상태 확인기는 퍼블릭 엔드포인트를 검사합니다. 프라이빗 리소스는 CloudWatch 지표·경보에 기반한 상태 확인 등으로 구성합니다. 여러 상태 확인을 결합하는 계산된 상태 확인도 가능합니다.

## 문제 유형

1. apex 도메인을 ALB로 연결 → Alias.
2. 신버전에 요청 일부만 전달 → Weighted. 실제 요청 비율은 DNS 캐시 등으로 정확히 일치하지 않을 수 있음.
3. 기본 사이트 장애 시 DR 사이트로 전환 → Failover + 상태 확인.
4. 전 세계 사용자 지연 감소 → Latency.
5. DNS 캐시 지연 없이 고정 진입 IP로 리전 장애 대응 → Global Accelerator 검토.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 최상위 도메인 → AWS 리소스 | Alias |
| 비율·카나리 | Weighted |
| 최저 지연 | Latency |
| 국가별 콘텐츠 | Geolocation |
| bias | Geoproximity |
| 기본·보조 사이트 | Failover |
| 사설 이름 해석 | Private hosted zone |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/07-Route53.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
