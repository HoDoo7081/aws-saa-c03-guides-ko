# 13. Lambda — 이벤트 기반 컴퓨팅

## 개념 이해

이벤트가 생길 때만 출근해 일을 처리하는 계약직에 비유할 수 있습니다. 함수와 트리거를 정의하면 AWS가 실행 환경을 관리합니다. 서버가 없어지는 것이 아니라 서버 운영을 직접 하지 않는 것입니다.

## 핵심 개념

일반적인 Lambda 함수 실행은 **호출당 최대 15분**입니다. 메모리를 늘리면 CPU 할당도 증가합니다. `/tmp` 임시 저장소와 메모리는 별도 설정이며, 배포 패키지·컨테이너 이미지 크기 한도와 리전별 동시성 할당량을 확인하세요. Provisioned Concurrency 같은 기능은 유휴 상태에도 비용이 생길 수 있습니다.

| 호출 방식 | 예 | 실패 처리 |
|---|---|---|
| 동기식 | API Gateway | 호출자에게 오류 반환, 호출자 재시도 설계 |
| 비동기식 | S3·SNS 등 | Lambda 큐·재시도·이벤트 수명, DLQ·대상 설정 |
| Event Source Mapping | SQS·Kinesis·DynamoDB Streams | 소스를 폴링해 배치 처리. 큐와 스트림의 재시도 방식은 다름 |

비동기 함수 오류는 기본 추가 재시도 2회지만 throttling·시스템 오류·이벤트 만료 동작은 구분해야 합니다. SQS는 메시지를 삭제하지 못하면 가시성 제한 시간 뒤 다시 나타납니다. 스트림은 체크포인트·배치 실패 처리 설정이 중요합니다.

### 두 동시성 설정

- **Provisioned Concurrency**: 실행 환경을 미리 준비해 콜드 스타트 지연 완화. 준비한 용량 초과 호출에는 여전히 초기화 지연 가능.
- **Reserved Concurrency**: 함수에 동시성을 예약하면서 상한도 설정, 하위 시스템 과부하 제한.

VPC의 프라이빗 RDS에 연결하려면 적절한 서브넷·SG를 구성합니다. VPC 연결 함수의 인터넷 접근에는 NAT 등 경로가 필요하며, 퍼블릭 서브넷에 넣는 것만으로 퍼블릭 IP를 받지 않습니다. DB 연결 폭증에는 RDS Proxy를 검토합니다.

실행 역할은 AWS 접근 권한, Layers는 공통 의존성, 환경 변수는 설정에 사용합니다. 비밀은 Secrets Manager 등 적절한 저장소를 검토합니다. 정기 실행은 EventBridge Scheduler를 사용할 수 있습니다.

### 엣지 코드

| 기능 | 대표 용도 |
|---|---|
| CloudFront Functions | 가벼운 헤더·URL 조작, viewer 이벤트, 임의 외부 네트워크 호출 불가 |
| Lambda@Edge | 지원 런타임으로 더 복잡한 처리, viewer·origin 이벤트, 지원 조건 내 네트워크 호출 |

JWT 검증이라는 단어만으로 Lambda@Edge를 고르지 말고 외부 조회·암호화 라이브러리·실행 제약을 봅니다.

## 문제 유형

1. 90분짜리 작업 → 단일 Lambda 호출 대신 Batch·ECS 등. 작업 분할 가능하면 Step Functions 검토.
2. CPU 중심 함수가 느림 → 메모리 증설과 성능·비용 측정.
3. 유휴 후 응답 지연 → Provisioned Concurrency 검토.
4. 하위 DB 과부하 → Reserved Concurrency·RDS Proxy 검토.
5. 실패한 비동기 이벤트 유실 방지 → DLQ·실패 대상과 재처리 설계.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 일반 함수 15분 초과 | Batch·ECS 또는 작업 분할 |
| 콜드 스타트 | Provisioned Concurrency |
| 동시 실행 상한 | Reserved Concurrency |
| SQS·스트림 소비 | Event Source Mapping |
| AWS 호출 AccessDenied | 실행 역할 |
| 엣지 URL·헤더 조작 | CloudFront Functions |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/13-Lambda.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
