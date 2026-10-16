# 06. VPC — 가상 사설 클라우드

## 개념 이해

VPC는 AWS 안의 전용 부지입니다. CIDR은 주소 구획, 서브넷은 방, 라우팅 테이블은 표지판, 보안 그룹과 NACL은 출입 통제입니다. VPC는 한 리전, 서브넷은 한 AZ에 속합니다.

## 핵심 개념

### 인터넷 연결

퍼블릭 서브넷은 인터넷 게이트웨이(IGW)로 향하는 경로가 있는 서브넷입니다. EC2의 인터넷 통신에는 경로뿐 아니라 적절한 IP와 보안 규칙도 필요합니다.

프라이빗 인스턴스가 외부 업데이트를 받아야 하면 퍼블릭 NAT Gateway를 검토합니다. 기존 **Zonal NAT Gateway** 구조에서는 퍼블릭 서브넷의 NAT와 IGW를 거치며, 다중 AZ 가용성을 위해 AZ별 NAT와 로컬 경로를 구성합니다. NAT Instance는 직접 운영하고 소스/대상 확인을 비활성화해야 합니다. IPv6의 외부 시작 연결은 egress-only IGW를 검토합니다.

> Regional NAT Gateway는 워크로드에 따라 여러 AZ로 자동 확장하며 호스팅용 퍼블릭 서브넷이 필요하지 않습니다. 원문은 Zonal NAT 중심입니다. NAT 유형과 IPv6 변환 지원을 확인하지 않고 “NAT는 항상 AZ별”, “IPv6에는 NAT가 없다”라고 일반화하지 마세요.

IPv4 VPC CIDR의 대표 범위는 /16~ /28이며, 일반 서브넷에서 AWS가 5개 주소를 예약합니다. /24의 256개 중 일반적으로 251개를 사용할 수 있습니다. 연결할 네트워크는 CIDR 중복을 피하세요.

### 보안 그룹과 NACL

| 구분 | 보안 그룹(SG) | Network ACL |
|---|---|---|
| 적용 | ENI·리소스 | 서브넷 경계 |
| 상태 추적 | Stateful | Stateless |
| 규칙 | Allow만 | Allow·Deny |
| 평가 | 적용 규칙을 함께 평가 | 낮은 번호부터 첫 일치 |
| 대상 표현 | CIDR·다른 SG 등 | CIDR |

SG는 허용된 연결의 응답을 자동 허용합니다. NACL은 왕복 방향 규칙을 따로 설정하며 클라이언트 임시 포트 범위도 고려해야 합니다. “응답이 안 온다”는 상황에서 어느 방향 규칙이 부족한지는 패킷 흐름에 따라 판단합니다.

### 엔드포인트와 VPC 연결

| 기능 | 적합한 요구 |
|---|---|
| Gateway Endpoint | 같은 VPC의 S3·DynamoDB 접근, 엔드포인트 추가 요금 없음 |
| Interface Endpoint | 서브넷 ENI의 사설 IP로 지원 서비스 접근, 시간·데이터 비용 |
| VPC Peering | 두 VPC의 직접 연결. 전이적 라우팅 불가 |
| Transit Gateway | 여러 VPC·VPN·온프레미스 연결을 허브로 통합 |
| PrivateLink | 전체 네트워크 연결 없이 특정 서비스 제공. 중복 CIDR도 가능 |
| VPC Sharing | RAM으로 공유한 서브넷에 여러 계정이 리소스 배치 |

S3 Gateway Endpoint는 피어링·VPN 뒤의 온프레미스에서 직접 사용할 수 없습니다. 그런 요구는 S3 Interface Endpoint와 라우팅·DNS를 검토합니다. 엔드포인트 정책은 IAM·리소스 정책을 대체하지 않습니다.

Peering A↔B, B↔C가 있어도 A↔C는 자동 연결되지 않습니다. 완전 메시 연결 수는 n(n−1)/2입니다. TGW도 라우팅·보안 설정을 해야 하며 연결만으로 모든 네트워크를 무조건 개방하지 않습니다. PrivateLink 서비스 제공자는 일반적으로 NLB 기반 엔드포인트 서비스를 구성합니다.

### 관찰과 관리

Flow Logs는 IP·포트·ACCEPT/REJECT 같은 **메타데이터**, Traffic Mirroring은 **패킷 복사**입니다. Session Manager는 인바운드 SSH·퍼블릭 IP 없이 관리할 수 있지만 에이전트·역할·서비스 연결이 필요합니다. 세션 로깅도 별도 구성과 프로토콜 제한을 확인하세요.

## 문제 유형

1. 프라이빗 EC2가 S3에 낮은 비용으로 접근 → Gateway Endpoint.
2. DB에는 앱 서버만 접속 → DB SG의 원본을 앱 SG로 지정.
3. 특정 악성 IP를 네트워크 계층에서 차단 → NACL Deny.
4. 많은 VPC와 VPN을 중앙 연결 → TGW.
5. CIDR이 겹치는 고객에게 API 하나만 제공 → PrivateLink.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 인스턴스 상태 추적 방화벽 | SG |
| 서브넷·Deny·임시 포트 | NACL |
| S3·DynamoDB, VPC 내부 | Gateway Endpoint |
| 사설 서비스·소비자 VPC | PrivateLink |
| 다수 네트워크의 허브 | TGW |
| SSH 포트 없이 관리 | Session Manager |
| 통신 메타데이터 / 패킷 | Flow Logs / Traffic Mirroring |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/06-VPC.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
