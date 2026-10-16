# 20. KMS와 CloudHSM — 암호화 키 관리

## 개념 이해

암호화에서 어려운 부분은 키를 누가 보관·사용·교체하는가입니다. KMS는 AWS 서비스와 통합된 관리형 금고, CloudHSM은 고객이 키 사용자와 암호화 운영을 더 직접 관리하는 전용 하드웨어 금고에 가깝습니다.

## 핵심 개념

| 키 구분 | 제어 |
|---|---|
| AWS owned key | 서비스가 소유·운영, 고객 계정에 직접 표시되지 않음 |
| AWS managed key | 계정에서 보이지만 정책·수명 주기는 AWS 관리 |
| Customer managed key | 고객이 정책·활성 상태·지원되는 교체 설정 등을 관리 |

요금·교체 주기는 키 유형·원본·기능별로 확인합니다. 고객 관리 대칭 키의 자동 교체를 “매년만 가능”이라고 한정하지 마세요.

### 권한과 봉투 암호화

KMS 키 정책이 핵심입니다. 같은 계정에서는 키 정책의 직접 부여 또는 IAM 위임 등의 방식이 있고, 계정 간 사용에는 양쪽 권한 구성이 필요합니다. **“모든 경우 키 정책과 IAM 양쪽에 동일 Allow가 반드시 필요”는 과도한 단순화**입니다. Deny·Grant·조건도 확인합니다.

일반 대칭 KMS Encrypt API는 직접 암호화할 데이터가 최대 4KB입니다. 큰 파일은 **봉투 암호화(Envelope encryption)**를 사용합니다.

1. `GenerateDataKey`로 평문 데이터 키와 암호화된 데이터 키를 받습니다.
2. 평문 키로 데이터를 로컬 암호화한 뒤 평문 키를 안전하게 폐기합니다.
3. 암호문 옆에 암호화된 데이터 키를 저장합니다.
4. 복호화 시 KMS로 데이터 키를 풀고 데이터를 복호화합니다.

### 수명 주기와 전용 HSM

KMS 키 삭제 예약에는 일반적으로 **7~30일 대기**가 있습니다. 사용을 먼저 중지하려면 비활성화를 검토하되 앱 영향·복구 가능성을 확인합니다. Multi-Region 키는 관련 키 자료로 리전 간 암복호화를 지원하지만 정책·권한·서비스 통합은 리전별로 구성합니다. S3 Bucket Key는 SSE-KMS 호출·비용을 줄일 수 있습니다.

CloudHSM은 전용 HSM, 고객 키 관리·표준 암호화 인터페이스·특수 규정 요구에 적합합니다. 그러나 **“KMS는 FIPS Level 2이므로 Level 3이면 무조건 CloudHSM”은 오래된 설명**입니다. 현재 KMS도 FIPS 140-3 Security Level 3 검증 HSM을 사용합니다. 전용성·운영 책임·인증서 범위로 비교하세요.

## 문제 유형

1. 키 사용 API 감사 → KMS + CloudTrail.
2. 100MB 파일을 KMS로 보호 → 데이터 키를 이용한 봉투 암호화.
3. 키 사용을 중단하고 복구 가능성을 유지 → 비활성화 검토.
4. 전용 HSM과 고객 직접 키 운영 → CloudHSM.
5. S3의 KMS 요청 비용 감소 → Bucket Key.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| AWS 통합 키 관리 | KMS |
| 큰 파일 암호화 | GenerateDataKey·봉투 암호화 |
| 키 사용 감사 | CloudTrail |
| 리전 간 관련 키 자료 | Multi-Region keys |
| 전용 HSM·직접 운영 | CloudHSM |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/20-KMS-CloudHSM.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
