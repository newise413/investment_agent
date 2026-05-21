---
name: trade-log
description: 자연어 거래 발화를 trades/log.csv에 1줄로 append + 평단/PnL 재계산
inputs: 사용자 발화 (예: "VOO 3주 $450에 샀어")
outputs: trades/log.csv (append)
---

# Skill: trade-log

## 파싱 규칙
사용자 발화에서 추출:
- `date` — 명시 없으면 오늘 (KST)
- `ticker` — 대문자 변환
- `side` — 매수/buy → BUY, 매도/팔 → SELL
- `qty` — 정수/소수 가능
- `price` — 통화 기호 자동 인식 ($ / ₩)
- `reason` — 발화에 있으면 그대로, 없으면 직전 분석 결론 1줄

## CSV 스키마
```
date,ticker,side,qty,price,currency,reason
2026-05-21,VOO,BUY,3,450.00,USD,"S&P 장기 코어"
```

## append 후 즉시 출력
1. 해당 종목 **평단** 재계산 (이전 보유 가중평균)
2. 실현 PnL (SELL일 때)
3. 미실현 PnL (현재가 웹 검색)
4. `memory.md > 보유 종목` 자동 갱신
