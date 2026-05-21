---
name: rebalance
description: 월 1회 목표 비중과 현재 비중 비교 → 매수/매도 액션 제안
inputs: user/portfolio.md, trades/log.csv
outputs: output/monthly-report-YYYY-MM.md
---

# Skill: rebalance

## 절차
1. `user/portfolio.md`의 **목표 비중** 읽기
2. `trades/log.csv` 누적 + 현재가(웹 검색)로 **실제 비중** 계산
3. 차이 ±3%p 초과 종목 추출
4. 매수/매도 액션 제안 — 단, **세금·수수료** 1회 언급
5. 다음 달 체크포인트 1개 제시

## 출력 표
| 종목 | 목표% | 실제% | 차이 | 액션 | 예상 금액 |

마지막에 다음 달 매크로 이벤트 캘린더 3개 (FOMC, CPI, 어닝 등).
