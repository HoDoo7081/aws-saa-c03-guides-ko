# 33. Transit Gateway

## 개념 이해

VPC마다 서로 직접 연결하면 케이블이 복잡해집니다. n개 VPC의 완전 메시 Peering은 n(n−1)/2개 연결이 필요합니다. TGW는 여러 네트워크를 모으는 허브로, 전이적 라우팅을 설계할 수 있습니다.

## 핵심 개념

VPC·Site-to-Site VPN·TGW 피어링 등을 연결하며, Direct Connect는 **Transit VIF → DX Gateway → TGW** 구조를 검토합니다. 다른 계정에는 RAM으로 공유할 수 있고 리전 간 TGW 피어링도 가능합니다.

### 라우팅과 검사

- 각 연결의 라우팅 테이블 **연결(Association)**과 경로 **전파(Propagation)**를 구분합니다.
- 운영·개발 연결에 서로 다른 테이블을 적용해 네트워크를 분리할 수 있습니다.
- 라우팅뿐 아니라 VPC 라우트·SG·NACL·방화벽도 맞아야 통신합니다.
- Appliance mode는 검사 VPC에서 지원되는 대칭 흐름을 유지하도록 돕습니다. 방화벽을 배치했다고 모든 트래픽이 자동 검사되는 것은 아닙니다.
- Multicast domain으로 지원되는 IP 멀티캐스트를 구성할 수 있습니다. 리전·인스턴스·연결 유형 제한을 확인하며 모든 TGW 연결에 멀티캐스트가 전파되는 것은 아닙니다.

### 다른 연결과 비교

| 요구 | 검토할 도구 |
|---|---|
| 소수 VPC의 직접 연결 | Peering |
| 많은 VPC·온프레미스 허브 | TGW |
| 고객에게 특정 서비스만 노출 | PrivateLink |

TGW는 연결 시간·데이터 처리 등의 비용을 고려해야 합니다. “VPC가 4개면 무조건 TGW” 같은 숫자 기준은 아니며 라우팅·관리·전송 요구로 판단합니다. TGW·Peering도 정책을 구성하면 접근을 제한할 수 있으므로 전체 네트워크가 무조건 모두 개방되는 것은 아닙니다.

## 문제 유형

1. 25개 VPC와 데이터센터의 중앙 연결 → TGW.
2. 운영·개발은 격리하되 공용 온프레미스 접근 → 분리된 TGW 라우팅 테이블.
3. 조직의 네트워크 허브 공유 → RAM.
4. 중앙 방화벽을 통과하는 대칭 흐름 → 라우팅 + Appliance mode 검토.
5. CIDR이 겹치는 외부 고객에게 API 하나 제공 → PrivateLink 비교.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 전이적 허브 | TGW |
| 환경 격리 | TGW 라우팅 테이블 |
| 계정 간 공유 | RAM |
| 리전 간 허브 | TGW peering |
| 검사 흐름 대칭 | Appliance mode |
| 지원 IP 멀티캐스트 | Multicast domain |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/33-Transit-Gateway.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
