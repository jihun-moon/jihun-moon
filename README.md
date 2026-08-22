# 문지훈

대구대학교 컴퓨터소프트웨어학부 4학년입니다.

데이터를 받아서 쌓고, 모델을 돌리고, 화면까지 내보내는 구간을 처음부터 끝까지 직접 세워 보는 걸
주로 하고 있습니다. 지금은 개인 서버 한 대에서 프로젝트 하나를 넉 달째 상시로 굴리는 중이고,
그 과정에서 만난 장애를 고치는 일이 코드를 새로 쓰는 일보다 많습니다.

* 메일 [jihun0948@naver.com](mailto:jihun0948@naver.com)
* 기록 [Notion Knowledge Hub](https://www.notion.so/My-Knowledge-Hub-27772d9f979f80569662de9c2e49399d?source=copy_link)
* 대구

<br/>

## 만든 것

### CHRONO (비공개, 졸업작품)

국내 주식과 코인 데이터를 모아서 분석하고 매매 판단까지 붙이는 개인 프로젝트입니다.
2026년 5월에 시작해서 지금까지 서버 한 대에서 계속 돌고 있습니다.

* FastAPI + PostgreSQL + Redis, 프런트는 Next.js 16 / React 19
* Docker Compose 로 서비스 20여 개를 한 박스에 올려서 운영
* 국내 상장 3,950종목 일봉, 나스닥 4,460종목, 코인 22심볼 3거래소, 뉴스 74만 건을 수집
* 테이블 228개, 적재 데이터 17GB
* 추론은 ONNX Runtime, 학습은 PyTorch CPU 와 XGBoost
* GitHub Actions 13개로 CI, E2E, 부하 테스트, Lighthouse, 시크릿 스캔, SBOM 생성, 릴리스 태깅을 자동화

만들면서 얻은 결론 중에 제일 큰 건 **가격의 방향은 예측되지 않는다**는 것이었습니다.
축 45개를 다 넣어도 라벨을 무작위로 섞은 대조군 성능을 넘지 못했습니다.
그래서 방향을 맞히는 쪽을 접고, 손절과 익절을 어디에 두느냐로 기대값이 달라지는 부분만 남겼습니다.
지표가 좋아 보이면 일단 라벨을 섞어서 다시 돌려 보는 습관이 여기서 생겼습니다.

### PII-Guardian (비공개)

웹 페이지를 긁어서 개인정보 노출을 찾아내는 모델을 만들고, 네이버 클라우드에 올려 둔 프로젝트입니다.

* 크롤러가 수집 → HyperCLOVA X 로 정답 라벨을 자동 생성 → NER 모델 재학습, 이렇게 한 바퀴가 돌아갑니다
* 라벨링을 사람이 하면 못 끝낸다고 보고 LLM 에게 맡긴 게 이 프로젝트의 핵심입니다
* GitHub Actions 에서 NCP 서버로 SSH 배포
* 학습 서버 요금을 줄이려고 서버를 내려 두고, 필요한 시간에만 NCP API 로 인스턴스를 깨우는 워크플로를 따로 뒀습니다
* 모델을 채점하려고 개인정보가 일부러 박힌 테스트 사이트를 직접 만들어서 재현율을 쟀습니다

### [im-bank-n8n-agent](https://github.com/jihun-moon/im-bank-n8n-agent)

금융 보안 로그를 받아서 개인정보를 지우고 위험도를 매기는 파이프라인입니다.
2025 AI Agent 해커톤 출품작이고 결선 최우수상을 받았습니다.

* n8n 워크플로 7개, 백엔드는 Express + SQLite, 대시보드는 SSE 로 실시간 갱신
* 위험도 판정에 Upstage Solar Pro 2 사용
* 로그를 큐에 먼저 넣고 워커가 하나씩 꺼내 가는 구조로 바꾼 뒤에 유실이 없어졌습니다
* 개인정보가 남아 있는 로그는 학습에서 빼고, 마스킹된 것만 KB 에 넣도록 나눴습니다

### 그 밖에

| 저장소 | 무엇 |
|---|---|
| [battle-rogue](https://github.com/jihun-moon/battle-rogue) | 언리얼 엔진 5 로 만든 1대1 온라인 대전 게임. 데디케이티드 서버 구성 |
| [opengl-earthquake-simulation](https://github.com/jihun-moon/opengl-earthquake-simulation) | C++ 와 OpenGL 로 만든 지진 대피 훈련 3D 시뮬레이터 |
| [mobile-doctor-app](https://github.com/jihun-moon/mobile-doctor-app) | 위치 기반 병원 검색과 진료 기록 관리 안드로이드 앱 (Java) |
| [Edu-Bridge-Library](https://github.com/jihun-moon/Edu-Bridge-Library) | 2025 도서관 데이터 활용 공모전. 초등 교육과정과 대구 도서관 데이터를 붙인 도서 추천 제안 |
| Lecture-Summarizer-AI (비공개) | LMS 강의를 스테레오 믹스로 녹음해서 Whisper 로 받아쓰고 요약 |
| eth-autotrade-bot (비공개) | CHRONO 이전에 만든 자동매매 봇. 전략 후보를 만들고 섀도로 돌려 본 뒤 교체하는 구조 |

<br/>

## 서버 굴리면서 겪은 것

혼자 운영하다 보니 장애를 남이 대신 찾아 주지 않습니다. 기억에 남는 세 건입니다.

**크론이 12줄에서 4줄로 잘려 있었다.**
어느 날 수집이 멈춰 있길래 봤더니 crontab 이 4줄만 남아 있었습니다.
더 나빴던 건 백업에서 되살리는 자동 복원 스크립트가 있었는데, 그게 잘린 쪽을 정상으로 알고
매일 덮어쓰고 있었다는 점입니다. 두 달 동안 그렇게 돌았습니다.
복원 전에 줄 수를 먼저 비교하고, 줄어들었으면 덮어쓰지 않고 알림만 보내도록 고쳤습니다.
**백업은 있는 것보다 맞는 것을 넣었는지가 중요하다**는 걸 여기서 배웠습니다.

**API 가 73분 멈췄는데 아무도 안 움직였다.**
감지는 되고 있었습니다. 로그에도 남아 있었습니다. 그런데 재기동을 맡은 워치독이
두 달 전부터 같이 죽어 있어서 아무 일도 일어나지 않았습니다.
알림 채널 조건이 error 레벨만 보내게 돼 있어서 사람한테도 안 갔습니다.
감시하는 쪽이 살아 있는지 확인하는 절차를 따로 두고, 알림 조건을 다시 잡았습니다.

**한 줄 때문에 천 줄이 사라졌다.**
`executemany` 로 1,000행씩 넣고 있었는데, 중간에 한 행이 제약에 걸리면 배치 전체가 롤백됐습니다.
auto-commit 이라 되돌릴 것도 없이 그냥 그날 데이터가 없었습니다.
실패한 배치는 행 단위로 다시 넣도록 바꾸고, 빠진 구간을 찾아서 다시 채웠습니다.

<br/>

## 쓴 것

써 본 것만 적었습니다. 괄호 안은 실제로 쓴 곳입니다.

* **언어** Python (CHRONO, PII-Guardian), TypeScript (CHRONO 프런트), Java (안드로이드 앱), C++ (OpenGL), SQL
* **백엔드** FastAPI, Express, PostgreSQL, Redis, SQLite
* **프런트** Next.js, React, Tailwind CSS
* **ML** PyTorch, scikit-learn, XGBoost, ONNX Runtime, HuggingFace NER 파인튜닝, OpenAI Whisper
* **LLM** HyperCLOVA X (자동 라벨링), Upstage Solar Pro 2 (로그 위험도 판정)
* **인프라** Docker, Docker Compose, GitHub Actions, 네이버 클라우드 플랫폼, Linux, cron, systemd
* **운영** Sentry, 자체 헬스체크와 알림 디스패처, n8n

쿠버네티스와 테라폼은 아직 실제로 운영해 본 적이 없어서 뺐습니다.

<br/>

## 자격 · 수상

* NCA (Naver Cloud Platform Certified Associate)
* 2025 AI Agent 해커톤 결선 최우수상 (팀 AIM)
* 2025 도서관 데이터 활용 공모전 참가

<br/>

## 학교

대구대학교 컴퓨터소프트웨어학부, 2026년 8월 기준 4학년 1학기까지 이수.
전공 평점 3.80 / 4.50, 전체 평점 3.85 / 4.50, 취득 학점 128.
수업 과제와 실습은 [daegu-univ-cs](https://github.com/jihun-moon/daegu-univ-cs) 에 모아 두고 있습니다.

<br/>

<p align="center">
  <img alt="contribution snake" src="https://raw.githubusercontent.com/jihun-moon/jihun-moon/output/github-contribution-grid-snake.svg" />
</p>
