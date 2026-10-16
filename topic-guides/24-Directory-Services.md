# 24. Directory Service — 디렉터리 연동

## 개념 이해

Active Directory는 회사의 사용자·컴퓨터·그룹을 관리하는 명부이자 인증 체계입니다. 도메인 가입은 컴퓨터가 그 체계에 참여하는 것이고, LDAP는 디렉터리 통신 프로토콜입니다. AWS에서는 전체 AD, 기존 AD로 연결하는 프록시, 단순 호환 디렉터리를 구분합니다.

## 핵심 개념

| 서비스 | 성격 | 적합한 요구 |
|---|---|---|
| AWS Managed Microsoft AD | AWS가 운영하는 Microsoft AD | 클라우드 AD 기능·지원 신뢰 관계·AWS 서비스 통합 |
| AD Connector | 기존 AD로 요청을 전달하는 게이트웨이 | 지원 AWS 앱에서 온프레미스 신원을 사용 |
| Simple AD | Samba 기반 호환 디렉터리 | 지원 범위 내 단순 디렉터리 요구 |

AD Connector는 독립 사용자 디렉터리를 새로 만드는 기능이 아니며 온프레미스 연결과 DNS가 중요합니다. Simple AD는 신뢰 관계·MFA 등 여러 고급 AD 기능을 지원하지 않습니다. Managed AD의 Trust는 방향·종류·DNS·네트워크·서비스별 지원을 확인해야 하며 “무조건 양방향만 가능”은 아닙니다.

### 서비스별 AD 통합을 구분하기

**FSx for Windows File Server는 AWS Managed Microsoft AD 또는 지원되는 자체 관리 AD와 통합**합니다. AD Connector를 FSx의 지원 디렉터리로 선택하는 것은 원문의 오류입니다. WorkSpaces·RDS for SQL Server 등도 각자의 지원 방식이 있으므로 모두 같은 세 가지 옵션에서 자유롭게 고른다고 생각하지 마세요.

“AD에 가입하면 모든 Windows 로그인이 자동으로 MFA”라는 뜻도 아닙니다. MFA 적용은 사용하는 앱·인증 흐름·RADIUS 등 통합 범위에 따라 다릅니다.

## 문제 유형

1. 클라우드에서 실제 AD와 온프레미스 신뢰 관계 필요 → Managed Microsoft AD.
2. WorkSpaces에서 기존 온프레미스 신원 사용, 디렉터리 복제 지양 → 지원되는 AD Connector 구조.
3. 단순 호환 디렉터리, 고급 신뢰·MFA 불필요 → Simple AD 지원 범위 검토.
4. FSx Windows에서 기존 AD 사용 → 자체 관리 AD 직접 통합 또는 Managed AD와 적절한 신뢰 관계.
5. Connector 연결이 끊겨 인증 실패 → 온프레미스 경로·DNS·도메인 컨트롤러 확인.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 클라우드의 실제 AD | Managed Microsoft AD |
| 기존 AD 요청 중계 | AD Connector |
| Samba 기반·단순 | Simple AD |
| FSx Windows AD | Managed AD·자체 관리 AD |
| 디렉터리 간 신뢰 | Managed AD의 지원 Trust |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/24-Directory-Services.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
