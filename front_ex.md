    frontend/
    │
    ├── src/
    │   ├── app/                # App Router (페이지 구성)
    │   │   ├── page.tsx        # 메인 페이지
    │   │   └── layout.tsx      # 공통 레이아웃
    │   │
    │   ├── components/         # 공통 컴포넌트 (추후 추가)
    │   │
    │   ├── lib/                # API 유틸, 헬퍼 함수
    │   │
    │   └── styles/             # 전역 스타일
    │
    ├── public/                 # 정적 파일
    │
    ├── .env.local              # 로컬 환경 변수 (커밋 금지)
    ├── .env.example            # 환경 변수 예시 (커밋)
    ├── package.json
    ├── pnpm-lock.yaml          # 의존성 고정 파일 (삭제 금지)
    ├── next.config.mjs
    └── README.md
