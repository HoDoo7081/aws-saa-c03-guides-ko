# 22. WAF, Shield와 보안 서비스

## 개념 이해

보안은 여러 역할로 나뉩니다. WAF는 HTTP 요청을 읽는 입구 경비, Shield는 DDoS 대응, Firewall Manager는 여러 계정의 정책 배포 관리자입니다. 탐지·취약점·민감정보 발견은 또 다른 기능입니다.

## 핵심 개념

| 서비스 | 문제의 핵심 |
|---|---|
| WAF | SQL Injection·XSS·HTTP 요청 필터링·속도 기반 규칙 |
| Shield Standard | 기본 DDoS 보호 |
| Shield Advanced | 추가 DDoS 기능·대응 지원·조건에 따른 비용 보호 |
| Firewall Manager | Organizations 전반 보안 정책 관리·적용 |
| Network Firewall | VPC 네트워크 흐름 검사·상태 기반 필터링 |
| GuardDuty | 비정상 활동·계정/워크로드 위협 탐지 |
| Macie | S3의 민감 데이터 발견 |
| Inspector | EC2·ECR 이미지·Lambda 등의 지원 취약점 검사 |
| Security Hub | 보안 결과 통합·준수 상태 등 관리 |
| Detective | 보안 조사·관계 분석 |
| Artifact | AWS 규정 준수 보고서 확인 |

### WAF와 DDoS

WAF의 IP set·Geo match·관리형 규칙·Rate-based rule을 구분합니다. Count 동작은 실제 차단 전에 일치 요청을 관찰하는 데 유용합니다. 평가 시간 창과 집계 키는 설정 가능하므로 “항상 IP당 5분”이라는 고정 규칙은 아닙니다.

CloudFront·ALB·지원 API Gateway REST API 등 **지원 리소스**에 연결합니다. NLB·EC2에 직접 연결하는 형태는 아닙니다. 적용 범위·리전·API 유형을 확인하세요.

Shield Advanced의 지원·비용 보호는 자격과 절차가 있으며 공격으로 생긴 모든 비용이 자동 환불되는 것은 아닙니다. L7 공격 대응에는 WAF 등과의 연동 구성이 중요합니다.

### 조직 관리와 탐지

Firewall Manager는 여러 계정·신규 리소스에 정책을 관리합니다. Organizations·위임 관리자·Config 등 정책 유형별 전제 조건을 확인합니다. Network Firewall은 라우팅을 통해 검사 경로에 트래픽을 보내야 하며 생성만으로 모든 패킷이 자동 검사되지 않습니다.

GuardDuty의 기본 로그 분석과 선택적 Runtime Monitoring 등 보호 계획은 수집 방식이 다르므로 “항상 에이전트가 전혀 없다”는 일반화는 피합니다. Inspector도 지원되는 에이전트·에이전트리스 검사 방식을 구분합니다.

## 문제 유형

1. ALB 웹앱의 SQL Injection 차단 → WAF.
2. 새 WAF 규칙을 영향 없이 평가 → Count.
3. 여러 계정의 WAF 정책 자동 적용 → Firewall Manager.
4. S3에 개인정보가 있는지 확인 → Macie.
5. 채굴·탈취 자격 증명 징후 → GuardDuty. 소프트웨어 CVE → Inspector.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| HTTP 공격 | WAF |
| DDoS 전문 지원 | Shield Advanced |
| 조직 단위 정책 | Firewall Manager |
| 민감정보 / CVE / 위협 행동 | Macie / Inspector / GuardDuty |
| 보안 결과 통합 | Security Hub |
| AWS SOC·ISO 보고서 | Artifact |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/22-WAF-Shield-SecurityServices.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
