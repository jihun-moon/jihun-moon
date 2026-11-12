# 문지훈

**만든 걸 직접 서버에 올려 끝까지 굴리고, 좋은 숫자부터 의심하는 개발자입니다.**
희망 직무는 데이터 파이프라인 구축과 서버·클라우드 운영입니다.

대구대학교 컴퓨터정보공학부 컴퓨터소프트웨어전공 4학년이고, 2027년 2월 졸업예정입니다. 육군 병장으로 만기전역했습니다(2022.04.19 ~ 2023.10.18).

데이터를 받아서 쌓고, 모델을 돌리고, 화면까지 내보내는 구간을 처음부터 끝까지 세워 보는 걸 주로 합니다.
지표가 좋아 보이면 일단 라벨을 섞어서 다시 돌려 봅니다.

* 포트폴리오 <https://jihun-moon.github.io/jihun-moon/>
* 기록 [Notion Knowledge Hub](https://www.notion.so/My-Knowledge-Hub-27772d9f979f80569662de9c2e49399d?source=copy_link)
* 메일 [jihun0948@naver.com](mailto:jihun0948@naver.com)

<br/>

## 한눈에

* **서버를 다섯 달 굴렸습니다.** CHRONO 를 Oracle Cloud ARM 한 대에 Docker 로 올려 2026년 5월부터 10월 공학제 전시까지 운영했습니다. 크론이 12줄에서 4줄로 잘린 일, API 가 73분 멈췄는데 워치독이 죽어 있던 일, 백업이 같은 디스크에만 있던 일, 배치 롤백으로 약 7만 7천 행이 빠진 일, 이렇게 사고 네 건을 원인과 고친 방법까지 [노션 CHRONO 페이지](https://www.notion.so/3c472d9f979f815daf89d32a3b515d15)에 적어 두었습니다.
* **좋은 숫자를 의심하는 장치를 만들었습니다.** 신호 판정 원장의 판정칸 231개 중 거래비용을 빼고도 양수인 칸은 0개였고, 그 결과를 첫 화면에 그대로 띄웠습니다. 이 작품으로 캡스톤디자인 공모전 우수상을 받았습니다.
* **개인정보는 LLM 앞에서 지웁니다.** im-bank-n8n-agent 에서 26시간 구동 기록 12,458건을 커밋해 두어 README 의 숫자를 누구나 다시 계산할 수 있게 했습니다. 해커톤 최우수상 작품입니다.

<br/>

## 프로젝트

| 프로젝트 | 무엇 |
|---|---|
| **CHRONO** (비공개 저장소, 서비스는 2026-10 전시 뒤 내림) | 개인 투자자용 주식 퀀트 분석 플랫폼. RISE 연계 SW종합설계 3인 팀 졸업작품이고, 저는 클라우드·인프라와 서버 운영을 맡았습니다. 구조와 화면은 [포트폴리오](https://jihun-moon.github.io/jihun-moon/#projects)에 있습니다 |
| [im-bank-n8n-agent](https://github.com/jihun-moon/im-bank-n8n-agent) | 금융 보안 로그의 개인정보를 지우고 위험도를 매기는 n8n 파이프라인. 5인 팀에서 파이프라인과 백엔드를 맡았습니다 |
| [PII-Guardian](https://github.com/jihun-moon/PII-Guardian) | 웹 페이지의 개인정보 노출을 찾는 NER 모델. HyperCLOVA X 로 라벨을 만들고 네이버 클라우드에 올렸습니다 |
| [du-radar](https://github.com/jihun-moon/du-radar) | 대구대 경산캠퍼스 지도 위에 공지, 편의시설, 제보, 운동장 예약을 핀으로 모은 웹앱. 2026 바이브코딩 해커톤 3인 팀 |

<details>
<summary>그 밖에 만든 것 7개</summary>

| 프로젝트 | 무엇 |
|---|---|
| [battle-rogue](https://github.com/jihun-moon/battle-rogue) | 언리얼 엔진 5 1대1 온라인 대전 게임. 2인 팀, 데디케이티드 서버 빌드 |
| [eth-autotrade-bot](https://github.com/jihun-moon/eth-autotrade-bot) | CHRONO 이전에 만든 자동매매 봇. 실전 투입 전에 멈췄습니다 |
| [opengl-earthquake-simulation](https://github.com/jihun-moon/opengl-earthquake-simulation) | C++ 와 OpenGL 로 만든 지진 대피 시뮬레이터. 3인 팀 |
| [mobile-doctor-app](https://github.com/jihun-moon/mobile-doctor-app) | 병원·약 검색과 진료 이력을 넣은 안드로이드 앱. 2인 팀 |
| [Edu-Bridge-Library](https://github.com/jihun-moon/Edu-Bridge-Library) | 도서관 대출 데이터를 초등 교육과정에 붙여 본 공모전 출품작 |
| [Lecture-Summarizer-AI](https://github.com/jihun-moon/Lecture-Summarizer-AI) | 강의 소리를 Whisper 로 받아쓰는 윈도우 도구 |
| [daegu-univ-cs](https://github.com/jihun-moon/daegu-univ-cs) | 수업 과제와 실습 모음 |

</details>

구조, 수치, 안 된 것은 각 저장소 README 와 노션에 적어 두었습니다.

<br/>

## 기술

어디에 썼는지와 같이 적었습니다.

* **운영해 본 것** Ubuntu 서버(Oracle Cloud ARM) 5개월 상시 운영. Docker Compose, Caddy(HTTPS), cron 으로 PostgreSQL 매일 덤프 → gpg 암호화 → OCI Object Storage 보관, GitHub Actions
* **직접 만든 것** Python(FastAPI) 백엔드, Kafka 를 거쳐 PostgreSQL 에 쌓는 적재 경로(Kafka 서버는 이민성과 같이 맡음), XGBoost·PyTorch 학습과 ONNX 추론, n8n·Express 로그 처리 파이프라인
* **클라우드 교육** 네이버 클라우드 DANCE 1기에서 VPC, 로드밸런서, Cloud DB 이중화를 콘솔로 구성, NCA 취득
* **배우는 중** 대구대 산학협력단 부트캠프(2026.07 ~ 2027.01)에서 Java, Spring Boot, JPA, Thymeleaf, Oracle Database, MySQL

쿠버네티스와 테라폼은 아직 실제로 운영해 본 적이 없어서 뺐습니다.

<br/>

## 수상 · 자격

* 대구대학교 AI·SW 캡스톤디자인 공모전 우수상 (2026, 팀 차트 정복단, CHRONO)
* 대구대학교 AI·SW Agent 해커톤 경진대회 최우수상 (2025, 팀 AIM)
* NCA (Naver Cloud Certified Associate)
* 대구대학교 네이버 클라우드 DANCE 교육 과정 1기 수료

## 참가

* 대구대학교 바이브코딩 해커톤 (2026, du-radar)
* 업스테이지 MABC 2026 예선 (팀 이문박)
* 도서관 데이터 활용 공모전 (2025, Edu-Bridge-Library)

<br/>

<p align="center">
  <img alt="contribution snake" src="https://raw.githubusercontent.com/jihun-moon/jihun-moon/output/github-contribution-grid-snake.svg" />
</p>
