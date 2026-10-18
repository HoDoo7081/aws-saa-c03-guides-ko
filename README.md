# AWS SAA-C03 한국어 학습 가이드

**37개 주제를 한국어로 공부하는 요약·재구성판**입니다. 개념 설명, 서비스 비교, 문제 유형, 빠른 복습표를 담았습니다.

## 바로 시작

- **[추천 순서로 공부하기 → 한국어 학습 목차](topic-guides/00-README.md)**
- [37개 주제 공부 체크리스트](STUDY-CHECKLIST.md)
- [번역 범위와 원문 정정 사항](TRANSLATION-NOTES.md)
- [AWS 공식 SAA-C03 시험 가이드](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html)

## 퀴즈 웹앱 — 클라우드 한입

매일 8문제씩 공부하는 웹앱을 [실행용 HTML](cloud-bites.html)에 추가했습니다.

- 기본 111문제 + 사용자 제공 PDF 심화 1,094문제
- [단일 HTML 파일 내려받기](cloud-bites.html): 파일을 내려받아 브라우저에서 열기
- [전체 소스 ZIP 내려받기](cloud-bites-source.zip): 압축을 풀면 코드·테스트·실행 안내가 있습니다.
- PC·모바일 반응형, 오답 복습, XP, 기록 백업 지원
- 소스 실행: 압축 해제 후 `python3 -m http.server 5173 --directory cloud-bites/dist`
- **[휴대폰·PC에서 바로 학습하기](https://hodoo7081.github.io/aws-saa-c03-guides-ko/)** — 설치나 로그인 없이 접속
- 휴대폰 브라우저에서 홈 화면에 추가해 사용할 수 있습니다. 학습 기록은 기기·브라우저별로 저장됩니다.

심화 문제·수록 해설·이미지는 사용자 제공 PDF에서 변환했으며, 아래 가이드의 CC BY 4.0 라이선스 범위에 포함되지 않습니다. 소스 ZIP 안의 `SOURCE-NOTICES.md`를 확인하세요.

## AWS 서비스 도감

**[그림으로 서비스 이해하기 → 서비스 도감](https://hodoo7081.github.io/aws-saa-c03-guides-ko/aws-services.html)**

- 공식 SAA-C03 예시 목록 118개(중복 제외) + 보충 기능 18개
- 16개 분야의 역할·배치 위치·작동 흐름·선택 기준
- 웹 멀티 AZ, 서버리스, 데이터 레이크, 하이브리드, 보안·운영, 스토리지 그림 6개
- 서비스 검색·분야 필터·헷갈리는 서비스 비교 12개
- [전체 정리 문서와 Mermaid 그림](AWS-SAA-서비스-도감.md)

2026-10-18 공식 문서 확인 기준입니다. 공식 목록은 비포괄적이며 변경될 수 있어 모든 실제 출제 항목을 보장하지 않습니다.

## 이 자료의 범위

원작 [RonitSachdev/aws-saa-c03-guides](https://github.com/RonitSachdev/aws-saa-c03-guides)를 바탕으로 만든 비공식 한국어 학습 자료입니다. **문장별 전체 번역이 아닙니다.** 원문의 반복 설명·일부 수치·예제를 압축하고, 한국어로 설명을 다시 구성했습니다. 37개 주제 모두 다루며 개별 문서에서 고정 커밋의 영문 원문으로 이동할 수 있습니다.

원문의 S3·EBS 한도, API Gateway 타임아웃, Multi-AZ, DAX, KMS 인증 수준, 제공이 중단된 서비스 등은 별도 정정했습니다. 확인한 주요 변경의 근거는 정정 문서에 있습니다. 모든 서비스의 기능·할당량·리전·요금을 전수 검증한 자료는 아니므로, 세부 조건은 최신 공식 문서를 확인하세요.

## 학습 흐름

개념을 읽고 → 비교표로 선택 기준을 익히고 → 문제 유형에서 단서를 찾고 → 복습표를 가리고 답해 보세요. 모의고사 오답은 서비스 이름뿐 아니라 놓친 조건도 기록합니다.

## 출처와 라이선스

- 원작자: **RonitSachdev**
- 원본 저장소: https://github.com/RonitSachdev/aws-saa-c03-guides
- 기준 커밋: `a71e466f02702fe01a6fe414c0b49cb8a673d205`
- 한국어판 구성: **HoDoo7081**, AI 번역·편집 도움을 받아 작성
- 변경 내용: 한국어 번역·요약·재구성, 설명 보완, 일부 오류 정정, 체크리스트 추가
- 라이선스: **[Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/)**. 원본 [LICENSE](LICENSE)를 보존했으며, 한국어판도 같은 라이선스로 제공합니다.

원작자나 AWS의 승인·보증을 뜻하지 않습니다. 실제 시험 문항이나 합격을 보장하는 자료가 아닙니다.
