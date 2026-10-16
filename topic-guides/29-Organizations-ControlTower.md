# 29. Organizations와 Control Tower

## 개념 이해

Organizations는 여러 AWS 계정을 조직 트리로 관리합니다. 관리 계정 아래 Root → OU → 계정 구조를 만들고, 계정별 격리와 조직 정책을 조합합니다. Control Tower는 그 위에 표준 멀티 계정 환경을 구축·관리하는 자동화를 제공합니다.

## 핵심 개념

**통합 결제(Consolidated billing)**는 조직 비용을 모아 청구하고 지원되는 사용량 할인·RI·Savings Plans 혜택을 공유합니다. 공유 설정·범위에 따라 달라지며 모든 예약 용량이 계정 사이 이동한다는 뜻은 아닙니다.

**SCP는 권한 상한이며 권한을 부여하지 않습니다.** Root·OU·계정에 적용되는 계층을 모두 통과해야 하고, IAM·리소스 정책 등 필요한 권한도 있어야 합니다. 멤버 계정의 루트도 제한하지만 **관리 계정에는 적용되지 않으며 서비스 연결 역할 등 예외가 있습니다**.

대표 사례는 허용되지 않은 리전 작업 제한·CloudTrail 비활성화 제한입니다. 글로벌 서비스·예외 역할·복구 접근을 고려해 정책을 설계합니다. 관리 계정에는 일상 워크로드를 최소화하는 것이 좋습니다.

| 도구·기능 | 역할 |
|---|---|
| Control Tower landing zone | 표준 계정·감사·로그·접근 체계를 구성 |
| Account Factory | 표준화된 계정 생성·프로비저닝 |
| 예방적 제어 | 지원되는 SCP·기타 정책 메커니즘으로 작업 제한 |
| 탐지적 제어 | Config 등으로 비준수 탐지 |
| 사전 평가 제어 | 배포 전에 지원 리소스 구성 검사 |
| RAM | 지원 서브넷·TGW·Resolver 규칙 등을 계정 간 공유 |

원문의 예방=SCP, 탐지=Config는 핵심 입문 구분이지만 Control Tower의 모든 제어 유형을 설명하지는 않습니다. 최신 제어 메커니즘을 확인하세요.

## 문제 유형

1. 멀티 계정 표준 환경을 자동 구성 → Control Tower.
2. 팀별 새 계정을 표준 기준으로 발급 → Account Factory.
3. 멤버 계정 관리자도 특정 API 불가 → SCP 등 상위 제한 확인.
4. 관리 계정에 SCP가 적용되지 않음 → 설계된 예외.
5. 여러 계정이 같은 서브넷·TGW 사용 → RAM 공유 지원 확인.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 조직·OU·통합 결제 | Organizations |
| 조직 권한 상한 | SCP |
| 표준 멀티 계정 환경 | Control Tower |
| 표준 새 계정 | Account Factory |
| 실제 리소스 공유 | RAM |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/29-Organizations-ControlTower.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
