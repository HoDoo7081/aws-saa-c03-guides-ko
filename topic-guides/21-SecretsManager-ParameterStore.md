# 21. Secrets Manager와 Parameter Store

## 개념 이해

앱 비밀번호를 코드에 적는 대신 잠금 보관함에 넣고 실행 시 필요한 권한으로 읽습니다. Parameter Store는 계층형 설정 보관함, Secrets Manager는 비밀 수명 주기와 교체에 초점을 둔 보관함입니다.

## 핵심 개념

| 구분 | Secrets Manager | Systems Manager Parameter Store |
|---|---|---|
| 주 용도 | DB 암호·API 비밀과 교체 | 앱 설정·경로형 파라미터·비밀 |
| 교체 | 지원 서비스의 관리형 교체·Lambda 기반 교체 | 자체적인 비밀번호 자동 교체 기능은 없음 |
| 암호화 | KMS | SecureString에 KMS |
| 크기 | 최대 64KB | Standard 4KB, Advanced 8KB |
| 비용 | 비밀 저장·API 등의 비용 | Standard 저장은 추가 요금 없음, 고처리량·KMS 등 별도 비용 가능 |

Parameter Store 경로 예: `/prod/db/host`, `/dev/db/host`. 경로별 권한과 `GetParametersByPath`의 재귀 접근 범위를 주의합니다. Advanced에는 파라미터 정책 등이 있습니다. 고급 파라미터의 계정 간 공유 기능도 있으므로 “Parameter Store는 어떤 경우에도 공유 불가”라고 외우지 않습니다.

Secrets Manager는 서비스별로 교체 구현이 다릅니다. “모든 비밀에 내장 Lambda가 자동으로 생겨 코드가 전혀 필요 없다”는 뜻은 아닙니다. 교체 권한·DB 연결·앱 캐시 갱신·기존 연결을 함께 설계해야 무중단에 가깝게 운영할 수 있습니다.

### 앱 접근 패턴

앱의 IAM 역할 → `GetSecretValue` 또는 `GetParameter` → 필요한 KMS 복호화 권한 → 서비스 연결 순서입니다. ECS의 시작 시 비밀 주입과 런타임 조회는 갱신 시점이 다릅니다. 주입된 환경 변수는 비밀이 교체된다고 자동 갱신되지 않으므로 새 태스크 배포 등을 고려합니다.

계정 간 Secrets Manager 사용에는 리소스 정책·호출자 IAM·고객 관리 KMS 키 등 필요한 권한을 확인합니다. API 접근 감사는 CloudTrail을 활용합니다. 비용은 리전·키·호출량별로 산정합니다.

## 문제 유형

1. RDS 암호 정기 자동 교체 → Secrets Manager 지원 교체 방식.
2. 낮은 비용의 환경별 앱 설정 → Parameter Store.
3. 소스에 DB 암호 하드코딩 → 비밀 저장소 + 역할 + 안전한 조회·갱신.
4. 20KB 비밀 → Secrets Manager 크기 범위 검토.
5. 교체 후 앱 인증 오류 → 앱 캐시·환경 주입 갱신·DB 상태·교체 단계 확인.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 자동 비밀번호 교체 | Secrets Manager |
| /prod·/dev 계층형 설정 | Parameter Store |
| 암호화 파라미터 | SecureString |
| 앱의 AWS 접근 | IAM 역할 |
| 비밀 접근자 감사 | CloudTrail |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/21-SecretsManager-ParameterStore.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
