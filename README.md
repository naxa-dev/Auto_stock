# Auto_stock (주식 자동 모니터링/대시보드)

하루/실시간(환경에 따라)으로 주가 데이터를 수집하고, 간단한 규칙 기반으로 **모니터링/알림/대시보드**까지 연결할 수 있도록 만든 프로젝트입니다.

> 이 저장소는 `main.py`, `main2.py`, `dashboard.py`, `config.py` 중심으로 동작하며, 실행 환경(로컬/서버)에서 스케줄러와 함께 돌리는 형태를 권장합니다.

---

## 흐름도

> 사용자/스케줄러 → 데이터 수집(가격/지표/뉴스 등) → 저장/로그 → 분석/시그널 생성 → 대시보드 표시(dashboard.py) / 알림(선택)

---

## 주요 기능 (Functional)

### 1) 데이터 수집 / 갱신
- 종목(또는 관심 리스트) 기준으로 가격/지표 데이터를 주기적으로 수집
- 실패 시 재시도 및 로그 저장(기본)

### 2) 상태 저장(State)
- 실행 상태/관심 종목/최근 처리 결과 등을 `seed_state.json` 같은 파일에 저장하여
  재시작 시에도 이어서 처리할 수 있도록 구성(프로젝트 설정에 따라 다를 수 있음)

### 3) 분석 / 시그널(간단 규칙 기반)
- 이동평균, 변동성, 거래량 등 **기본적인 룰 기반 시그널**을 만들어
  모니터링에 활용(구체 로직은 코드 내 구현 기준)

### 4) 대시보드 (dashboard.py)
- 수집/분석 결과를 로컬 대시보드로 확인
- 실행 환경에 따라 Streamlit/Dash/Flask 등으로 구성 가능(현재는 `dashboard.py` 기준)

### 5) 로그 관리
- 실행 로그를 `logs/`에 저장해 문제 발생 시 추적 가능

---

## 프로젝트 구조

```bash
Auto_stock/
├─ app/                 # (있다면) 핵심 로직/모듈
├─ logs/                # 실행 로그(ignored 권장)
├─ .github/workflows/   # GitHub Actions (선택)
├─ config.py            # 설정(키/토큰/종목/주기 등)
├─ dashboard.py         # 대시보드 실행 파일
├─ main.py              # 메인 실행(수집/분석/알림 등)
├─ main2.py             # 보조/대체 실행 엔트리(옵션)
├─ requirements.txt     # Python 의존성
├─ seed_state.json      # 초기 상태/샘플 상태(버전관리 가능)
└─ start.sh             # 서버 실행 스크립트(옵션)
```

---

## 실행 방법

### 1) Python 환경 준비
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate
```

### 2) 패키지 설치
```bash
pip install -r requirements.txt
```

### 3) 설정값 구성
- `config.py`에서 API 키/토큰/기본 설정(관심 종목/주기/로그 경로 등)을 확인하고 본인 환경에 맞게 수정하세요.
- 민감 정보(API Key 등)는 **.env로 분리**한 뒤 `config.py`에서 불러오는 방식을 추천합니다.

예시(.env):
```env
STOCK_API_KEY=...
DISCORD_WEBHOOK_URL=...
```

---

## 실행

### 메인 실행
```bash
python main.py
```

### 대시보드 실행
프로젝트 구현에 따라 아래 중 하나로 실행됩니다(코드 내 확인):
```bash
python dashboard.py
# 또는
streamlit run dashboard.py
```
