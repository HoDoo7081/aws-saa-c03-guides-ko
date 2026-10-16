# 28. CloudFormation — 코드형 인프라

## 개념 이해

콘솔에서 매번 임기응변으로 요리하는 대신 레시피를 저장하는 방식입니다. YAML·JSON 템플릿으로 인프라를 정의하고, 배포된 리소스 묶음을 Stack이라고 부릅니다. 버전 관리·반복 배포·변경 검토에 유용합니다.

## 핵심 개념

| 템플릿 항목 | 역할 |
|---|---|
| Parameters | 배포 시 입력 |
| Mappings | 리전별 값 등의 조회 표 |
| Conditions | 조건에 따른 리소스 생성 |
| Resources | 생성할 리소스 정의, 필수 섹션 |
| Outputs | 결과값·Export |

`!Ref`는 참조, `!GetAtt`는 리소스 속성, `!Sub`는 문자열 치환입니다. 다른 스택의 Export는 `Fn::ImportValue`로 참조할 수 있으며 리전·계정·수정 제약을 확인합니다.

| 기능 | 선택 단서 |
|---|---|
| Change Set | 실행 전에 예상 변경 검토 |
| Stack Policy | 스택 업데이트 중 특정 리소스 변경 제한 |
| Drift Detection | 템플릿과 실제 지원 속성의 차이 탐지 |
| StackSets | 여러 계정·리전에 템플릿 배포 |
| Nested Stacks | 하위 스택으로 재사용·모듈화 |
| DeletionPolicy | 삭제 시 Retain·지원 리소스 Snapshot 등 |
| UpdateReplacePolicy | 업데이트로 교체되는 이전 리소스 처리 |

Change Set이 모든 런타임 실패를 예측하지는 않고, Drift Detection도 모든 속성·리소스를 지원하지 않습니다. 생성 실패 시 기본 롤백이 가능하지만 보존·실패 처리 옵션이 있습니다. 스택 삭제·업데이트가 DB 데이터를 어떻게 처리할지 별도 정책을 확인하세요.

CDK는 TypeScript·Python 등으로 인프라를 작성하고 CloudFormation 템플릿으로 합성하는 도구입니다. Beanstalk는 앱 배포 중심, CloudFormation은 범용 리소스 선언 중심입니다.

## 문제 유형

1. 변경 전 생성·수정·삭제 검토 → Change Set.
2. 조직 여러 계정의 보안 기준 배포 → StackSets.
3. 콘솔 수동 변경 탐지 → Drift Detection.
4. 스택 삭제 뒤 DB 유지 → DeletionPolicy Retain.
5. 프로그래밍 언어로 인프라 정의 → CDK.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 변경 미리보기 | Change Set |
| 여러 계정·리전 | StackSets |
| 실제와 템플릿 차이 | Drift Detection |
| 삭제 시 보존 | DeletionPolicy |
| 교체 시 이전 자원 보존 | UpdateReplacePolicy |
| 재사용 하위 스택 | Nested Stacks |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/28-CloudFormation.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
