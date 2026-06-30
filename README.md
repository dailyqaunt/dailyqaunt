# 📊 Quant Research Platform

개인 퀀트 리서치 플랫폼입니다. 한국(KRX) 및 미국(US) 주식 시장을 대상으로 알파 팩터 리서치, 전략 백테스팅, 기술적 지표 분석을 수행하고, 그 결과를 정적 웹사이트로 시각화합니다.

> **핵심 철학**: 분석은 로컬(VSCode)에서, 결과만 GitHub Pages에 게시합니다.

🔗 **Live Site**: [your-username.github.io/quant-research](https://your-username.github.io/quant-research)

---

## 📁 프로젝트 구조

```
quant-research/
│
├── research/                   # 로컬 분석 코드 (VSCode에서 실행)
│   ├── data/                   # 데이터 수집
│   │   ├── fetch_stock.py      # 종목 주가 / 거래량
│   │   ├── fetch_index.py      # 대표 지수 (날짜 범위 입력)
│   │   └── fetch_macro.py      # 매크로 데이터 (금리, 환율 등)
│   │
│   ├── indicators/             # 기술적 보조지표
│   │   ├── trend.py            # MACD, EMA, SMA, 볼린저밴드
│   │   ├── momentum.py         # RSI, Stochastic, ROC
│   │   └── volume.py           # OBV, 거래량 이동평균
│   │
│   ├── alpha/                  # 알파 팩터 리서치
│   │   ├── factor_momentum.py  # 모멘텀 팩터
│   │   ├── factor_value.py     # 가치 팩터 (PER, PBR)
│   │   ├── factor_quality.py   # 퀄리티 팩터 (ROE, 부채비율)
│   │   └── factor_analysis.py  # 팩터 IC / 수익률 분석
│   │
│   ├── backtest/               # 백테스팅 엔진
│   │   ├── engine.py           # Backtrader 설정
│   │   ├── strategies/         # 전략 모음
│   │   │   ├── momentum_strategy.py
│   │   │   ├── mean_reversion.py
│   │   │   └── factor_strategy.py
│   │   └── run_backtest.py     # 백테스트 실행 → JSON 저장
│   │
│   └── export/                 # 결과 → JSON 변환
│       ├── export_chart.py
│       ├── export_backtest.py
│       └── export_factor.py
│
├── output/                     # ⚠️ gitignore — 로컬 결과 저장소
│
├── site/                       # GitHub Pages 정적 사이트
│   ├── data/                   # output/ 에서 복사한 JSON
│   ├── assets/                 # CSS / JS
│   ├── index.html              # 메인 대시보드
│   ├── stock.html              # 종목 차트
│   ├── backtest.html           # 백테스트 결과
│   └── factor.html             # 팩터 리서치
│
├── docs/
│   ├── SETUP.md                # 환경 세팅 가이드
│   └── HOW_TO_USE.md           # 기능별 사용법
│
├── .env.example                # API 키 양식
├── requirements.txt
└── README.md
```

---

## ⚙️ 환경 세팅

### 1. 저장소 클론

```bash
git clone https://github.com/your-username/quant-research.git
cd quant-research
```

### 2. Python 패키지 설치

Python 3.10 이상을 권장합니다.

```bash
pip install -r requirements.txt
```

### 3. API 키 설정

`.env.example`을 복사해서 `.env` 파일을 만들고 키를 입력합니다.

```bash
cp .env.example .env
```

```env
# .env
KOREA_INVESTMENT_APP_KEY=your_key_here
KOREA_INVESTMENT_APP_SECRET=your_secret_here
```

> `.env` 파일은 gitignore에 포함되어 있어 GitHub에 올라가지 않습니다.

---

## 🚀 사용 방법

### 종목 데이터 수집

```bash
# 삼성전자 최근 1년 주가 수집
python research/data/fetch_stock.py --ticker 005930 --market KRX --period 1y

# 애플 주가 수집
python research/data/fetch_stock.py --ticker AAPL --market US --period 1y
```

### 지수 데이터 수집

```bash
# KOSPI 날짜 범위로 수집
python research/data/fetch_index.py --index KOSPI --start 2020-01-01 --end 2024-12-31
```

### 보조지표 계산

```bash
# MACD, 볼린저밴드 등 계산
python research/indicators/trend.py --ticker 005930
```

### 백테스트 실행

```bash
# 모멘텀 전략 백테스트
python research/backtest/run_backtest.py --strategy momentum --start 2020-01-01 --end 2024-12-31
```

### 결과 사이트에 반영

```bash
# output/ → site/data/ 복사 후 push
cp -r output/ site/data/
git add site/data/
git commit -m "update: 백테스트 결과 업데이트"
git push
```

GitHub Pages가 자동으로 사이트를 업데이트합니다.

---

## 🛠️ 기술 스택

| 구분 | 기술 |
|------|------|
| 데이터 수집 | `yfinance`, `pykrx` |
| 데이터 처리 | `pandas`, `numpy` |
| 보조지표 | `pandas-ta` |
| 백테스팅 | `backtrader` |
| 성과 분석 | `pyfolio-reloaded` |
| 팩터 분석 | `alphalens-reloaded` |
| 프론트엔드 | HTML / CSS / JavaScript |
| 차트 | TradingView Lightweight Charts |
| 배포 | GitHub Pages |

---

## 📈 주요 기능

- **종목 차트**: 주가 + 거래량 + 보조지표(MACD, RSI, 볼린저밴드 등) 시각화
- **지수 조회**: KOSPI, KOSDAQ, S&P500, NASDAQ 등 날짜 범위로 조회
- **알파 팩터 리서치**: 모멘텀 / 가치 / 퀄리티 팩터 분석 및 IC 계산
- **백테스팅**: 전략별 수익률, MDD, 샤프비율 분석
- **대시보드**: 분석 결과 종합 리포트

---

## 📋 워크플로우

```
1. research/data/       → 데이터 수집
2. research/indicators/ → 보조지표 계산
3. research/alpha/      → 팩터 분석
4. research/backtest/   → 백테스트 실행
5. research/export/     → JSON 변환
          ↓
6. output/ 저장 (로컬 전용)
          ↓
7. site/data/ 복사 → git push
          ↓
8. GitHub Pages 자동 반영
```

---

## ⚠️ 주의사항

- `output/` 폴더는 gitignore 처리되어 GitHub에 올라가지 않습니다.
- `.env` 파일에 API 키를 저장하고 절대 커밋하지 마세요.
- 본 프로젝트는 **투자 권유가 아닙니다**. 리서치 및 학습 목적으로만 사용하세요.

