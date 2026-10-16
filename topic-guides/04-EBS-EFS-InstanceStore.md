# 04. EBS, EFS와 인스턴스 스토어

## 개념 이해

EC2를 책상이라고 하면 EBS는 개인 외장 디스크, EFS는 사무실 공유 폴더, 인스턴스 스토어는 책상에 붙은 임시 메모장입니다. **블록·파일·임시 로컬 저장소**를 구분하는 것이 출발점입니다.

## 핵심 개념

| 저장소 | 범위·특징 | 주 용도 |
|---|---|---|
| EBS | 한 AZ의 네트워크 블록 스토리지 | 부팅 디스크·DB |
| EFS | NFS 공유 파일시스템, Regional 유형은 여러 AZ | Linux 서버들의 공용 파일 |
| Instance Store | 호스트 로컬 디스크, 중지·종료·호스트 장애 시 손실 가능 | 캐시·임시 처리·앱 수준 복제 데이터 |
| FSx | Windows·Lustre·ONTAP·OpenZFS 파일시스템 | SMB·HPC 등 |
| S3 | 객체 API | 객체·백업·데이터 레이크 |

### EBS 볼륨

- 기본적으로 한 번에 한 인스턴스에 연결. io1/io2 Multi-Attach는 같은 AZ의 지원 인스턴스 최대 16개에 연결 가능하지만, 동시 쓰기를 안전하게 처리하는 클러스터 소프트웨어가 필요.
- 인스턴스와 별개로 존속하지만 `DeleteOnTermination` 설정에 따라 종료 시 삭제될 수 있음. 루트 볼륨의 기본값을 특히 확인.
- 새 빈 볼륨은 파일시스템 생성·마운트가 필요. 기존 데이터 볼륨을 다시 포맷하지 않기.
- 다른 AZ로 옮길 때는 스냅샷으로 새 볼륨 생성.

| 유형 | 판단 기준 |
|---|---|
| gp3 | 일반 SSD, 용량과 IOPS·처리량을 독립적으로 조절 |
| gp2 | 용량에 성능이 연동되는 기존 일반 SSD |
| io1 / io2 | 프로비저닝 IOPS, 까다로운 DB 성능·지연 요구 |
| st1 | 빈번한 대용량 순차 I/O, 처리량 중심 HDD |
| sc1 | 접근이 적은 순차 I/O, 저렴한 HDD |

IOPS는 작은 랜덤 읽기·쓰기 횟수, 처리량은 초당 전송하는 데이터 크기입니다. HDD 유형은 부팅 볼륨으로 사용할 수 없습니다. **원문의 gp3 16,000 IOPS 상한은 오래된 수치**이므로 단순히 이를 넘으면 io2라고 외우지 마세요. 일반 리전의 gp3는 최대 80,000 IOPS·2,000MiB/s를 지원하며, 인스턴스·볼륨 크기·환경별 조건을 함께 확인합니다.

### 스냅샷과 암호화

스냅샷은 증분 백업입니다. 리전 간 복사로 DR을 구성하고, DLM으로 생성·보존·삭제를 자동화할 수 있습니다. Snapshot Archive는 드물게 복원하는 백업 비용 절감, Recycle Bin은 삭제 복구, Fast Snapshot Restore는 복원 후 초기 읽기 지연 완화에 사용합니다.

암호화되지 않은 기존 볼륨은 스위치 하나로 암호화하지 못합니다. **스냅샷 → 암호화된 복사본 → 새 볼륨 생성 → 교체** 절차를 사용합니다.

### EFS와 인스턴스 스토어

EFS는 NFS 기반 공유 저장소입니다. 여러 AZ의 Linux ASG가 업로드 파일·CMS 콘텐츠를 공유하는 대표 답입니다. Windows SMB·AD 연동 요구에는 FSx for Windows File Server를 검토합니다.

EFS의 성능 모드와 처리량 모드는 다른 설정입니다. General Purpose·Max I/O, Elastic·Provisioned·Bursting의 호환성과 지연을 확인하며, 클라이언트 수만 보고 Max I/O를 고르지 않습니다. Lifecycle로 IA·Archive 계층을 활용할 수 있습니다. EFS에는 One Zone 유형도 있으므로 모든 EFS가 다중 AZ라고 가정하지 마세요.

인스턴스 스토어는 재부팅 시에는 유지되지만 중지·최대 절전·종료 시 데이터가 유지되지 않습니다. EBS처럼 분리하거나 EBS 스냅샷을 만들 수 없습니다. 영구 보존 요구를 단독으로 만족시키지 못합니다.

## 문제 유형

1. ASG의 여러 Linux 서버가 같은 업로드 파일을 사용 → EFS.
2. 인스턴스 종료 뒤에도 데이터 디스크 보존 → `DeleteOnTermination=false` 확인.
3. 임시 계산 결과에 매우 높은 로컬 I/O 필요 → 인스턴스 스토어.
4. gp2에서 용량 증가 없이 성능 조절 → gp3 전환 비교.
5. 다른 리전에 EBS 복구 지점 보관 → 스냅샷 복사.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 랜덤 I/O·부팅 | SSD EBS |
| 순차 I/O·처리량 | st1·sc1 |
| 공유 NFS·Linux | EFS |
| 공유 SMB·Windows | FSx for Windows |
| 임시·스크래치·캐시 | Instance Store |
| 기존 EBS 암호화 | 스냅샷 → 암호화 복사 → 복원 |
| 복원 직후 성능 | Fast Snapshot Restore |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/04-EBS-EFS-InstanceStore.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
