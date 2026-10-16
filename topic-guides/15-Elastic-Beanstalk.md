# 15. Elastic Beanstalk

## 개념 이해

코드를 들고 가면 가구가 갖춰진 집을 준비해 주는 서비스입니다. 애플리케이션을 배포하면 EC2·Auto Scaling·로드 밸런서 같은 기반을 구성하고, 사용자는 기반 리소스를 확인하고 조정할 수 있습니다. Beanstalk 자체 추가 요금 대신 생성된 리소스 비용을 지불합니다.

## 핵심 개념

웹 서버 환경은 HTTP 요청을 처리하고, **Worker 환경**은 SQS 기반 백그라운드 작업을 처리합니다. `.ebextensions`는 소스 번들 안의 환경 구성 파일입니다. 지원 플랫폼에 따라 다른 확장 방식도 있으므로 플랫폼 조건을 확인합니다.

### 배포 방식

| 방식 | 동작 | 절충 |
|---|---|---|
| All at once | 전체를 한 번에 갱신 | 빠르지만 중단 가능 |
| Rolling | 기존 인스턴스를 배치별 갱신 | 추가 인스턴스 비용 감소, 배포 중 용량 감소 |
| Rolling with additional batch | 추가 배치 확보 후 순차 갱신 | 기존 용량 유지에 유리, 임시 비용 |
| Immutable | 새 인스턴스 집합을 만든 뒤 정상 여부 확인 | 실패 시 새 집합 폐기, 임시 비용 |
| Blue/Green | 별도 환경을 검증하고 CNAME 교환 | 환경 단위 전환, 두 환경 비용·DNS 캐시 고려 |

무중단 여부는 잔여 용량·상태 확인·애플리케이션 호환성에 달려 있습니다. “Rolling이면 무조건 무중단”, “CNAME 교환은 모든 클라이언트에 즉시 반영”이라고 단정하지 않습니다.

### DB 수명 주기

환경에 결합된 DB는 환경 종료 시 삭제 정책의 영향을 받습니다. 운영 데이터는 환경과 수명 주기를 분리한 RDS로 관리하고 삭제 방지·백업을 구성하는 방식을 검토합니다. 기존 DB 분리는 지원 절차 또는 스냅샷 복원 후 연결 변경으로 계획합니다. 비밀번호를 평문 환경 변수에 그대로 두는 대신 비밀 관리 연동을 검토하세요.

정밀한 IaC 정의는 CloudFormation, 컨테이너 오케스트레이션은 ECS·EKS, 짧은 이벤트 실행은 Lambda와 비교합니다.

## 문제 유형

1. 코드 배포는 간단히 하되 기반 리소스를 조정하고 싶음 → Beanstalk.
2. 개발 환경이며 잠깐 중단 허용 → All at once.
3. 추가 인스턴스를 줄이고 기존 용량 감소 허용 → Rolling.
4. 새 환경에서 검증 후 환경 단위 전환 → Blue/Green.
5. 환경 종료와 독립적으로 DB 보존 → DB 수명 주기 분리.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 빠른 웹앱 배포·기반 제어 | Beanstalk |
| 배치별 갱신 | Rolling |
| 추가 배치로 용량 유지 | Rolling with additional batch |
| 새 인스턴스 집합·쉬운 실패 폐기 | Immutable |
| 환경 CNAME 교환 | Blue/Green |
| SQS 백그라운드 처리 | Worker 환경 |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/15-Elastic-Beanstalk.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
