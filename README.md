# Investment Agent — 공통 하네스 패키지

GPTers 22기 4주 스터디 참여자 50명이 **같은 페르소나 · 같은 출력 포맷 · 같은 자가검증 규칙**으로 움직이도록 만든 미니 하네스입니다.

> ⚠️ 본 패키지는 **AI 학습 · 실습용**입니다. 실제 투자 자문이 아닙니다.

## 설치 (1분)

Claude Desktop에서 새 대화창을 열고 아래 프롬프트를 통째로 붙여넣으세요.

```
이 저장소를 내 작업 폴더에 복제해줘:
git clone https://github.com/newise413/investment-agent.git
그리고 SYSTEM.md, memory.md, skills/ 를 모두 읽고
"하네스 로드 완료" 라고 출력해줘.
```

## 폴더 구조

```
investment-agent/
├── SYSTEM.md          # 페르소나·커리큘럼·공통 규칙 (= 미니 하네스 본체)
├── memory.md          # 장기 기억 템플릿 (사용자가 채움)
├── skills/
│   ├── stock-discovery.md   # 종목 발굴
│   ├── risk-check.md        # 리스크 점검
│   ├── self-verify.md       # 자가검증 3회 루프
│   ├── rebalance.md         # 월간 리밸런싱
│   └── trade-log.md         # 거래 자동 기록 + 평단/PnL
├── submit.md          # 표준 결과 제출 절차
└── README.md
```

## 사용 원칙

1. 분석은 AI 투자 에이전트가, **결정은 내가**, 기록은 Cowork가
2. 자가검증 3회 루프를 통과하지 않은 종목은 매수하지 않는다
3. 모든 분석은 **실시간 웹 검색** 기반 (학습 데이터 단독 사용 금지)
4. 결과는 `submit.md` 절차에 따라 운영자에게 제출
