# 🛠 GitHub 셋업 체크리스트 — 마무리 가이드

자동화로 처리한 작업 + 사용자가 **직접** 해야 하는 작업을 모두 정리했습니다.

---

## ✅ 이미 자동으로 완료된 것

### 프로필 리포 (`jihun-moon/jihun-moon`)
- [x] README 전면 개편 — 시니어 톤, Featured Work 표, Engineering Principles, GitHub Pages 안내
- [x] GitHub 데코레이션 — Profile views, Streak, Trophy, Snake animation
- [x] Snake animation GitHub Actions workflow (.github/workflows/snake.yml, daily cron)
- [x] Discord 알림 workflow scaffold (`DISCORD_WEBHOOK` secret 만 추가하면 작동)
- [x] `/docs/` 단일 페이지 포트폴리오 (Tokyo Night 톤, 반응형)
- [x] Discussions 활성화

### 7개 공개 리포 (description / homepage / topics)
- [x] `im-bank-n8n-agent`, `opengl-earthquake-simulation`, `mobile-doctor-app`, `battle-rogue`
- [x] `daegu-univ-cs`, `coding-test-practice`, `Edu-Bridge-Library`
- [x] 모두 Notion 포트폴리오로 homepage 연결, 검색 친화적인 topics 부여

### 모든 리포 위생 PRs
- [x] **showcase 4개**: LICENSE + SECURITY.md + issue/PR 템플릿 + Dependabot
- [x] **archive 3개**: LICENSE
- [x] `im-bank-n8n-agent`: CI workflow (백엔드/프론트엔드 lint/build) + README 배지 + Discord notify
- [x] showcase 4개: README 전면 개편 (hero / problem / architecture / stack / build / deep-dive)

### 일관 라벨 (8개 리포)
- [x] `priority/p0~p3`, `type/{feat,fix,docs,refactor,chore,test}`, `area/{backend,frontend,infra,ml}`, `status/{blocked,in-review,wip}`

### Merge / 브랜치 정책 (8개 리포)
- [x] Squash merge 기본값
- [x] Merge commit 비활성화
- [x] 머지 후 브랜치 자동 삭제
- [x] Auto-merge 활성화

### Stars / 큐레이션
- [x] 30+ reference repos star (MLOps · Quant · LLM · FastAPI · ONNX · Observability · Frontend)

### Achievements 트리거
- [x] **Quickdraw** — 이슈 #2 53초 만에 close
- [x] **Heart On Your Sleeve** — 모든 PR/issue 에 emoji 반응 16건
- [x] **Pull Shark** — PR 머지 시 자동 (현재 10개 대기 중)

### v1.0.0 GitHub Releases
- [x] `im-bank-n8n-agent`, `opengl-earthquake-simulation`, `mobile-doctor-app`, `battle-rogue` — 모두 풍성한 릴리즈 노트와 함께

---

## 🙋 사용자가 직접 해야 하는 것 (제 권한 밖)

### 1. GitHub 토큰 scope 확장 (가장 중요)

`gh` CLI 토큰에 `user` scope 가 없어 아래 작업이 막혔습니다. 한 번만 다음 명령 실행:

```bash
gh auth refresh -h github.com -s user,workflow,admin:repo_hook
```

이걸 하면 다음이 가능해집니다:
- ✏️ Bio 자동 변경
- 🔗 Notion 등 social account 자동 추가
- 📌 Pinned repos 변경 (현재는 GraphQL 공개 mutation 이 사라져서 결국 web UI 만 가능 — 아래 항목 참고)

### 2. Pinned Repositories 재구성 (web UI 필수)

현재 pinned: `battle-rogue`, `mobile-doctor-app`, `opengl-earthquake-simulation`, `Edu-Bridge-Library`, `daegu-univ-cs`, `coding-test-practice`

**추천 순서**: 강한 인상 → 약한 순으로
1. `im-bank-n8n-agent` ← **반드시 추가** (가장 강한 공개 작업물)
2. `opengl-earthquake-simulation`
3. `battle-rogue`
4. `mobile-doctor-app`
5. `daegu-univ-cs`
6. `Edu-Bridge-Library` (또는 `coding-test-practice`)

**어떻게:** [github.com/jihun-moon](https://github.com/jihun-moon) → Customize your pins → 위 순서대로 체크.

### 3. Profile bio 수정

현재: `🚀 Aspiring MLOps Engineer | ☁️ Cloud Native | 🐍 Python`

추천 (160자 이하):
```
ML/Data Systems Engineer · Building CHRONO (quant) & PII-Guardian (LLM ops) · FastAPI · PyTorch · ONNX
```

GitHub → Settings → Profile → Bio.

### 4. Profile README 의 PR #1 머지

PR: https://github.com/jihun-moon/jihun-moon/pull/1

**렌더링 미리보기 보고 머지** 하시면:
- 새 README + 데코레이션이 프로필에 즉시 반영
- `/docs/` 디렉토리가 main 으로 이동 → GitHub Pages 활성화 가능 (다음 항목)

### 5. GitHub Pages 활성화 (PR #1 머지 후)

1. https://github.com/jihun-moon/jihun-moon/settings/pages 접속
2. **Source**: Deploy from a branch
3. **Branch**: `main` / `/docs`
4. Save

URL: **https://jihun-moon.github.io/jihun-moon/** (3~5분 후 활성화)

### 6. 9개 Hygiene/License PR 머지

각 PR 의 Files Changed 보고 OK 면 squash-merge:

- https://github.com/jihun-moon/jihun-moon/pull/1 (프로필 + 포트폴리오)
- https://github.com/jihun-moon/im-bank-n8n-agent/pull/1
- https://github.com/jihun-moon/opengl-earthquake-simulation/pull/1
- https://github.com/jihun-moon/mobile-doctor-app/pull/1
- https://github.com/jihun-moon/battle-rogue/pull/1
- https://github.com/jihun-moon/daegu-univ-cs/pull/1
- https://github.com/jihun-moon/coding-test-practice/pull/1
- https://github.com/jihun-moon/Edu-Bridge-Library/pull/1

> **YOLO 업적 트리거**: review 없이 머지하시면 됩니다 (혼자 작업한 PR 들이라 자연스러움).
> **Pull Shark 업적**: 머지마다 카운트 +1, 8개 머지 시 Bronze (x2) 달성 후 다음 티어로.

### 7. Discord 연동 (선택)

원하는 Discord 서버 채널 → 서버 설정 → 통합 → 웹훅 → 새 웹훅 → URL 복사.

각 리포 → Settings → Secrets and variables → Actions → New repository secret:
- Name: `DISCORD_WEBHOOK`
- Secret: 복사한 URL

설정한 리포: `jihun-moon`, `im-bank-n8n-agent`, `opengl-earthquake-simulation`, `mobile-doctor-app`, `battle-rogue` (워크플로우 이미 푸시됨, secret 만 추가하면 작동).

### 8. Notion 연동 (선택)

Notion 은 통합 토큰 발급 + 페이지 권한 부여가 필요해 자동화 못 했습니다. 직접 하시려면:
- Notion API 토큰 발급: https://www.notion.so/my-integrations
- 각 GitHub 리포 README 의 Notion 링크는 이미 wired 되어 있어 별도 자동화 없이도 양방향 navigability 확보됨.

### 9. Star Lists 생성 (web UI 필수)

GraphQL API 가 create 를 지원 안 해 직접 하셔야 합니다. https://github.com/jihun-moon?tab=stars

추천 리스트:
- **MLOps Reference** — `mlflow`, `kserve`, `BentoML`, `ray`, `feast`, `great_expectations`
- **Quant & Trading** — `qlib`, `zipline`, `yfinance`, `machine-learning-for-trading`
- **LLM Agents** — `langchain`, `autogen`, `n8n`, `openai-python`, `anthropic-sdk-python`
- **My Stack** — `fastapi`, `pydantic`, `next.js`, `tailwindcss`, `onnxruntime`, `timescaledb`
- **Observability** — `prometheus`, `grafana`, `sentry`

이미 모두 star 되어 있으니 리스트로 묶기만 하면 됩니다.

### 10. Achievements: Galaxy Brain (수동 권장)

Discussions Q&A 에 누군가 답변 → 본인이 accepted answer 마킹 → 트리거. 자기 글에 자기 답변은 어색하니 자연스러운 첫 질문이 들어올 때까지 기다리시는 걸 권장.

### 11. Profile Status / Pronouns (web UI)

GitHub 프로필 우측 상단 → status emoji + 한 줄 (예: 🛠 Building CHRONO).
Settings → Profile → Pronouns 에 he/him 등 설정 가능.

### 12. 2FA (없으시면 강력 권장)

Settings → Password and authentication → Two-factor authentication. 토큰 / 보안 의식 신호로 면접관도 흔히 봅니다.

---

## 📈 면접관 시점에서 강해진 포인트

1. **첫 인상** — 프로필 README 가 \"꿈꾸는 Aspiring\" → \"Designing and shipping\" 으로 톤 전환
2. **Featured Work** 에 CHRONO 56.9% OOS, 22 signals, HyperCLOVA, n8n + Solar 같은 **숫자/스택** 노출
3. **포트폴리오 사이트** — `jihun-moon.github.io/jihun-moon/` 단일 페이지 (Tokyo Night 톤)
4. **Engineering Principles** 섹션 — 시니어가 갖는 가치관 명시 (boundaries · reproducibility · observability)
5. **모든 공개 리포** 가 LICENSE / 템플릿 / Dependabot / 일관 라벨 / squash-merge 갖춰 \"잘 관리되는\" 인상
6. **README 별 Architecture 다이어그램** — 그냥 코드 dump 가 아닌 \"설계 관점\" 노출
7. **v1.0.0 Releases** — 릴리즈 사이클을 인지하는 개발자 신호
8. **CI + Dependabot + Discord notify** — DevOps/MLOps 감각 신호
9. **Curated Stars** — 30+ reference repos, 분야별 이해 신호

---

## 📌 Quick Action Sequence (5분이면 끝)

가장 임팩트 큰 순서:

1. PR #1 머지 (프로필 + 포트폴리오 라이브)
2. Settings → Pages → main /docs → save (포트폴리오 사이트 활성화)
3. Pinned repos 재구성 (im-bank-n8n-agent 추가)
4. Bio 변경 (\"Aspiring\" 제거)
5. 나머지 8개 PR 머지 (Pull Shark 업적 진척)

이 5단계만 끝내면 면접관 입에서 \"이 사람 심상치 않은데\" 가 나오는 상태가 됩니다.
