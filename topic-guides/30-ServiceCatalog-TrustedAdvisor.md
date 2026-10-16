# 30. Service Catalog와 Trusted Advisor

## 개념 이해

Service Catalog는 승인된 인프라 상품을 고르는 사내 자판기입니다. Trusted Advisor는 AWS 환경을 점검하고 개선을 제안하는 상담가입니다. 하나는 **표준 제품 배포**, 다른 하나는 **모범 사례 권장사항**입니다.

## 핵심 개념

### Service Catalog

관리자는 제품(Product)을 포트폴리오(Portfolio)에 묶고 사용자·계정에 접근을 부여합니다. CloudFormation 기반 제품 등 지원 제품 유형을 제공하며, Launch constraint의 IAM 역할로 승인된 배포 범위를 제어할 수 있습니다. 사용자의 권한이 전혀 필요 없다는 뜻은 아니고 카탈로그 접근·실행 권한과 서비스 역할을 적절히 구성합니다.

### Trusted Advisor

| 범주 | 점검 예 |
|---|---|
| 비용 최적화 | 미사용·저사용 리소스 |
| 성능 | 처리 용량·구성 개선 |
| 보안 | 루트 MFA·공개 접근 |
| 내결함성·복원력 | 백업·가용성 |
| 서비스 한도 | 할당량 접근 |
| 운영 우수성 | 환경 운영 개선 |

원문은 다섯 범주만 열거하지만 **운영 우수성 범주도 있습니다**. 전체 검사·API·조직 보기는 지원 플랜과 기능에 따라 다르며 플랜 명칭도 변경될 수 있으므로 현재 지원표를 확인합니다.

Trusted Advisor는 권장사항을 제공하는 것이 핵심입니다. Config는 내 규칙 준수, Inspector는 취약점, Compute Optimizer는 지원 리소스 크기 권장이라는 차이를 기억합니다.

## 문제 유형

1. 팀이 승인된 인프라 구성만 셀프서비스 배포 → Service Catalog.
2. 표준 상품을 부서별로 묶어 제공 → Portfolio.
3. 유휴 리소스·한도 접근·기본 모범 사례 점검 → Trusted Advisor.
4. 일부 검사만 보임 → 지원 플랜·권한·기능 범위 확인.
5. 자체 규정의 구성 준수 확인 → Config와 비교.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 승인 상품 셀프서비스 | Service Catalog |
| 상품 모음 | Portfolio |
| 배포 역할 제한 | Launch constraint |
| 모범 사례 권장 | Trusted Advisor |
| 전체 검사 접근 범위 | 현재 지원 플랜 확인 |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/30-ServiceCatalog-TrustedAdvisor.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
