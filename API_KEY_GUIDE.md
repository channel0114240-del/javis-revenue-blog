# YouTube/Instagram/TikTok API 키 획득 가이드

## YouTube Data API v3 (무료 티어: 10,000 quota/day)

1. https://console.cloud.google.com 접속
2. 새 프로젝트 생성 (또는 기존 프로젝트 선택)
3. APIs & Services → Library → "YouTube Data API v3" 검색 → 활성화
4. APIs & Services → Credentials → Create Credentials → API Key
5. API Key 복사 → .env에 추가:
   ```
   YOUTUBE_API_KEY=your_key_here
   YOUTUBE_CLIENT_ID=your_client_id
   YOUTUBE_CLIENT_SECRET=your_client_secret
   YOUTUBE_REFRESH_TOKEN=your_refresh_token
   ```

## Instagram Graph API (비즈니스 계정 필요)

1. https://developers.facebook.com 접속
2. 앱 생성 → Business 타입
3. Instagram Graph API 추가
4. 앱 리뷰 통과 후 Access Token 발급
5. .env에 추가:
   ```
   INSTAGRAM_ACCESS_TOKEN=your_token
   INSTAGRAM_BUSINESS_ID=your_business_id
   ```

## TikTok API (비즈니스 계정 필요)

1. https://developers.tiktok.com 접속
2. 앱 생성 → 리뷰 통과
3. Access Token 발급
4. .env에 추가:
   ```
   TIKTOK_ACCESS_TOKEN=your_token
   TIKTOK_APP_ID=your_app_id
   TIKTOK_SECRET=your_secret
   ```

## 참고: 무료 대안 (API 키 없이 시작)

- YouTube: 수동 업로드 (API 키 없이도 콘텐츠 생성 가능)
- Instagram: 수동 업로드
- TikTok: 수동 업로드

API 키 없이도 콘텐츠는 생성할 수 있습니다. 수동 업로드로 시작하고, 키 확보 후 자동화하면 됩니다.
