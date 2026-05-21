---
name: risk-check
description: 현재 포트폴리오의 거시·집중·유동성·환 리스크 점검
inputs: memory.md, user/portfolio.md, trades/log.csv
outputs: output/risk-YYYY-MM-DD.md
---

# Skill: risk-check

## 점검 5축
1. **거시** — Fed 금리·CPI·달러 인덱스·VIX (웹 검색)
2. **집중도** — 단일 종목 30% / 단일 섹터 40% 초과 여부
3. **상관관계** — 보유 종목 간 1년 상관계수 0.8 이상 묶음
4. **유동성** — 일평균 거래대금 < $10M 종목
5. **환 리스크** — USD 자산 비중 vs 사용자 KRW 지출 비중

## 출력
| 축 | 현재 상태 | 임계치 | 판정 | 권고 |
|---|---|---|---|---|

마지막에 **Top 1 즉시 조치**를 한 줄로.
