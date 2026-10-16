# 23. Cognito — 앱 사용자 인증과 AWS 접근

## 개념 이해

사용자 풀은 로그인 후 방문증을 발급하는 창구, 자격 증명 풀은 그 신원 정보를 AWS 임시 출입증으로 바꾸는 창구입니다. **JWT와 AWS 자격 증명은 다릅니다.**

## 핵심 개념

| 구분 | User Pools | Identity Pools |
|---|---|---|
| 주 역할 | 사용자 가입·로그인·신원 연동 | 인증된 신원·게스트에 AWS 임시 자격 증명 제공 |
| 결과 | ID·Access·Refresh 토큰 등 | STS 기반 임시 AWS 자격 증명 |
| 기능 | MFA·로그인 UI·지원 소셜/OIDC/SAML 연동 | 역할 매핑·비인증 게스트·세분화된 AWS 권한 |
| 연동 | API Gateway·ALB 등 지원 방식 | 앱에서 S3·DynamoDB 등 AWS API 접근 |

대표 흐름: 사용자 로그인 → User Pool 토큰 → Identity Pool에서 역할 기반 자격 증명 발급 → 앱이 허용된 AWS API 호출. Identity Pool은 지원 외부 IdP를 직접 사용할 수도 있어 User Pool이 항상 필수는 아닙니다.

Cognito 토큰만으로 S3 API를 그대로 호출하지는 않습니다. 직접 AWS 자격 증명이 필요한 구조에는 Identity Pool, 제한된 업로드·다운로드만 필요하면 백엔드의 S3 사전 서명 URL도 비교합니다.

API Gateway authorizer는 지원 유형에 맞는 JWT를 검사합니다. ID 토큰과 Access 토큰의 목적·scope·검증 대상을 구분하세요. ALB는 Cognito 인증 후 요청을 전달할 수 있습니다.

사용자별 S3 경로 제한에는 Identity Pool 역할의 `${cognito-identity.amazonaws.com:sub}` 같은 정책 변수를 사용할 수 있습니다. 이 값은 User Pool의 sub와 다른 식별자라는 점에 주의합니다. 게스트 접근도 최소 권한으로 제한해야 합니다.

직원의 AWS 계정 SSO는 IAM Identity Center가 대표 답이고, 앱 고객 인증은 Cognito가 대표 답입니다. “직원이면 무조건 Cognito 불가”라는 뜻은 아니며 실제 목적을 봅니다.

## 문제 유형

1. 가입·로그인·소셜 로그인 → User Pools.
2. 모바일 앱에서 역할 기반 S3 직접 접근 → Identity Pools.
3. 로그인 전 제한된 AWS 접근 → Identity Pool 비인증 역할 검토.
4. 각 사용자 자신의 S3 폴더만 접근 → IAM 정책 변수·접두사 제한.
5. 사내 직원이 여러 AWS 계정에 SSO → IAM Identity Center.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 가입·로그인·MFA | User Pools |
| JWT 발급 | User Pools |
| 앱의 임시 AWS 자격 증명 | Identity Pools |
| 게스트 AWS 접근 | 비인증 Identity Pool 역할 |
| AWS 계정 직원 SSO | IAM Identity Center |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/23-Cognito.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
