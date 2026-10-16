# 25. Storage Gateway, DataSync와 Transfer Family

## 개념 이해

Storage Gateway는 로컬 파일함처럼 보이는 클라우드 연결 통로, DataSync는 데이터를 옮기는 이사업체, Transfer Family는 파트너의 기존 파일 전송 방식을 받는 접수창구입니다. **지속 접근·복사·프로토콜 제공**을 나누세요.

## 핵심 개념

| 기능 | 프로토콜·저장 구조 | 목적 |
|---|---|---|
| S3 File Gateway | NFS·SMB, S3 객체 + 로컬 캐시 | 기존 파일 앱의 지속적인 하이브리드 접근 |
| Volume Gateway Cached | iSCSI, 주 데이터는 클라우드·캐시는 로컬 | 로컬 용량 확장 |
| Volume Gateway Stored | iSCSI, 전체 주 데이터 로컬·클라우드 스냅샷 | 전체 데이터의 로컬 저지연 접근과 백업 |
| Tape Gateway | iSCSI VTL, 클라우드 가상 테이프 | 기존 테이프 백업 소프트웨어 유지 |
| DataSync | 지원 파일·객체 저장소 사이 복사 | 대량 이전·주기적 동기화 |
| Transfer Family | SFTP·FTPS·FTP 등 지원 전송 방식 | 기존 파트너 전송 흐름으로 S3·EFS 등 접근 |

원문의 **FSx File Gateway는 신규 고객 제공이 중단**되었습니다. 기존 서비스 맥락과 신규 구축을 구분하세요.

DataSync는 메타데이터·권한 보존, 검증·일정·대역폭 제어 등을 지원하지만 **모든 프로토콜의 모든 권한이 S3로 동일하게 옮겨지는 것은 아닙니다**. 소스·대상 조합별 지원을 확인합니다. 온프레미스 연결에는 에이전트가 필요한 구성이 있고, AWS 저장소 사이 전송은 에이전트가 필요 없는 구성이 있습니다.

Transfer Family도 프로토콜별 저장소·엔드포인트 제약이 다릅니다. 관리형 SFTP가 요구를 만족하면 EC2 SFTP 서버의 운영 부담을 줄일 수 있지만 모든 EC2 답이 항상 틀린 것은 아닙니다.

## 문제 유형

1. NAS 데이터를 정기적으로 EFS에 복사, 대역폭 제어 → DataSync.
2. S3에 저장하되 기존 SMB·NFS 앱이 계속 파일 접근 → S3 File Gateway.
3. 기존 테이프 백업 도구를 유지하면서 물리 테이프 제거 → Tape Gateway.
4. 온프레미스 전체 데이터의 낮은 지연 + 클라우드 백업 → Stored Volume Gateway.
5. 거래처 SFTP 업로드를 S3로 받기 → Transfer Family.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 지속 하이브리드 파일 접근 | File Gateway |
| 주 데이터 클라우드 / 로컬 | Cached / Stored Volume Gateway |
| 가상 테이프 | Tape Gateway |
| 이전·정기 복사 | DataSync |
| 파트너 SFTP | Transfer Family |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/25-StorageGateway-DataSync-TransferFamily.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
