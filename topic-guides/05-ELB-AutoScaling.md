# 05. ELB와 Auto Scaling

## 개념 이해

로드 밸런서는 식당에서 손님을 빈 테이블로 안내하는 직원이고, Auto Scaling은 수요에 따라 테이블 수를 늘리고 줄이는 관리자입니다. ELB는 요청을 분산하고, ASG는 인스턴스 수와 정상 상태를 유지합니다.

## 핵심 개념

| 유형 | 계층 | 용도 |
|---|---|---|
| ALB | L7 | HTTP·HTTPS·gRPC, 경로·호스트·헤더 라우팅 |
| NLB | L4 | TCP·UDP·TLS, AZ별 고정 IP, 높은 성능 |
| Gateway Load Balancer | L3 | 방화벽·IDS/IPS 등 네트워크 어플라이언스 삽입 |
| CLB | 기존 세대 | 레거시 요구·이전 환경 문맥에서 판단 |

- ALB: `/api/*`와 `/images/*`를 다른 대상 그룹으로 분리. EC2·IP·Lambda 대상 등 지원. 클라이언트 주소는 보통 `X-Forwarded-For`로 전달. WAF 연동 가능.
- NLB: 고정 IP·EIP, 비 HTTP 프로토콜, PrivateLink 엔드포인트 서비스에 사용. 원본 IP 보존 여부는 대상 유형·프로토콜·설정에 따라 다름.
- GWLB: GENEVE UDP 6081로 보안 어플라이언스에 트래픽 전달.
- ALB 자체에는 고정 IP가 없음. 필요하면 NLB→ALB 또는 Global Accelerator 조합 검토.
- 고정 세션(Sticky session)은 로컬 세션 유지에 도움. 장기적으로는 ElastiCache·DynamoDB 등에 세션 외부화 고려.
- ECS 동적 포트 매핑은 여러 컨테이너를 하나의 호스트에서 서비스할 때 유용.

### 공통 기능

**Cross-zone**은 AZ 간 대상으로 부하를 분산합니다. 기본값과 요금은 LB·대상 그룹 유형에 따라 확인하세요. **등록 취소 지연(Deregistration delay)**은 축소 시 진행 중 요청을 마무리하게 합니다. **SNI**는 하나의 TLS 리스너에 여러 도메인 인증서를 사용하게 합니다. **TLS 종료**는 LB가 암호화를 처리하는 방식입니다.

### ASG

시작 템플릿(Launch Template)에 AMI·인스턴스 유형·보안 그룹·User data를 정의하고 `min / desired / max`로 용량을 관리합니다. ASG는 desired를 유지하기 위해 실패한 인스턴스를 교체합니다.

| 확장 정책 | 문제의 단서 |
|---|---|
| Target Tracking | 평균 CPU 40%처럼 목표 지표 유지 |
| Step Scaling | 구간별로 다른 수량 조절 |
| Scheduled | 매주 월요일 9시 등 정해진 시각 |
| Predictive | 반복 수요를 예측해 사전 확장 |

인스턴스 워밍업·상태 확인 유예 기간·쿨다운은 서로 다른 개념입니다. 일괄적으로 모두 300초라고 외우지 말고 새 인스턴스가 지표와 교체 판단에 반영되는 시점을 구분하세요.

**핵심 함정:** EC2 상태는 정상이지만 앱은 죽을 수 있습니다. ELB가 비정상으로 판단한 인스턴스를 ASG가 교체하게 하려면 **ASG의 ELB 상태 확인을 활성화**해야 합니다.

SQS 워커는 CPU만 보지 말고 **인스턴스당 대기 메시지 수**를 기준으로 확장을 검토합니다. 축소 시 기본 종료 정책은 AZ 균형과 시작 템플릿 등 여러 조건을 고려합니다.

## 문제 유형

1. URL 경로별 마이크로서비스 분기 → ALB.
2. UDP 게임 트래픽·고정 주소 → NLB.
3. 타사 보안 장비를 통과하도록 구성 → GWLB.
4. 앱이 응답하지 않는데 ASG가 교체하지 않음 → ELB 상태 확인 활성화.
5. 축소 중 요청 오류 → 등록 취소 지연·종료 처리 확인.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 경로·호스트·WAF | ALB |
| UDP·고정 IP | NLB |
| 보안 어플라이언스 | GWLB |
| 여러 HTTPS 도메인 인증서 | SNI |
| 정해진 시각 확장 | Scheduled |
| 목표 지표 유지 | Target Tracking |
| 앱 수준 장애 교체 | ASG ELB 상태 확인 |
| 대기 작업 기반 확장 | SQS backlog per instance |

> 원문의 “CLB는 항상 오답”은 절대 규칙이 아닙니다. 선택지는 서비스 기능과 요구사항으로 판단하세요.

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/05-ELB-AutoScaling.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
