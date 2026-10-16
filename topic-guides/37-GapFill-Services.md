# 37. 놓치기 쉬운 서비스 보충

## 개념 이해

이 장은 익숙하지 않은 서비스의 용도를 빠르게 확인하는 지도입니다. 원문의 “각 서비스는 1~2문항 출제”는 보장된 출제 정보가 아닙니다. 서비스 이름과 대표 기능을 연결하되 요구 조건과 제공 상태를 확인하세요.

## 핵심 개념

### 이전·메시징

| 서비스 | 역할 |
|---|---|
| Amazon MQ | ActiveMQ·RabbitMQ 기반 앱과 지원 표준 프로토콜 |
| Application Migration Service(MGN) | 지원 서버를 블록 수준으로 복제해 AWS로 Rehost |
| Elastic Disaster Recovery(DRS) | 지속 복제·복구 훈련·재해 시 복구 |
| DMS | 지원 DB 이전·변경 데이터 복제 |
| SCT·DMS Schema Conversion | 이기종 DB 스키마·코드 변환 평가와 지원 변환 |

엔진이 다르면 자동 변환으로 전부 해결되지 않을 수 있습니다. 저장 프로시저·타입·앱 SQL의 수동 수정 범위를 평가합니다. MQ도 신규 앱에 금지된 서비스는 아니며 프로토콜·브로커 기능·운영 요구에 따라 판단합니다.

7R: **Rehost**(그대로 이전), **Replatform**(일부 플랫폼 변경), **Repurchase**(SaaS 등 재구매), **Refactor/Re-architect**(구조 재설계), **Relocate**(환경 단위 이동), **Retain**(유지), **Retire**(폐기).

### 인증서·DNS·엣지

| 기능 | 핵심 구분 |
|---|---|
| ACM | 인증서 발급·관리·지원 갱신 |
| Resolver Inbound Endpoint | 온프레미스 → VPC 이름 해석 |
| Resolver Outbound Endpoint | VPC → 온프레미스 DNS 전달 |
| Egress-only IGW | IPv6의 외부 시작 연결 |
| Outposts | 고객 현장에 AWS 인프라 배치 |
| Local Zones | 특정 도시 가까이에 AWS 지원 리소스 배치 |
| Wavelength | 지원 통신사 네트워크의 엣지 컴퓨팅 |

CloudFront용 ACM 인증서는 us-east-1입니다. **ACM 공개 인증서는 절대 내보낼 수 없다는 원문은 오래됐습니다.** 내보내기 가능한 공개 인증서가 제공되며 발급 옵션·비용·개인 키·외부 배포 갱신 절차를 구분합니다. IPv6에도 NAT64 등의 변환 시나리오가 있으므로 “IPv6는 모두 NAT 불가”라고 외우지 마세요.

### 분석·데이터 통합

| 서비스 | 단서 |
|---|---|
| Glue | ETL·Data Catalog·Crawler |
| EMR | Hadoop·Spark 등 빅데이터 프레임워크 |
| MSK | 관리형 Apache Kafka |
| Athena | S3의 SQL 분석 |
| QuickSight 및 관련 BI 제공 기능 | 비즈니스 시각화·대시보드 |
| Lake Formation | 데이터 레이크 권한·지원 세부 접근 제어 |
| AppFlow | 지원 SaaS와 AWS 데이터 이동 |

Spark라는 이유만으로 항상 EMR인 것은 아니며 Glue 등도 비교합니다. Kafka 호환·브로커 운영 요구가 MSK의 강한 단서입니다. EMR의 Spot 사용은 노드 역할·재시도·데이터 보존을 고려합니다.

### AI·ML 기능 사전

| 서비스 | 역할 |
|---|---|
| Rekognition | 이미지·영상 분석 |
| Transcribe | 음성 → 텍스트 |
| Polly | 텍스트 → 음성 |
| Translate | 언어 번역 |
| Comprehend | 자연어·감정 분석 |
| Textract | 스캔 문서의 글자·표·양식 추출 |
| Kendra | 기업 문서 검색 |
| Personalize | 개인화 추천 |
| Forecast | 시계열 예측의 기존 서비스. 신규 고객 제공 중단 |
| Lex | 대화형 봇 |
| SageMaker | ML 개발·학습·배포 관련 기능 |

문서에서 필드를 **추출**하는 Textract와 텍스트의 **의미를 분석**하는 Comprehend를 구분하세요. Forecast는 신규 구축에 그대로 추천하지 않습니다.

### 운영·앱 도구

| 도구 | 역할 |
|---|---|
| SSM Session Manager | SSH 인바운드 없이 지원 관리형 노드 접속 |
| SSM Run Command | 여러 노드에 명령 실행 |
| SSM Patch Manager | 지원 OS 패치 관리 |
| AWS Batch | 장시간 배치 작업 스케줄·컴퓨팅 관리 |
| AppSync | GraphQL·구독 등 지원 API 기능 |
| Amplify | 웹·모바일 개발·호스팅·백엔드 통합 |
| SES | 앱 이메일 발송·수신 관련 기능 |
| S3 Batch Operations | 기존 객체 집합에 복사·태그·복원 등 일괄 작업 |
| Aurora Cloning | Copy-on-write 테스트 복제 |
| X-Ray·관련 추적 도구 | 분산 요청 경로 분석 |
| Artifact | AWS 규정 준수 보고서 |

SSM에는 에이전트·IAM·서비스 연결이 필요합니다. 세션 로깅도 별도 설정과 SSH·포트 전달의 제약을 확인합니다. AppSync의 오프라인 동기화는 클라이언트·데이터 소스 등 추가 구성이 필요하며 이름만으로 자동 완성되지 않습니다.

## 문제 유형

1. 스캔 청구서의 표·필드 추출 → Textract.
2. 500대 노드에 정기 보안 패치 → Patch Manager·Maintenance Window.
3. 기존 Kafka 이전 → MSK 호환성 평가.
4. 온프레미스에서 AWS 사설 DNS 조회 → Resolver Inbound.
5. 외부 도메인 DNS를 VPC에서 조회 → Resolver Outbound + 전달 규칙.
6. 기존 S3 객체 암호화 방식 변경 → Batch Operations 등 재기록.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 서버 이전 / 서버 DR | MGN / DRS |
| DB 이전·변환 | DMS + 변환 평가 |
| 양식·표 추출 | Textract |
| Kafka / Spark | MSK / EMR·Glue 비교 |
| 현장 AWS 인프라 | Outposts |
| 인바운드 SSH 없이 관리 | Session Manager |
| 긴 배치 | Batch·ECS 등 |
| 앱 메일 | SES |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/37-GapFill-Services.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
