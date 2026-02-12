AI 실내 공간 전후 변화 분석 시스템 - 프론트엔드

전/후 이미지를 기반으로 분석 결과를 시각화하고, 사용자 인증 및 리포트를 확인할 수 있는 웹 클라이언트입니다.
백엔드 API(FastAPI)와 통신하여 데이터를 표시합니다.

🚀 기술 스택

Framework: Next.js (App Router)

Language: TypeScript

Styling: Tailwind CSS

Package Manager: pnpm

Runtime: Node.js 22.x

State Management: (추후 추가 예정)

API Communication: fetch (기본) / axios (선택)

📁 프로젝트 구조(예시)
frontend/
│
├── src/
│   ├── app/                          # App Router (페이지 라우팅)
│   │
│   │   ├── layout.tsx                # 루트 레이아웃
│   │   ├── page.tsx                  # 랜딩 / 비로그인 대시보드
│   │
│   │   ├── (auth)/                   # 인증 영역 (Route Group)
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   ├── signup/
│   │   │   │   └── page.tsx
│   │   │   └── layout.tsx
│   │
│   │   ├── dashboard/                # 로그인 사용자 대시보드
│   │   │   ├── page.tsx
│   │   │   └── layout.tsx
│   │
│   │   ├── properties/               # 매물 영역 (공통 접근)
│   │   │   ├── page.tsx              # 매물 목록 (비로그인/로그인 공통)
│   │   │   ├── [id]/
│   │   │   │   └── page.tsx          # 매물 상세
│   │   │   │
│   │   │   ├── manage/               # 로그인 전용
│   │   │   │   ├── page.tsx          # 매물 관리 허브
│   │   │   │   │
│   │   │   │   ├── move-in/          # 입주 등록
│   │   │   │   │   └── page.tsx
│   │   │   │   │
│   │   │   │   ├── move-out/         # 퇴거 등록
│   │   │   │   │   └── page.tsx
│   │   │   │   │
│   │   │   │   ├── compare/          # AI 전·후 비교 분석
│   │   │   │   │   └── page.tsx
│   │   │   │   │
│   │   │   │   └── report/           # 분석 보고서 생성
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   └── layout.tsx
│   │
│   │   ├── settings/                 # 사용자 설정 (로그인 전용)
│   │   │   ├── page.tsx              # 설정 메인
│   │   │   └── edit/
│   │   │       └── page.tsx          # 회원정보 수정
│   │
│   │   └── api/                      # (선택) Next API Route
│   │
│   ├── components/                   # 공통 UI 컴포넌트
│   │   ├── layout/
│   │   ├── property/
│   │   ├── analysis/
│   │   └── ui/
│   │
│   ├── features/                     # 도메인 단위 기능 모듈화 (권장)
│   │   ├── auth/
│   │   ├── property/
│   │   ├── analysis/
│   │   └── user/
│   │
│   ├── lib/                          # API 유틸, fetch wrapper
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   └── constants.ts
│   │
│   ├── hooks/                        # 커스텀 훅
│   │
│   ├── store/                        # 전역 상태 (zustand 등)
│   │
│   └── styles/                       # 전역 스타일
│       └── globals.css
│
├── public/                           # 정적 파일
│
├── .env.local                        # 로컬 환경 변수 (커밋 금지)
├── .env.example                      # 환경 변수 예시 (커밋)
├── middleware.ts                     # 로그인/권한 체크
├── package.json
├── pnpm-lock.yaml                    # 삭제 금지
├── next.config.mjs
└── README.md

🔧 설치 및 실행
1️⃣ 런타임 설치 (최초 1회)

프로젝트 루트(HCT)에서:

mise install


확인:

node -v
pnpm -v

2️⃣ 의존성 설치
cd frontend
pnpm install

3️⃣ 환경 변수 설정

.env.example을 복사하여 .env.local 생성:

cp .env.example .env.local


.env.local 내용:

NEXT_PUBLIC_API_URL="http://localhost:8000/api/v1"


⚠ .env.local은 커밋하지 않습니다.

4️⃣ 개발 서버 실행
pnpm dev


브라우저 접속:

http://localhost:3000

🔗 백엔드 연결

Backend: http://localhost:8000

API Base URL:

http://localhost:8000/api/v1


백엔드에서 반드시 CORS 설정 필요:

allow_origins=["http://localhost:3000"]

📦 주요 스크립트
pnpm dev        # 개발 서버 실행
pnpm build      # 프로덕션 빌드
pnpm start      # 빌드된 서버 실행
pnpm lint       # ESLint 실행

🔐 인증 흐름

로그인/회원가입 요청 → 백엔드에서 JWT 발급

프론트엔드에서 access_token 저장

API 요청 시 헤더 포함

Authorization: Bearer {access_token}


토큰 저장 방식은 추후 구현 예정 (localStorage / cookie 등 결정 필요)

🛠 개발 가이드
새로운 페이지 추가
src/app/경로/page.tsx


App Router 기반 구조 사용.

API 호출 추가

src/lib/api.ts (예시)

const BASE_URL = process.env.NEXT_PUBLIC_API_URL;

export async function fetchUser() {
  const res = await fetch(`${BASE_URL}/me`);
  return res.json();
}

컴포넌트 추가
src/components/ComponentName.tsx

🧪 로컬 개발 전체 실행 순서
1️⃣ 백엔드 실행
uv run uvicorn app.main:app --reload --port 8000

2️⃣ 프론트엔드 실행
pnpm dev

3️⃣ 접속

Frontend: http://localhost:3000

Backend Docs: http://localhost:8000/api/docs

📦 배포 (예정)

Vercel 배포 예정

환경 변수는 Vercel Dashboard에서 설정

NEXT_PUBLIC_API_URL을 배포 서버 주소로 변경

🧪 팀원 온보딩 가이드
git clone <repo>
cd HCT/frontend
mise install
pnpm install
cp .env.example .env.local
pnpm dev


접속:

http://localhost:3000
