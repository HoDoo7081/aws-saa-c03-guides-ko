# 번역 범위와 정정 안내

## 한국어판의 성격

원작은 [RonitSachdev/aws-saa-c03-guides](https://github.com/RonitSachdev/aws-saa-c03-guides), 기준 커밋은 `a71e466f02702fe01a6fe414c0b49cb8a673d205`입니다. 아래 37개 주제를 모두 한국어 학습 문서로 작성했지만 **문장별 완역이 아닌 요약·재구성**입니다. 원문의 일부 반복 예제·비유·고정 수치는 생략했고, 단정적 암기 규칙에는 조건을 붙였습니다. 사례는 학습용이며 실제 시험 문항이라는 뜻이 아닙니다.

주요 정정 근거 확인일: **2026-10-16 (한국 시간)**. 아래는 선택적으로 확인한 항목이며 전체 기능·요금·리전·시험 범위에 대한 전수 검증은 아닙니다. 서비스 제공 상태와 시험 가이드의 평가 범위는 별개이므로 종료된 서비스가 예전 자료에 보인다고 현재 신규 구축을 추천하지는 않습니다.

## 공식 문서와 대조한 주요 항목

| 주제 | 원문에서 보완한 내용 | 공식 근거 |
|---|---|---|
| S3 객체 크기 | 최대 5TB라는 설명은 오래됨. 2025년 12월부터 50TB로 확대 | [AWS 발표](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-s3-maximum-object-size-50-tb/) |
| S3 Object Lock | 생성 시에만 활성화 가능하다는 설명 수정. 기존 버킷에도 활성화 가능 | [Object Lock 구성](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-configure.html) |
| S3 Select | 신규 고객에게 제공되지 않음 | [S3 Select 안내](https://docs.aws.amazon.com/AmazonS3/latest/userguide/selecting-content-from-objects.html) |
| 퍼블릭 IPv4 | 유휴 EIP만 과금된다는 설명 수정. 사용 중인 주소도 과금 대상 | [VPC IP 주소](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html) |
| EC2 배치 그룹 | Cluster가 같은 랙을 보장한다는 설명을 한 AZ 안의 근접 배치로 수정 | [배치 전략](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-strategies.html) |
| EBS gp3 | 16,000 IOPS를 일반 상한으로 둔 결정 트리 수정. 현재 일반 리전 문서에는 최대 80,000 IOPS·2,000MiB/s, 별도 용량·인스턴스·환경 조건이 있음 | [General Purpose SSD](https://docs.aws.amazon.com/ebs/latest/userguide/general-purpose.html) |
| NAT | Zonal NAT의 AZ별 구성과 Regional NAT의 자동 다중 AZ 확장을 구분 | [Regional NAT](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateways-regional.html) |
| RDS Multi-AZ | DB 인스턴스 배포의 읽기 불가 대기와 DB 클러스터의 읽기 가능한 대기를 구분 | [Multi-AZ](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |
| DAX | 코드 변경이 전혀 없다는 표현 수정. DAX 클라이언트를 사용하는 최소 변경 필요 | [DAX 클라이언트](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.client.html) |
| ElastiCache | Redis·Memcached만 제시한 목록에 Valkey 보완 | [ElastiCache 개요](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/WhatIs.html) |
| API Gateway | 29초를 절대로 늘릴 수 없다는 설명 수정. Regional·Private REST 통합은 상향 가능 | [타임아웃 확대 발표](https://aws.amazon.com/about-aws/whats-new/2024/06/amazon-api-gateway-integration-timeout-limit-29-seconds/) |
| API Gateway 스트리밍 | REST API 응답 스트리밍은 버퍼링 응답과 다른 제한이 있음 | [응답 스트리밍](https://docs.aws.amazon.com/apigateway/latest/developerguide/response-transfer-mode.html) |
| SQS | 256KiB 한도는 오래됨. 2025년 8월부터 최대 본문 1MiB | [AWS 발표](https://aws.amazon.com/about-aws/whats-new/2025/08/amazon-sqs-max-payload-size-1mib/) |
| KMS | KMS는 FIPS Level 2라는 설명 수정. 현재 FIPS 140-3 Security Level 3 검증 HSM 사용 | [KMS 데이터 보호](https://docs.aws.amazon.com/kms/latest/developerguide/data-protection.html) |
| FSx와 AD | AD Connector를 FSx의 디렉터리로 선택하는 예시 수정. Managed AD·지원 자체 관리 AD와 통합 | [FSx AD 연동](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/aws-ad-integration-fsxW.html) |
| ACM | 공개 인증서를 내보낼 수 없다는 설명 수정. 내보내기 가능한 공개 인증서 제공 | [AWS 발표](https://aws.amazon.com/blogs/aws/aws-certificate-manager-introduces-exportable-public-ssl-tls-certificates-to-use-anywhere/) |
| Trusted Advisor | 다섯 범주만 있다는 설명에 운영 우수성 보완. 전체 검사 접근은 현재 지원 플랜 확인 | [Trusted Advisor 시작](https://docs.aws.amazon.com/awssupport/latest/user/get-started-with-aws-trusted-advisor.html) |
| 비용 배분 태그 | 소급 불가라는 설명 수정. 조건에 따라 최대 12개월 backfill 가능 | [태그 backfill](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-allocation-backfill.html) |

## 서비스 제공 상태

| 원문 항목 | 한국어판에서 구분한 상태 | 공식 근거 |
|---|---|---|
| QLDB | 2025-07-31 지원 종료, 신규 설계 대상으로 그대로 추천하지 않음 | [AWS 종료·이전 안내](https://aws.amazon.com/jp/blogs/news/migration-from-amazon-qldb/) |
| Timestream for LiveAnalytics | 2025-06-20부터 신규 고객 접근 중단. InfluxDB 제품과 구분 | [제공 상태 변경](https://docs.aws.amazon.com/timestream/latest/developerguide/AmazonTimestreamForLiveAnalytics-availability-change.html) |
| Forecast | 신규 고객 제공 중단 | [공식 문서 이력](https://docs.aws.amazon.com/forecast/latest/dg/doc-history.html) |
| FSx File Gateway | 신규 고객 제공 중단 | [공식 사용자 가이드](https://docs.aws.amazon.com/filegateway/latest/filefsxw/storagegateway-fsxfile-ug.pdf) |
| Snowball Edge | 2025-11-07부터 기존 고객만 이용 | [공식 문서 이력](https://docs.aws.amazon.com/snowball/latest/developer-guide/doc-history.html) |
| Snowcone·이전 Snow 장비 | 중단·세대 변경이 있으므로 원문 용량 표를 현재 카탈로그로 사용하지 않음 | [AWS 장비 변경 안내](https://aws.amazon.com/blogs/storage/aws-snow-device-updates/) |

## 단정적 표현의 편집 원칙

- “이 단어가 나오면 무조건 이 서비스”를 대표 후보와 검토 조건으로 변경했습니다.
- 백업·복제·재시도·캐시를 “유실 없음·중복 없음·절대 최신·완전 무중단”으로 표현하지 않았습니다.
- 원문 IAM JSON의 버킷 작업과 객체 작업 리소스 구분을 설명으로 보완했습니다.
- 전통적 배포의 사실을 모든 제품 유형에 일반화하지 않았습니다.
- RPO·RTO·복원 시간은 목표·통상값·실측값·보장값을 구분합니다.
- 요금·지원 플랜·할당량은 리전·세대·계정·지원 옵션에 따라 다르므로 불필요한 고정 암기를 줄였습니다.
- 정확한 정답 개수 합격선을 제시하지 않습니다. [AWS 시험 가이드](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html)의 환산 점수·비채점 설명을 따릅니다.

[한국어 학습 목차](topic-guides/00-README.md) · [저장소 안내](README.md)

---
원작 RonitSachdev, [원본 저장소](https://github.com/RonitSachdev/aws-saa-c03-guides). 한국어 번역·요약·재구성 및 보완: HoDoo7081(AI 편집 도움). [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
