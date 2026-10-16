# 08. CloudFront와 Global Accelerator

## 개념 이해

CloudFront는 인기 책을 동네 서점에 미리 두는 방식이고, Global Accelerator는 본사까지 가는 전용 고속도로입니다. 전자는 **콘텐츠 캐싱**, 후자는 **네트워크 경로와 진입점**에 초점이 있습니다.

## 핵심 개념

| 구분 | CloudFront | Global Accelerator |
|---|---|---|
| 주 역할 | HTTP 콘텐츠 전송·캐싱 | AWS 글로벌 네트워크를 이용한 트래픽 가속 |
| 프로토콜 | HTTP·HTTPS | TCP·UDP |
| 캐시 | 있음. 동적 요청 가속도 가능 | 없음 |
| 대표 진입점 | 배포 도메인 | 표준 IPv4 가속기 기준 고정 Anycast IPv4 2개 |
| 대표 용도 | 웹·영상·API·다운로드 | 게임·VoIP·고정 IP·다중 리전 |

### CloudFront

원본(Origin)은 S3·ALB·HTTP 서버 등입니다. TTL을 늘리면 일반적으로 원본 요청이 감소하지만 최신성 요구를 확인해야 합니다. 캐시 무효화(Invalidation)는 조기 갱신이며 파일명 버전 관리도 비용·캐시 관리에 유용합니다.

- S3 REST 원본을 CloudFront로만 접근: **OAC + 버킷 정책**. OAI는 기존 방식이며 모든 문맥에서 무조건 오답은 아님.
- 파일 하나의 제한 접근: **서명 URL**.
- 여러 파일·유료 콘텐츠 묶음: **서명 쿠키**.
- 국가 허용·차단: **Geo Restriction**.
- CloudFront용 ACM 인증서: **us-east-1**.
- 민감한 필드만 엣지에서 암호화: **Field-level encryption**.
- 원본 그룹: 기본·보조 원본 간 장애 조치. 메서드·오류 코드 등 지원 조건 확인.
- Price class: 사용할 엣지 범위를 조절해 비용·지연 절충.

### Global Accelerator

사용자는 가까운 AWS 엣지로 접속하고 트래픽은 AWS 네트워크를 통해 정상 엔드포인트로 전달됩니다. 고정 IP를 유지하므로 DNS 주소 변경에 의존하지 않는 장애 조치를 구성할 수 있습니다. 그러나 “즉시·무중단·항상 30초”를 보장하는 것은 아닙니다. 상태 확인·연결·앱 복구 시간을 함께 봅니다.

### 방향과 프로토콜로 고르기

- 전 세계 사용자의 HTTP 콘텐츠 조회: CloudFront.
- 멀리 있는 사용자의 S3 대용량 업로드: S3 Transfer Acceleration 검토.
- TCP·UDP 앱의 글로벌 진입점: Global Accelerator.

## 문제 유형

1. 비공개 S3 콘텐츠를 전 세계에 낮은 지연으로 제공 → CloudFront + OAC.
2. 구독자가 영상 라이브러리 전체를 이용 → 서명 쿠키.
3. 글로벌 UDP 게임과 고정 IP 요구 → Global Accelerator.
4. CloudFront 원본 부하 감소 → 캐시 키·TTL·캐시 적중률 조정.
5. 특정 국가 배포 제한 → Geo Restriction.

## 빠른 복습표

| 단서 | 답 후보 |
|---|---|
| 웹·영상·캐시 | CloudFront |
| S3 우회 접근 차단 | OAC |
| 한 파일 / 여러 파일 | 서명 URL / 서명 쿠키 |
| TCP·UDP·고정 Anycast IP | Global Accelerator |
| 원본 장애 조치 | CloudFront origin group |
| CloudFront 인증서 리전 | us-east-1 |

---
[학습 목차](00-README.md) · [영문 원문](https://github.com/RonitSachdev/aws-saa-c03-guides/blob/a71e466f02702fe01a6fe414c0b49cb8a673d205/topic-guides/08-CloudFront-GlobalAccelerator.md)

원작: RonitSachdev · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). 한국어 번역·요약·재구성본이며, 원문의 반복 설명과 예시는 압축했습니다. 보완 사항은 [번역 및 정정 안내](../TRANSLATION-NOTES.md)를 참고하세요.
