# 16. API Gateway — API의 출입구

## 개념 이해

API Gateway는 백엔드 앞의 안내 데스크입니다. 요청 라우팅·인증·제한·관찰을 맡습니다. 대표 서버리스 구조는 API Gateway → Lambda → DynamoDB입니다.

## 핵심 개념

| 유형 | 대표 기능·선택 기준 |
|---|---|
| REST API | 사용량 계획·API 키·캐싱·요청 검증 등 풍부한 기능 |
| HTTP API | 단순 HTTP·Lambda 프록시, JWT 인증, 비용·지연 비교 |
| WebSocket API | 지속적인 양방향 통신·채팅·실시간 알림 |

REST API의 Edge-optimized·Regional·Private 엔드포인트를 구분합니다. Private REST API는 Interface VPC Endpoint를 통해 접근합니다. HTTP API에 REST API의 모든 엔드포인트·기능이 있다고 가정하지 않습니다.

### 인증과 사용량

- AWS 자격 증명으로 서명: **IAM·SigV4**.
- Cognito 사용자 풀의 사용자: 지원 API 유형의 **Cognito/JWT authorizer**.
- 사용자 정의 토큰·추가 로직: **Lambda authorizer**.
- API 키·사용량 계획: 클라이언트별 사용량 구분과 제한. **API 키는 사용자 인증을 대신하지 않음**.
- HTTP 429: throttling·할당량·사용량 제한 확인, 호출자는 backoff와 재시도 설계.
- 반복 요청의 백엔드 부하: 지원 API의 캐싱과 캐시 키 검토.

### 타임아웃과 CORS

**원문의 “29초는 절대 늘릴 수 없다”는 설명은 잘못되었습니다.** Regional·Private REST API는 통합 타임아웃을 29초보다 늘릴 수 있으며 할당량 절충이 생길 수 있습니다. REST API 응답 스트리밍도 별도 지원 조건이 있습니다. API 유형·통합·전송 방식별 제한을 확인하세요.

긴 작업은 202 응답과 작업 ID를 반환하고 SQS·Step Functions·워커로 넘긴 뒤 조회·알림으로 완료를 전달하는 비동기 구조도 검토합니다. 타임아웃을 늘리는 것과 비동기 설계는 요구사항에 따라 선택합니다.

브라우저가 다른 도메인의 API를 호출할 때 CORS와 preflight 응답을 구성합니다. CORS는 인증이나 서버 간 네트워크 허용 정책의 대체 기능이 아닙니다.

## 문제 유형

1. 간단한 Lambda 프록시·JWT·낮은 비용 → HTTP API 검토.
2. 고객 등급별 사용량 계획·API 키 → REST API.
3. 실시간 양방향 채팅 → WebSocket API.
4. 사설 API → Private REST API + Interface Endpoint + 정책.
5. 2분 작업이 타임아웃 → API 제약을 확인하고 비동기 작업 구조 또는 지원되는 타임아웃 확장 비교.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| API 키·사용량 계획·캐싱 | REST API |
| 단순 프록시·JWT | HTTP API |
| 양방향 실시간 | WebSocket |
| AWS 요청 서명 | SigV4 |
| 커스텀 인증 | Lambda authorizer |
| 브라우저 교차 출처 | CORS |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/16-API-Gateway.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
