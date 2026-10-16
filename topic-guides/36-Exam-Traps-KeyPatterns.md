# 36. 시험 함정과 핵심 패턴

## 개념 이해

서비스 이름보다 **할 수 있는 일·할 수 없는 일·적용 조건**을 구분해야 합니다. 원문의 단정적 암기 규칙을 그대로 외우지 않도록 다음 표는 조건을 보완했습니다.

## 핵심 개념 — 자주 혼동하는 구분

| 혼동 | 올바른 구분 |
|---|---|
| Multi-AZ의 대기는 모두 읽기 불가 | 전통적 DB 인스턴스 대기는 불가, Multi-AZ DB 클러스터는 읽기 가능한 대기 인스턴스 |
| apex에 CNAME | 지원 AWS 대상에는 Alias 검토 |
| SG의 Deny 규칙 | SG는 Allow만, NACL은 Deny 가능 |
| 프라이빗 S3는 반드시 NAT | 같은 VPC에는 Gateway Endpoint 비교 |
| 20분짜리 단일 Lambda | 일반 함수 호출은 최대 15분. ECS·Batch 또는 분할 |
| Windows SMB에 EFS | FSx Windows·ONTAP 등 프로토콜 지원 비교 |
| Peering은 전이적 | A-B·B-C만으로 A-C 연결되지 않음 |
| 기존 비암호화 EBS·RDS를 제자리 암호화 | 지원 스냅샷·암호화 복사·복원 절차 |
| FIPS Level 3이면 KMS 불가 | 현재 KMS도 Level 3 HSM, 전용성과 관리 요건으로 비교 |
| 관리 계정에도 SCP 적용 | 관리 계정은 SCP 대상 아님 |
| DynamoDB Streams 한 달 재생 | 기본 Streams는 24시간, 다른 보존 수단 필요 |
| ALB 자체의 고정 IP | NLB·Global Accelerator 조합 등 검토 |
| 기존 테이블에 LSI 추가 | LSI는 생성 시, 이후 다른 키 조회는 GSI 검토 |
| UDP를 CloudFront로 가속 | TCP·UDP의 Global Accelerator 검토 |
| Parameter Store가 비밀번호 자동 교체 | Secrets Manager의 교체 기능 비교 |
| 모든 NAT는 반드시 AZ별 여러 개 | Zonal과 Regional NAT 유형 구분 |
| SNS는 저장·재시도가 전혀 없음 | 전달 재시도·유형별 기능이 있음, 독립 소비에는 SNS→SQS |
| DX는 사설이므로 자동 암호화 | 암호화 계층 별도 검토 |
| API Gateway는 언제나 29초 고정 | API 유형·통합·할당량·스트리밍에 따라 다름 |
| DAX는 코드 변경 0 | DAX 클라이언트 등 최소 연동 변경 필요 |

### 대표 조합

| 요구 | 조합 예 |
|---|---|
| 서버리스 웹 | S3·CloudFront + API Gateway + Lambda + DynamoDB |
| 독립적인 여러 소비자 | SNS → 소비자별 SQS |
| 글로벌 관계형·NoSQL | Aurora Global / DynamoDB Global Tables |
| 데이터 레이크 분석 | S3 + Glue Catalog + Athena + BI 도구 |
| 앱 로그인·직접 AWS 접근 | User Pool + 필요 시 Identity Pool |
| 파일·DB 이전 | DataSync / DMS + 변환 평가 |
| DR | 복제·백업·복구 자동화·트래픽 전환을 함께 설계 |

어떤 조합도 이름만으로 “유실 없음·완전 무중단”이 되지 않습니다. 재시도·멱등성·권한·검증이 필요합니다.

## 문제 풀이 방법

1. 마지막 문장에서 최적화 기준을 확인합니다.
2. 전체 본문의 요구사항을 체크리스트로 만듭니다.
3. 기능·보존 시간·지연·프로토콜·가용성 조건으로 오답을 제거합니다.
4. 남은 답 중 비용·운영·성능 기준에 맞는 것을 선택합니다.
5. 복수 정답 수를 확인하고 모르는 문제는 표시 후 이동합니다.

130분에 65문항이면 평균 2분입니다. 비채점 문항은 표시되지 않습니다. 틀린 답에 추가 감점은 없으므로 빈칸을 남기지 않습니다. **720점은 단순 정답률 72%가 아니며 정확한 정답 개수 합격선은 공개되지 않습니다.** [공식 시험 안내](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html)

## 문제 유형

- 같은 기능을 하는 두 답 → 비용·운영 부담 같은 마지막 조건으로 비교.
- 서비스는 맞지만 한 조건을 위반 → 오답 후보.
- “항상·절대·즉시·무료” → 범위와 예외를 점검.
- 익숙한 키워드 등장 → 이름만 고르지 말고 전체 요구 확인.

## 빠른 복습표

| 확인 질문 | 목적 |
|---|---|
| 프로토콜·OS는? | 기능 불일치 제거 |
| AZ 장애인가 리전 장애인가? | HA와 DR 구분 |
| 복원 시간·보존 기간은? | 저장소·백업 선택 |
| 중단·중복·순서 허용은? | 컴퓨팅·메시징 선택 |
| 비용인가 운영 부담인가? | 최종 선택 |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/36-Exam-Traps-KeyPatterns.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
