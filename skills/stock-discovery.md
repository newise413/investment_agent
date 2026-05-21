---
name: stock-discovery
description: 사용자 성향·기존 보유에 맞는 종목 1~3개를 실시간 데이터로 발굴
inputs: memory.md, user/profile.md, user/portfolio.md
outputs: output/discovery-YYYY-MM-DD.md
---

# Skill: stock-discovery

## 절차
1. `memory.md`에서 투자자 유형·편향·보유 종목 확인
2. 사용자 질문에서 키워드 추출 (예: "배당", "AI", "방어주")
3. **웹 검색**으로 후보 5개 수집 — 시가총액·섹터·1년 수익률·배당률
4. 보유 종목과의 상관관계 점검 (중복 노출 회피)
5. 후보 5개 → 사용자 성향 필터링 → 3개로 좁힘
6. 각 후보별 SYSTEM.md §4 표준 4블록으로 출력
7. `self-verify` skill을 자동 호출하여 3회 루프 실행
8. 결과를 `output/discovery-YYYY-MM-DD.md`에 저장

## 출력 헤더
```
# 종목 발굴 — YYYY-MM-DD
요청: <원문>
투자자 유형: <memory.md 참조>
검색 시점: YYYY-MM-DD HH:MM KST
```
