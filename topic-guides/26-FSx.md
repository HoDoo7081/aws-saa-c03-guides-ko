# 26. FSx 제품군

## 개념 이해

EFS가 관리형 NFS 공유 저장소라면 FSx는 특정 파일시스템 기술의 전문점입니다. 서비스 이름만 보지 말고 프로토콜·가용성·성능·기존 기술을 맞추세요.

## 핵심 개념

| 제품 | 핵심 단서 | 기능 |
|---|---|---|
| FSx for Windows File Server | SMB·Windows·NTFS·AD | 관리형 Windows 공유, 지원 Multi-AZ 구성 |
| FSx for Lustre | HPC·ML 데이터·병렬 처리 | 높은 처리량의 병렬 파일시스템, S3 연동 |
| FSx for NetApp ONTAP | NetApp·SnapMirror·다중 프로토콜 | NFS·SMB·iSCSI 등 |
| FSx for OpenZFS | ZFS·NFS·스냅샷·복제 | 관리형 OpenZFS |

### 구분할 사항

- Windows File Server는 Managed Microsoft AD 또는 지원 자체 관리 AD와 통합합니다. 단순한 Windows OS 언급보다 SMB·Windows 파일 권한 요구가 강한 단서입니다.
- Lustre는 S3 데이터와 연결해 대규모 연산에 파일 접근을 제공합니다. 데이터 가져오기·내보내기·동기화 설정을 구성해야 하며 S3 자체가 범용 POSIX 파일시스템으로 바뀌는 것은 아닙니다.
- 원문의 Lustre Scratch는 임시 처리, Persistent는 저장 복제와 지속 사용 맥락입니다. 배포 유형별 현재 제공·리전 지원을 확인하세요.
- ONTAP은 NetApp 이전·SnapMirror·같은 환경의 여러 프로토콜 요구에 유용합니다.
- OpenZFS는 ZFS 기반 저장소의 스냅샷·클론·NFS 요구를 비교합니다.
- “여러 Linux EC2가 공유 파일만 필요”라면 EFS도 비교합니다. 고성능·프로토콜·운영 요구가 FSx 선택을 정합니다.

## 문제 유형

1. Windows 사용자 홈 디렉터리와 AD, AZ 장애 대응 → FSx Windows Multi-AZ.
2. S3 학습 데이터의 병렬 파일 접근 → FSx Lustre.
3. 기존 NetApp의 SnapMirror 이전 → FSx ONTAP.
4. NFS·SMB·iSCSI를 함께 요구 → FSx ONTAP.
5. ZFS 앱 이전 → FSx OpenZFS.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| SMB·NTFS·AD | Windows File Server |
| HPC·병렬·S3 연동 | Lustre |
| NetApp·SnapMirror | ONTAP |
| ZFS | OpenZFS |
| 탄력적 공유 NFS | EFS도 비교 |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/26-FSx.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
