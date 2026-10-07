# Container Fixes (2026-09-05)

## 문제 1: javis-crypto-bot 재시작 실패 (calc_atr TypeError)

### 원인
- `strategies_unified.py` line 639-641에서 `calc_atr()` 함수의 호출 시그니처 불일치
- `import pandas as pd as _pd` (유효하지 않은 Python 구문 - double-as clause)
- Docker 컨테이너가 네트워크 별칭 문제로 Redis에 연결 실패

### 해결
1. **strategies_unified.py 수정** (line 639-641):
   ```python
   import pandas as pd
   _df = pd.DataFrame({"high": highs, "low": lows, "close": closes})
   atr = calc_atr(_df, period=14)
   ```

2. **import 문 수정**: `import pandas as pd as _pd` → `import pandas as pd`

3. **.go_live_token 게이트 추가** (`crypto_bot_v2.py __main__` 블록):
   - 토큰 없으면 강제 DRY_RUN 모드

### 증거
- 컨테이너 재시작 후 `Paper mode: skipping leverage setup` 메시지 출력
- calc_atr TypeError 에러 없음

---

## 문제 2: javis-bridge Restart 발생

### 원인
- `javis-redis` 컨테이너가 Docker Compose 네트워크(`javis_javis`)에 연결되지 않음
- `redis` 호스트명 DNS 해석 실패 → `Name or service not known`

### 해결
```bash
docker rm javis-redis
docker-compose up -d --no-deps --force-recreate redis javis-bridge javis-core
```

### 증거
- `javis-bridge`: `Up 20 minutes` (재시작 없음)
- `[Bridge] 구독 시작` + `[Bridge] 5개 이벤트 전달` 로그 출력

---

## 문제 3: javis-core healthcheck 실패

### 원인
- 동일하게 Redis 연결 문제

### 해결
- javis-redis 컨테이너 재생성으로 자동 해결
- `javis-core`: `Up 20 minutes (healthy)`

---

## 문제 4: javis-crypto-bot 시작 실패 (hyperliquid-python-sdk 미설치)

### 원인
- `Dockerfile.worker`가 `requirements.txt`만 사용하지만, `hyperliquid-python-sdk` 의존성 누락

### 해결
```bash
# requirements.txt에 추가
hyperliquid-python-sdk>=0.1.0
```

### 상태
- Docker 이미지 재빌드 중

---

## 문제 5: Grafana 대시보드 프로비저닝 실패

### 원인
- `javis_overview.json`이 Grafana API 응답 형식(`{"dashboard": {...}}`)으로 작성됨
- Grafana 11+에서 `"Dashboard title cannot be empty"` 에러

### 해결
- `dashboard` 키 제거, 대시보드 모델 JSON만 유지

### 상태
- 커밋 완료 (4cc2b421)
