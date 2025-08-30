# 전래동화 '만약에?' 프로젝트

> 전래동화를 기반으로 한 상상력 확장 AI 채팅 플랫폼

## 🎯 **프로젝트 소개**

이 프로젝트는 전래동화를 바탕으로 "만약에?" 질문을 통해 아이들의 상상력을 키우는 AI 채팅 애플리케이션입니다. 사용자는 전래동화의 등장인물이나 상황을 바꿔보며 새로운 이야기를 만들어갈 수 있습니다.

**디자인 컨셉**: 전래동화의 따뜻하고 정감 있는 분위기를 반영하여 갈색과 주황색 위주의 색상 톤을 사용했습니다. 이는 한국 전통문화의 정취를 담으면서도 현대적인 UI/UX를 제공합니다.

## ✨ **주요 기능**

- 🔐 **사용자 인증**: Supabase Auth를 통한 안전한 로그인/회원가입
- 🤖 **AI 채팅**: 네이버 클라우드 API 연동으로 자연스러운 대화
- 📚 **세션 관리**: 대화 기록 저장 및 세션별 관리
- 📱 **반응형 UI**: 모바일 최적화된 ChatGPT 스타일 인터페이스
- 🛡️ **안전 필터**: 부적절한 내용 자동 필터링
- 🔄 **자동 복구**: 네트워크 오류 시 자동 재시도

## 🛠️ **기술 스택**

### Frontend
- **React 18** - 최신 React 기능 활용
- **TypeScript** - 타입 안전성 보장
- **Vite** - 빠른 개발 환경
- **Tailwind CSS** - 유틸리티 기반 스타일링

### UI Components
- **shadcn/ui** - 재사용 가능한 컴포넌트 라이브러리
- **Radix UI** - 접근성 중심의 프리미티브 컴포넌트
- **Lucide React** - 아이콘 라이브러리

### Backend & Infrastructure
- **Supabase** - 인증, 데이터베이스, Edge Functions
- **React Query** - 서버 상태 관리
- **React Router** - 클라이언트 사이드 라우팅

## 🚀 **시작하기**

### 필수 요구사항
- Node.js 18+ 
- npm 또는 yarn
- Supabase 계정
- 네이버 클라우드 API 키

### 설치 및 실행

```bash
# 1. 저장소 클론
git clone <repository-url>
cd korean-traditional-tales-whatif

# 2. 의존성 설치
npm install

# 3. 환경 변수 설정
cp .env.example .env.local
# .env.local 파일에 Supabase 및 네이버 클라우드 API 키 입력

# 4. 개발 서버 실행
npm run dev

# 5. 빌드
npm run build
```

### 환경 변수 설정

```env
# Supabase 설정
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key

# 네이버 클라우드 API (Edge Function에서 사용)
NAVER_CLOUD_API_KEY=your_naver_cloud_api_key
```

## 📁 **프로젝트 구조**

```
src/
├── components/          # 재사용 가능한 UI 컴포넌트
│   ├── ui/             # shadcn/ui 컴포넌트
│   ├── ChatInterface.tsx    # 채팅 인터페이스
│   ├── Sidebar.tsx         # 사이드바
│   └── ...
├── contexts/           # React Context
│   └── SessionContext.tsx  # 세션 관리
├── hooks/              # 커스텀 훅
│   ├── useAuth.ts          # 인증 관련
│   └── useSessionManager.ts # 세션 관리
├── pages/              # 페이지 컴포넌트
│   ├── Index.tsx           # 메인 페이지
│   ├── Auth.tsx            # 인증 페이지
│   └── NotFound.tsx        # 404 페이지
├── types/               # TypeScript 타입 정의
│   └── session.ts          # 세션 관련 타입
└── integrations/        # 외부 서비스 연동
    └── supabase/           # Supabase 설정
```

## 🔧 **Supabase 설정**

### 1. 데이터베이스 테이블 생성

```sql
-- stories 테이블
CREATE TABLE stories (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  title TEXT NOT NULL,
  summary TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- sessions 테이블
CREATE TABLE sessions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id),
  story_id UUID REFERENCES stories(id),
  title TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- chat_turns 테이블
CREATE TABLE chat_turns (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  session_id UUID REFERENCES sessions(id) ON DELETE CASCADE,
  role TEXT NOT NULL CHECK (role IN ('user', 'assistant', 'system')),
  content TEXT NOT NULL,
  timestamp BIGINT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### 2. Edge Functions 배포

```bash
# Supabase CLI 설치
npm install -g supabase

# 로그인 및 프로젝트 연결
supabase login
supabase link --project-ref your-project-ref

# Edge Functions 배포
supabase functions deploy chat
supabase functions deploy start-session
```

## 🎨 **UI/UX 특징**

- **전래동화 테마**: 갈색과 주황색 위주의 따뜻한 톤으로 전래동화의 정취를 담은 디자인
- **ChatGPT 스타일**: 깔끔하고 직관적인 채팅 인터페이스
- **반응형 디자인**: 모든 디바이스에서 최적화된 경험
- **접근성**: ARIA 라벨, 키보드 네비게이션 지원
- **다크/라이트 모드**: 사용자 선호도에 따른 테마 지원
- **로딩 상태**: 사용자 경험을 향상시키는 시각적 피드백

## 🛡️ **보안 및 안전성**

- **사용자 인증**: Supabase Auth를 통한 안전한 인증
- **API 키 보호**: Edge Function에서만 API 키 접근
- **입력 검증**: 사용자 입력에 대한 안전성 검사
- **금칙어 필터**: 부적절한 내용 자동 차단

## 🚀 **배포**

### Vercel 배포 (권장)

```bash
# Vercel CLI 설치
npm i -g vercel

# 배포
vercel
```

### 수동 배포

```bash
# 프로덕션 빌드
npm run build

# dist 폴더를 웹 서버에 업로드
```

## 🤝 **기여하기**

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 **라이선스**

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## ❓ **문의사항**

프로젝트에 대한 질문이나 제안사항이 있으시면 이슈를 생성해 주세요.

---

**Made with ❤️ for Korean Traditional Tales**
