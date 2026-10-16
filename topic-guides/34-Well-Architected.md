# 34. Well-Architected Framework

## 개념 이해

건물이 서 있기만 한다고 좋은 건물은 아닙니다. 보안·화재 대비·유지비·운영도 평가해야 합니다. Well-Architected는 클라우드 워크로드를 여섯 관점으로 검토하는 틀입니다.

## 핵심 개념

| 원칙 | 질문 | 대표 접근 |
|---|---|---|
| 운영 우수성(Operational Excellence) | 어떻게 운영·개선하는가? | IaC·관찰·작고 되돌릴 수 있는 변경 |
| 보안(Security) | 데이터·시스템을 어떻게 보호하는가? | 최소 권한·계층 방어·암호화 |
| 안정성(Reliability) | 실패를 견디고 복구하는가? | 장애 격리·자동 복구·용량 관리 |
| 성능 효율성(Performance Efficiency) | 적절한 자원을 효율적으로 쓰는가? | 적합한 기술·실험·캐시·글로벌 배치 |
| 비용 최적화(Cost Optimization) | 요구를 만족하면서 비용을 줄이는가? | 측정·적정 크기·적합한 구매 옵션 |
| 지속 가능성(Sustainability) | 환경 영향을 줄이는가? | 활용률·효율적 하드웨어·불필요한 처리 감소 |

서비스 하나가 원칙 하나에만 속하지는 않습니다. 예를 들어 Auto Scaling은 가용성·성능·비용·활용률을 함께 개선할 수 있습니다. Serverless나 Graviton도 실제 호환성·사용량을 고려해야 합니다.

Well-Architected Tool은 질문을 통해 위험과 개선 과제를 정리하는 도구입니다. 설문을 완료했다고 시스템의 안전성·규정 준수가 자동 인증되는 것은 아닙니다.

### 문제를 읽는 방법

먼저 모든 기능 요구를 만족하는 선택지를 남기고, 마지막의 **MOST cost-effective·LEAST operational overhead·highly available** 같은 최적화 기준으로 비교합니다. 관리형 서비스라는 이유만으로 필수 기능이 빠진 선택지를 고르면 안 됩니다.

## 문제 유형

1. 중단 가능한 배치의 비용 최소화 → Spot 등 지원 옵션 비교.
2. 단일 AZ DB의 안정성 개선 → 적절한 Multi-AZ.
3. 컨테이너 서버 운영 부담 감소 → Fargate 등 지원 관리형 실행.
4. 글로벌 HTTP 콘텐츠 성능 → CloudFront·캐시.
5. 아키텍처 개선 계획 → Well-Architected Tool.

## 빠른 복습표

| 단서 | 원칙 |
|---|---|
| 반복 운영·작은 변경 | 운영 우수성 |
| 최소 권한·방어 | 보안 |
| 장애 복구 | 안정성 |
| 적합한 자원·속도 | 성능 효율성 |
| 측정·지출 최적화 | 비용 최적화 |
| 활용률·환경 영향 | 지속 가능성 |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/34-Well-Architected.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
