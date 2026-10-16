# 14. ECS, EKS와 Fargate

## 개념 이해

컨테이너는 앱과 실행에 필요한 의존성을 표준 상자에 담는 방식입니다. ECS·EKS는 상자를 배치하고 유지하는 오케스트레이터이고, Fargate는 서버 운영을 맡기는 컴퓨팅 방식입니다. **Fargate를 ECS·EKS와 나란한 세 번째 오케스트레이터로 생각하지 마세요.**

## 핵심 개념

| ECS 용어 | 의미 |
|---|---|
| Task Definition | 이미지·CPU·메모리·포트·역할을 정의하는 설계도 |
| Task | 설계도로 실행한 작업 단위 |
| Service | 원하는 태스크 수를 유지·교체하고 LB와 연결 |
| Cluster | 작업·용량을 묶는 논리적 그룹 |

EC2 기반은 인스턴스 유형·OS·GPU 등 제어가 필요할 때, Fargate는 컨테이너를 실행하되 서버 운영을 줄일 때 검토합니다. 기존 Kubernetes 도구·매니페스트·운영 체계를 유지하려면 EKS가 자연스러운 후보입니다. EKS도 AWS 고유 통합을 쓰면 완전한 이식성이 자동 보장되지는 않습니다.

### 두 IAM 역할

| 역할 | 누구를 위한 것인가 | 오류 단서 |
|---|---|---|
| Task Execution Role | ECS 에이전트·실행 기반 | 이미지 가져오기·로그 전송·비밀 주입 권한 |
| Task Role | 컨테이너 안의 앱 코드 | S3·DynamoDB 등 앱 호출의 AccessDenied |

이미지 다운로드 실패는 IAM 외에도 네트워크·엔드포인트·이미지 경로를 확인해야 합니다. ECR은 이미지 저장소이며 이미지 스캔으로 취약점을 점검할 수 있습니다.

`awsvpc` 모드는 **태스크별 ENI와 보안 그룹**을 제공합니다. 한 태스크 안의 컨테이너는 네트워크를 공유하므로 컨테이너마다 별개 ENI라는 뜻은 아닙니다. ECS Fargate는 이 모드를 사용합니다. EC2 기반 bridge 모드에서는 ALB 동적 호스트 포트 매핑을 활용할 수 있습니다.

ECS Service Auto Scaling은 CPU·메모리·사용자 지표 등에 따라 태스크 수를 조정합니다. 중단 가능한 ECS 작업에는 Fargate Spot 등 지원 용량 옵션을 검토합니다. ECS용 기능이 EKS Fargate에도 모두 동일하게 적용된다고 가정하지 마세요.

## 문제 유형

1. 서버 관리 없이 컨테이너 실행 → Fargate.
2. 기존 Kubernetes 환경 이전 → EKS.
3. GPU가 필요한 컨테이너 → GPU 지원 EC2 컴퓨팅 검토.
4. 앱의 DynamoDB 호출 거부 → Task Role.
5. 태스크별 보안 그룹 → awsvpc.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 컨테이너 + 서버 운영 최소화 | Fargate |
| Kubernetes | EKS |
| AWS 기본 컨테이너 오케스트레이션 | ECS |
| 실행 기반 권한 / 앱 권한 | Execution Role / Task Role |
| 컨테이너 이미지·스캔 | ECR |
| 태스크 수 유지 | ECS Service |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/14-ECS-EKS-Fargate.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
