# Investment Agent — 공통 하네스 패키지

GPTers 22기 4주 스터디 참여자들이 **같은 페르소나 · 같은 출력 포맷 · 같은 자가검증 규칙**으로 움직이도록 만든 미니 하네스입니다.

> ⚠️ 본 패키지는 **AI 학습 · 실습용**입니다. 실제 투자 자문이 아닙니다.

## 설치 (1분)

Claude Desktop 새 대화창에 아래를 그대로 붙여넣으세요.

이 저장소를 내 작업 폴더에 복제해줘: git clone https://github.com/newise413/investment_agent.git

그리고 investment_agent/ 안의 SYSTEM.md, memory.md, skills/ 를 모두 읽고 "하네스 로드 완료" 라고 출력해줘.


## 폴더 구조

```
investment_agent/
├── SYSTEM.md                 # 페르소나·커리큘럼·공통 규칙
├── memory.md                 # 장기 기억 (성향·편향·재무)
├── user/                     # STEP 1~4 입력 데이터 (빈 템플릿)
│   ├── profile.md            #  STEP 1
│   ├── assets.md             #  STEP 2
│   ├── freedom-goal.md       #  STEP 3
│   └── portfolio.md          #  STEP 4
├── skills/
│   ├── stock-discovery.md
│   ├── risk-check.md
│   ├── self-verify.md
│   ├── rebalance.md
│   └── trade-log.md
├── output/                   # STEP별 결과 리포트가 쌓이는 곳
├── trades/
│   └── log.csv               # date,ticker,side,qty,price,note
├── submit.md
└── README.md
```

## 워크숍 흐름과의 매핑

| STEP | 워크숍 작업 | 쓰는 파일 |
|------|-------------|----------|
| 1 | 투자 성향 진단 | `user/profile.md` → `memory.md` → `output/01-philosophy-fit.md` |
| 2 | 자산/현금흐름 진단 | `user/assets.md` → `output/02-finance-diagnosis.md` |
| 3 | 경제적 자유 로드맵 | `user/freedom-goal.md` → `output/03-freedom-roadmap.md` |
| 4 | 종목 발굴 + 자가검증 | `skills/*` → `output/04-*.md` → `user/portfolio.md` |
| 5 | 최종 리포트 | 모두 종합 → `output/05-final-report.md` |
| 6 | 월간 알림/거래 기록 | `skills/rebalance.md`, `skills/trade-log.md` → `trades/log.csv` |

## 사용 원칙

1. 분석은 AI 투자 에이전트가, **결정은 내가**, 기록은 Cowork가
2. 자가검증 3회 루프를 통과하지 않은 종목은 매수하지 않는다
3. 모든 분석은 **실시간 웹 검색** 기반 (학습 데이터 단독 사용 금지)
4. 결과는 `submit.md` 절차에 따라 운영자에게 제출
