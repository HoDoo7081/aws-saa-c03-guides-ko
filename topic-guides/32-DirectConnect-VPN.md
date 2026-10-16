# 32. Direct Connect와 VPN

## 개념 이해

Site-to-Site VPN은 공용 도로로 다니는 암호화 차량이고, Direct Connect는 전용 연결입니다. **사설 경로와 암호화는 같은 개념이 아닙니다.**

## 핵심 개념

| 구분 | Site-to-Site VPN | Direct Connect |
|---|---|---|
| 경로 | 일반적으로 인터넷 기반 IPsec | 전용 연결·지원 파트너 연결 |
| 암호화 | IPsec | 기본으로 모든 구간 암호화되는 것은 아님 |
| 구축 | 상대적으로 빠름 | 회선·사업자·위치 준비 필요 |
| 성능 | 인터넷 경로·터널 구성에 영향 | 일관된 네트워크 성능에 유리 |
| 대표 요구 | 신속한 연결·백업 | 지속적인 대량 전송·사설 연결 |

VPN의 AWS 종단은 VGW 또는 TGW 등 지원 구조이며, Customer Gateway는 고객 라우터를 나타내는 AWS 리소스입니다. VPN 연결의 복수 터널과 온프레미스 이중화를 함께 구성합니다. 개별 직원 노트북 접근은 **Client VPN**을 검토합니다.

### Direct Connect 가상 인터페이스

| VIF | 일반적인 연결 대상 |
|---|---|
| Private VIF | VGW·Direct Connect Gateway 기반 VPC 접근 |
| Public VIF | AWS 퍼블릭 서비스 엔드포인트 |
| Transit VIF | Direct Connect Gateway를 통해 TGW 연결 |

DX Gateway는 지원되는 여러 리전·계정 연결을 모을 수 있지만 VPC 간 전이 라우터를 대신하는 것은 아닙니다. 암호화 요구에는 IPsec VPN over DX 또는 지원 회선의 MACsec 등 보호 구간을 비교합니다.

### 복원력

DX + VPN 백업은 비용을 고려한 대표 구조입니다. 더 높은 가용성에는 서로 다른 위치·장비·회선과 이중 DX를 검토합니다. VPN부터 사용하고 DX 준비 뒤 전환할 수도 있습니다. 원문의 회선 속도·구축 시간은 보편적 보장값이 아닙니다.

## 문제 유형

1. 당장 온프레미스 연결 필요 → VPN 검토.
2. 매일 대용량 전송과 일정한 성능 → DX.
3. DX의 비용 효율적 백업 → Site-to-Site VPN.
4. 전용 회선의 암호화 필요 → 지원 IPsec·MACsec 구성.
5. 원격 직원 장치 → Client VPN.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 신속·IPsec | Site-to-Site VPN |
| 일관된 전용 연결 | Direct Connect |
| 여러 리전 VPC 연결 지원 | DX Gateway |
| TGW로 DX 연결 | Transit VIF + DX Gateway |
| 개별 사용자 장치 | Client VPN |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/32-DirectConnect-VPN.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
