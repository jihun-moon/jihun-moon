# 문지훈

대구대학교 컴퓨터소프트웨어학부 4학년이고, 2027년 2월 졸업예정입니다.

데이터를 받아서 쌓고, 모델을 돌리고, 화면까지 내보내는 구간을 처음부터 끝까지 세워 보는 걸 주로 합니다.
지금은 졸업작품 서버 한 대를 2026년 5월부터 상시로 굴리고 있는데, 새 기능을 만드는 시간보다 터진 걸 고치는 시간이 더 많습니다.

* 포트폴리오 **<https://jihun-moon.github.io/jihun-moon/>** (프로젝트를 한 화면에 정리해 뒀습니다)
* 기록 [Notion Knowledge Hub](https://www.notion.so/My-Knowledge-Hub-27772d9f979f80569662de9c2e49399d?source=copy_link) (과목별 노트와 프로젝트 상세)
* 메일 [jihun0948@naver.com](mailto:jihun0948@naver.com)
* 대구

<br/>

## 만든 것

### CHRONO (비공개 저장소, 서비스는 공개)

개인 투자자용 주식 퀀트 분석 플랫폼입니다. 2026학년도 SW종합설계 과제로 시작했습니다.
대구대학교 RISE사업단 4조, 팀장 이민성, 팀원 문지훈과 임창혁 3인 팀입니다.
Kafka·DB 서버와 Docker 배포, 수집 파이프라인, 신호 검증 체계를 제가 맡았고, 과목이 끝난 뒤에도 서버 운영을 이어 가고 있습니다.

서비스는 <https://chrono-ai.mooo.com> 에서 돌고 있습니다.

**인프라**

Oracle Cloud ARM 무료 티어 한 대(Ubuntu 22.04)에 Docker 컨테이너 18개를 올려서 24시간 운영합니다.

| 영역 | 컨테이너 |
|---|---|
| 서비스 | `team-frontend`, `team-api` |
| 데이터 | `team-producer`, `team-consumer`, `team-analyzer` |
| 저장소 | `team-postgres`, `team-neo4j`, `team-redis` |
| 메시지 | `team-kafka`, `team-zookeeper` |
| AI 운영 | `team-conclave`, `team-retrainer`, `team-refresher-embeddings`, `team-visual-auditor` |
| 인프라 | `team-caddy`, `team-bot`, `team-bot-presence`, `team-polymarket-alert-dispatcher` |

HTTPS 는 Caddy 로 붙였고 CI/CD 는 GitHub Actions 14개로 돌립니다.
CI, E2E, 부하 테스트, Lighthouse, 시크릿 스캔, SBOM 생성, Dependabot 자동 병합, 릴리스 태깅이 들어 있습니다.

**데이터**

KIS, DART, FRED, KOSIS, KIPRIS, NASA FIRMS 위성 열화상, 선박 AIS, 네이버뉴스, Polymarket 등에서
긁어옵니다. Kafka 로 넘기고 PostgreSQL 15 에 쌓습니다.
종목 관계는 Neo4j 5 에, 캐시는 Redis 7 에 둡니다. 테이블 227개, 적재 데이터 18GB 입니다.

**모델**

| 모델 | 하는 일 |
|---|---|
| Transformer | 1·5·20일 기대수익률 예측 (13 features × 60일) |
| XGBoost | 5일 방향성 확률 |
| Regime HMM | 시장 국면 5단계 분류 |
| KR-FinBERT | 한국어 뉴스와 공시 감성 |
| IsolationForest + LogReg | 이상 거래 탐지 |

Transformer 는 서버 CPU 로 학습하기엔 버거워서 Colab GPU 에서 학습하고, ONNX 로 바꿔 서버의 ONNX Runtime 으로 추론합니다.
LLM 은 한 곳이 죽어도 멈추지 않게 4단계 폴백으로 묶었습니다.
기본은 Gemini 2.5 Flash Lite 무료 키 여러 개를 돌려 쓰고, 막히면 Mistral, Cohere 순으로 넘어갑니다. 마지막은 룰 기반입니다. 유료인 Claude Haiku 는 2026년 6월부터 기본으로 꺼 두었습니다.

**여기서 제일 크게 배운 것**

백테스트 숫자는 좋게 나왔습니다.
그런데 검증 장치를 붙이고 다시 재니 남는 게 없었습니다.

- Walk-Forward purge/embargo 로 학습과 검증 경계의 미래 정보 누수를 막고
- Deflated Sharpe Ratio 게이트로 통계적으로 유의하지 않으면 검증완료 대신 판정불가로 표시하고
- 검증 안 된 신호는 실험적 라벨을 강제로 달았습니다

그 결과 국내주식 판정칸 231개 중 표본 기준을 넘은 게 21칸이었습니다. 다중비교 보정을 통과한 8칸은 전부 코스피보다 못한 쪽이었고, 비용을 빼고도 양수인 칸은 0이었습니다.
코인 선물 쪽 개인 연구에서도 축 45개를 다 넣은 모델이 라벨을 무작위로 섞은 대조군을 못 넘겼습니다.

그래서 좋은 숫자를 앞에 내거는 대신 판정불가를 그대로 보여주게 만들었습니다.
지표가 좋아 보이면 일단 라벨을 섞어서 다시 돌려 보는 습관이 여기서 생겼습니다.

### [PII-Guardian](https://github.com/jihun-moon/PII-Guardian)

웹 페이지를 긁어서 개인정보 노출을 찾는 모델을 만들고 네이버 클라우드에 올린 프로젝트입니다.

* 크롤러 수집 → HyperCLOVA X 로 정답 라벨 자동 생성 → NER 모델 재학습으로 한 바퀴가 돌게 짰습니다. 2026년 1월부터는 멈춰 있습니다
* 라벨링을 사람이 하면 못 끝낸다고 보고 LLM 에게 맡긴 게 핵심입니다
* GitHub Actions 에서 NCP 서버로 SSH 배포
* 학습 서버 요금을 아끼려고 평소엔 인스턴스를 내려 두고, 필요한 시간에만 NCP API 로 깨우는 워크플로를 따로 뒀습니다
* 모델을 채점하려고 개인정보가 일부러 박힌 테스트 사이트를 직접 만들었습니다. 정답을 아는 페이지가 없으면 재현율을 구할 수 없어서입니다. 다만 재현율과 정밀도를 숫자로 정리해 두지는 못했습니다

### [im-bank-n8n-agent](https://github.com/jihun-moon/im-bank-n8n-agent)

금융 보안 로그에서 개인정보를 지우고 위험도를 매기는 파이프라인입니다. 팀 AIM 5인 중 n8n 파이프라인과 Express 백엔드, SQLite 스키마를 맡았습니다.
대구대학교 AI·SW Agent 해커톤 경진대회(2025 대구울산경북 AI Agent 해커톤) 출품작이고 최우수상을 받았습니다.

* n8n 워크플로 노드 43개(그중 21개는 설명 노트), 백엔드는 Express 5 + SQLite, 대시보드는 SSE 로 실시간 갱신
* 위험도 판정에 Upstage Solar Pro 2, 실패하면 Gemini 로 넘어갑니다
* 26시간 돌려서 12,458건을 처리했고 그 기록을 저장소에 같이 넣어 뒀습니다
* 로그의 43.3%가 볼 필요 없는 것이었습니다. 소스 하나가 통째로 노이즈였습니다
* p50 은 0.2초인데 p99 가 182초였습니다. 워커를 하나만 돌려서 뒤가 밀립니다

### 그 밖에

| 저장소 | 무엇 |
|---|---|
| [battle-rogue](https://github.com/jihun-moon/battle-rogue) | 언리얼 엔진 5 1대1 온라인 대전 게임. 데디케이티드 서버 타깃을 따로 만들어 붙였습니다 |
| [opengl-earthquake-simulation](https://github.com/jihun-moon/opengl-earthquake-simulation) | C++ 와 OpenGL 로 학교 건물 2층을 통째로 그린 지진 대피 시뮬레이터. 2,047줄. 3인 팀에서 1인칭 카메라와 대피 경로 재생을 맡았습니다 |
| [mobile-doctor-app](https://github.com/jihun-moon/mobile-doctor-app) | 위치 기반 병원 검색과 복약 관리 안드로이드 앱 (Java) |
| [Edu-Bridge-Library](https://github.com/jihun-moon/Edu-Bridge-Library) | 2025 도서관 데이터 활용 공모전. 장서 373,443행 중 대출 상위 100종을 초등 교육과정에 붙여 봤고 11종은 분류에 실패했습니다 |
| [Lecture-Summarizer-AI](https://github.com/jihun-moon/Lecture-Summarizer-AI) | 온라인 강의 소리를 스테레오 믹스로 받아 Whisper 로 받아쓰는 도구. 이름과 달리 요약은 아직 없습니다. 윈도우 전용 |
| [eth-autotrade-bot](https://github.com/jihun-moon/eth-autotrade-bot) | CHRONO 이전에 만든 자동매매 봇. 후보 전략을 섀도로 먼저 돌려 보고 교체하는 구조. 실전 투입 전에 멈췄습니다 |

<br/>

## 네이버 클라우드 실습

대구대학교와 네이버 클라우드가 같이 연 DANCE 교육 과정 1기를 수료했습니다.
2025년 10월 29일부터 11월 21일까지 12일차 과정이고, 이론보다 콘솔 실습 비중이 큽니다.

| 영역 | 직접 해 본 것 |
|---|---|
| 네트워크 | VPC, Subnet, Route Table, **다른 계정과 VPC Peering**, NAT Gateway, NACL, IPsec VPN |
| 로드밸런싱 | NLB, ALB, 클라이언트 IP 전달 |
| 스토리지 | 서버 볼륨 증설과 축소, 서버 두 대 NAS 연결, Object Storage 마운트, 멀티 디스크 단일 볼륨 |
| DB | Cloud DB 생성, 외부 클라이언트 접속, Master/Standby |
| 운영 | Cloud Insight, Cloud Log Analytics, ActiveTracer 로그를 Object Storage 로 적재 |
| 보안 | System Security Checker, Web Security Checker, ACG 인바운드 규칙 |
| 기타 | AutoScaling, Backup/Restore, Rsync, TTS/STT, Papago, Chatbot API |

마지막 미니 프로젝트로 2-Tier 구성을 직접 세웠습니다. 졸업생 관리 시스템입니다.

- Public Subnet 에 ALB 만 두고 외부 진입을 하나로 모았습니다
- 웹 서버 두 대는 Private Subnet 에 두고 서로 다른 AZ 에 배치했습니다
- DB 는 Master 와 Standby 로 이중화하고 매일 02:00 백업, 14일 보관으로 잡았습니다
- ACG 인바운드 규칙으로 3306 접근을 웹 서버 대역으로만 열었습니다

ALB 가 두 서버로 실제로 나눠 보내는지 확인하려고 페이지 제목에 서버 이름을 찍게 했습니다.
두 서버가 같은 DB 를 보는지 데이터 동기화까지 확인했습니다.

과정 끝에 NCA 자격증을 땄습니다.

<br/>

## 서버 굴리면서 겪은 것

혼자 운영하면 장애를 남이 대신 찾아 주지 않습니다. 기억에 남는 세 건입니다.

**크론이 12줄에서 4줄로 잘려 있었다.**
8월에 점검하다가 암호화 백업이 0개인 걸 보고 따라가 봤더니 crontab 이 4줄만 남아 있었습니다.
더 나빴던 건 부팅할 때마다 크론을 되살리는 스크립트가 있었는데, 되살릴 목록을 이미 잘린 4줄에서 떠 왔다는 점입니다.
재부팅을 몇 번 해도 백업 줄은 돌아오지 않는 구조였습니다. 두 달 동안 그렇게 돌았습니다.
그 목록에 백업, 헬스체크, 주간 감사, 워치독 줄을 다시 넣어 4줄을 10줄로 늘렸습니다.
백업은 있는 것보다 맞는 것을 넣었는지가 중요하다는 걸 여기서 배웠습니다.

**API 가 73분 멈췄는데 아무도 안 움직였다.**
감지는 되고 있었고 로그에도 남아 있었습니다. 그런데 재기동을 맡은 워치독이
두 달 전부터 같이 죽어 있어서 아무 일도 일어나지 않았습니다.
재기동할 때 디스코드로 알리는 것도 그 워치독 몫이라 알림도 없었습니다.
워치독을 5분 주기 크론으로 되살리고 부팅 때 복원하는 목록에도 넣었습니다. API 가 왜 얼었는지는 스택을 못 떠서 아직 모릅니다.

**한 줄 때문에 천 줄이 사라졌다.**
`executemany` 로 1,000행씩 넣고 있었는데 중간에 한 행이 제약에 걸리면 배치 전체가 롤백됐습니다.
Kafka 오프셋은 자동 커밋이라 다시 받지도 못해서 그 배치가 통째로 빠졌습니다. 로그로 세어 보니 16일 동안 87번, 약 7만 7천 행이었습니다.
실패한 배치는 행 단위로 다시 넣고, 오프셋은 적재가 끝난 뒤 직접 커밋하도록 바꿨습니다. 이미 빠진 틱은 되받을 경로가 없어 못 채웠습니다.

<br/>

## 쓴 것

써 본 것만 적었습니다. 괄호 안이 실제로 쓴 곳입니다.

* **언어** Python (CHRONO, PII-Guardian), TypeScript (CHRONO 프런트), Java (안드로이드), C++ (OpenGL), SQL
* **백엔드** FastAPI, Express, PostgreSQL, Neo4j, Redis, SQLite, Kafka
* **프런트** Next.js, React, Tailwind CSS
* **ML** PyTorch, scikit-learn, XGBoost, ONNX Runtime, HuggingFace NER 파인튜닝, KR-FinBERT, Whisper
* **LLM** HyperCLOVA X (자동 라벨링), Upstage Solar Pro 2 (로그 판정), Claude, Gemini (폴백 체인)
* **인프라** Docker, Docker Compose, Caddy, GitHub Actions, 네이버 클라우드 플랫폼, Oracle Cloud, Linux, cron, systemd
* **운영** Sentry, 자체 헬스체크와 알림 디스패처, n8n, PM2

쿠버네티스와 테라폼은 아직 실제로 운영해 본 적이 없어서 뺐습니다.

<br/>

## 자격 · 수상

* NCA (Naver Cloud Certified Associate)
* 대구대학교 AI·SW Agent 해커톤 경진대회 최우수상 (2025 대구울산경북 AI Agent 해커톤, 팀 AIM)
* 대구대학교 네이버 클라우드 DANCE 교육 과정 1기 수료
* 2025 도서관 데이터 활용 공모전 참가
* 업스테이지 MABC 2026 예선 참가 (팀 이문박 3인)

<br/>

## 학교

대구대학교 컴퓨터소프트웨어학부. 2026년 8월 기준 4학년 1학기까지 이수했습니다.
전공 평점 3.80 / 4.50, 전체 평점 3.85 / 4.50, 취득 학점 128.
수업 과제와 실습은 [daegu-univ-cs](https://github.com/jihun-moon/daegu-univ-cs) 에 모아 두고 있습니다.

<br/>

프로젝트를 한눈에 보려면 [포트폴리오 페이지](https://jihun-moon.github.io/jihun-moon/)가 편합니다.

<p align="center">
  <img alt="contribution snake" src="https://raw.githubusercontent.com/jihun-moon/jihun-moon/output/github-contribution-grid-snake.svg" />
</p>
