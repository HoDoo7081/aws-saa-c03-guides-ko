# 27. Snow 제품군 — 물리적 데이터 이전과 엣지

## 개념 이해

대역폭이 너무 낮으면 데이터를 네트워크로 보내는 것보다 암호화된 장비를 배송하는 편이 빠를 수 있습니다. Snow는 이 물리적 이전과 연결이 불안정한 현장의 로컬 처리에 사용되어 왔습니다.

## 핵심 개념

> **제공 상태 주의:** Snowball Edge는 2025년 11월 7일부터 기존 고객만 이용할 수 있습니다. Snowcone은 중단되었고 일부 이전 장비도 종료되었습니다. 원문의 Snowmobile·용량 표를 현재 주문 가능한 카탈로그처럼 사용하지 마세요. [AWS 장비 변경 안내](https://aws.amazon.com/blogs/storage/aws-snow-device-updates/)와 [문서 이력](https://docs.aws.amazon.com/snowball/latest/developer-guide/doc-history.html)을 먼저 확인합니다.

| 원문에 등장하는 이름 | 학습할 개념 |
|---|---|
| Snowcone | 소형 현장 데이터 수집·이전이라는 과거 제품 맥락 |
| Snowball Edge Storage Optimized | 저장 중심 물리 이전 |
| Snowball Edge Compute Optimized | 지원 장비의 현장 컴퓨팅 |
| Snowmobile | 초대규모 물리 이전의 과거 서비스 맥락 |
| OpsHub | 지원 Snow 장비의 GUI 관리 도구 |

장비 용량·GPU·EC2 호환 기능·Lambda 지원은 장비 세대별로 다릅니다. 원문의 “80TB”, “8~14TB”, “10PB 이상이면 무조건 Snowmobile”을 일반 공식으로 외우지 않습니다.

### 네트워크 전송 시간 계산

대략적인 시간 = **데이터 바이트 수 × 8 ÷ 유효 비트/초**입니다. 예를 들어 100TB를 100Mbps로 보내면 이론적으로 약 93일이며 프로토콜·공유 회선·검증·재시도 때문에 더 걸릴 수 있습니다. “일주일 이상이면 무조건 장비”라는 규칙 대신 배송·복사·반입 시간과 현재 제공 여부를 비교합니다.

기본적인 이전 흐름은 장비 준비 → 로컬 데이터 복사·암호화 → 반송 → S3 반입·검증입니다. 보관 계층 전환은 S3 반입 후 Lifecycle 등 지원 절차를 사용합니다. 온라인 경로가 가능하면 DataSync·전용 연결·파트너 솔루션도 비교하세요.

## 문제 유형

1. 수백 TB와 낮은 대역폭, 촉박한 기한 → 먼저 전송 시간 계산, 현재 이용 가능한 물리 이전 옵션 검토.
2. 인터넷 없는 연구 현장에서 연산 → 지원되는 로컬 엣지 컴퓨팅 옵션.
3. 반입 후 장기 아카이브 → S3 반입과 수명 주기 정책 분리.
4. 과거 문제에서 Snow 장비명 제시 → 장비 역할은 이해하되 현재 구축 가능성과 구분.

## 빠른 복습표

| 단서 | 검토할 내용 |
|---|---|
| 오프라인·회선 부족 | 물리 이전 가능성과 현재 자격 |
| 엣지 연산 | 장비 세대·지원 기능 |
| Snowball Edge 신규 고객 | 제공 제한 확인 |
| S3 반입 후 아카이브 | Lifecycle |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/27-Snow-Family.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
