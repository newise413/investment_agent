---
name: price-fetch
description: 미국(티커 그대로)·한국(숫자.KS) 종목의 실시간 가격·밸류에이션·52주 지표를 yfinance bash로 직접 조회. 웹 검색 대신 이 skill을 우선 실행.
inputs: 티커 목록 (자유 형식 — 한국/미국 혼용 가능)
outputs: 가격 테이블 (이후 분석 skill에 주입)
---

# Skill: price-fetch

## 티커 변환 규칙
사용자 발화에서 티커를 추출한 뒤 아래 규칙으로 변환한다.

| 입력 예시 | 변환 결과 | 비고 |
|-----------|-----------|------|
| VOO, QQQ, META | 그대로 | 미국 주식·ETF |
| 069500, KODEX200 | `069500.KS` | 한국 ETF (6자리 숫자) |
| 005930, 삼성전자 | `005930.KS` | 한국 주식 |
| 379800, KODEX미국S&P500 | `379800.KS` | 한국 상장 해외 ETF |
| 티커 불명확 | `etf-universe.md` 검색 후 매핑 | reference 파일 우선 |

> 한국 종목은 **6자리 숫자 + `.KS`** 가 규칙. 코스닥은 `.KQ` 사용.

## 실행 절차
1. 입력 티커를 위 규칙으로 변환
2. 아래 bash 스크립트를 실행
3. 출력 테이블을 이후 분석 skill(stock-discovery, risk-check, rebalance 등)에 그대로 주입
4. 조회 실패 종목은 `⚠️ 조회실패` 표기 후 WebSearch로 fallback

## bash 스크립트
```bash
pip install yfinance --break-system-packages -q

python3 - <<'EOF'
import yfinance as yf
import json
from datetime import datetime

tickers = __TICKERS__  # 예: ["VOO", "069500.KS", "META"]

rows = []
for t in tickers:
    try:
        info = yf.Ticker(t).info
        rows.append({
            "티커":        t,
            "이름":        info.get("shortName", info.get("longName", "-")),
            "현재가":      info.get("currentPrice") or info.get("regularMarketPrice", "-"),
            "통화":        info.get("currency", "-"),
            "52주고":      info.get("fiftyTwoWeekHigh", "-"),
            "52주저":      info.get("fiftyTwoWeekLow", "-"),
            "시총":        info.get("marketCap", "-"),
            "PER(TTM)":   info.get("trailingPE", "-"),
            "ForwardPER": info.get("forwardPE", "-"),
            "배당수익률":  info.get("dividendYield", "-"),
            "베타":        info.get("beta", "-"),
            "조회시각":    datetime.now().strftime("%Y-%m-%d %H:%M KST"),
        })
    except Exception as e:
        rows.append({"티커": t, "현재가": f"⚠️ 조회실패({e})"})

# 마크다운 테이블 출력
headers = ["티커","이름","현재가","통화","52주고","52주저","PER(TTM)","ForwardPER","배당수익률","베타","조회시각"]
print("| " + " | ".join(headers) + " |")
print("|" + "---|" * len(headers))
for r in rows:
    print("| " + " | ".join(str(r.get(h, "-")) for h in headers) + " |")
EOF
```

## 출력 포맷
```
## 📡 실시간 가격 조회 — YYYY-MM-DD HH:MM KST
| 티커 | 이름 | 현재가 | 통화 | 52주고 | 52주저 | PER(TTM) | ForwardPER | 배당수익률 | 베타 | 조회시각 |
|------|------|--------|------|--------|--------|----------|------------|------------|------|----------|
| VOO  | Vanguard S&P 500 ETF | 542.10 | USD | 570.21 | 439.88 | - | - | 1.23% | 1.00 | ... |
| 069500.KS | KODEX 200 | 129,305 | KRW | 134,000 | 88,000 | - | - | 0.8% | - | ... |
```

## 다른 skill과의 연동 규칙
- `stock-discovery` — 후보 5개 확정 후 이 skill로 가격 주입 → 표준 4블록 작성
- `risk-check` — 보유 종목 현재가 일괄 조회 → 실제 비중 계산에 사용
- `rebalance` — 목표 비중 대비 현재가 기반 실제 비중 계산에 사용
- `trade-log` — 미실현 PnL 계산 시 현재가 조회에 사용

## 주의사항
- yfinance는 KRX 종가 기준 데이터를 제공 — 장중 실시간이 아닌 **최근 종가** 기준
- 한국 ETF 배당수익률은 yfinance에서 누락되는 경우 있음 → `etf-universe.md` 보수 항목으로 보완
- 조회 실패 시 WebSearch fallback 전에 티커 형식(`.KS` / `.KQ`) 재확인
