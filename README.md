# 제주담다 (JejuDAMDA) - 제주도 여행 추천 앱 백엔드

안드로이드 앱 **제주담다**의 백엔드 서버입니다. 제주도 관광 정보를 제공하고, 사용자 맞춤형 여행 코스를 추천하는 서비스를 구현했습니다.

## 📋 프로젝트 소개

이 프로젝트는 FastAPI를 사용하여 RESTful API를 구축하고, MySQL 데이터베이스와 연동하여 제주도 관광지 정보를 제공합니다. 사용자의 온보딩 정보(연령대, 성별, 여행 테마)를 기반으로 맞춤형 추천을 제공하며, OpenAI GPT API를 활용한 AI 여행 가이드 기능도 포함되어 있습니다.

## 🛠️ 기술 스택

- **Backend Framework**: FastAPI 0.111.0
- **Database**: MySQL (PyMySQL)
- **AI Integration**: OpenAI GPT-4 API
- **Data Processing**: Pandas, NumPy
- **Server**: Uvicorn
- **Environment**: Python 3.8+
- **Authentication**: bcrypt
- **SSH Tunnel**: sshtunnel (데이터베이스 보안 연결)

## 📁 프로젝트 구조

```
project_JEJU/
├── main.py                      # FastAPI 애플리케이션 진입점
├── requirements.txt             # 프로젝트 의존성
│
├── login/                       # 사용자 인증 및 로그인
│   ├── insert_user_info.py     # 사용자 정보 등록
│   └── select_user_id.py       # 사용자 조회 및 인증
│
├── onboarding/                  # 온보딩 프로세스
│   └── onboarding_user_info.py # 사용자 취향 정보 수집
│
├── user/                        # 사용자 관리
│   ├── select_information.py   # 사용자 정보 조회
│   ├── update_information.py   # 사용자 정보 수정
│   └── select_like_list.py     # 좋아요 목록 조회
│
├── travel/                      # 관광지 정보 및 추천
│   ├── call_main_page_items.py # 메인 페이지 아이템 (개인화 추천)
│   ├── call_travel_item_details.py # 관광지 상세 정보
│   ├── likes.py                # 좋아요 기능
│   └── main_category_items.py  # 카테고리별 관광지 조회
│
├── course/                      # 여행 코스 관리
│   ├── router.py               # 코스 라우터
│   ├── create.py               # 코스 생성
│   ├── select.py               # 코스 조회
│   ├── insert.py               # 코스에 관광지 추가
│   ├── detail.py               # 코스 상세 정보
│   ├── update.py               # 코스 정보 수정
│   ├── update_course_sequence.py # 코스 순서 변경
│   └── search.py               # 관광지 검색
│
├── docentAI/                    # AI 여행 가이드
│   ├── travel_course.py        # GPT 기반 여행 코스 추천
│   ├── recommend_list.py       # 사용자 맞춤 추천 목록
│   └── nearby_spot_title.py    # 근처 관광지 추천
│
└── Database/                    # 데이터베이스 스크립트
    ├── create_table.py         # 테이블 생성
    └── create_numprice_columns.py # 컬럼 생성
```

## 🚀 주요 기능

### 1. 사용자 인증 및 온보딩
- 카카오 로그인 연동 (안드로이드 클라이언트)
- 사용자 정보 등록 및 조회
- 온보딩 프로세스를 통한 사용자 취향 수집 (연령대, 성별, 여행 테마)

### 2. 관광지 추천 시스템
- **개인화 추천**: 사용자의 온보딩 정보를 기반으로 태그 매칭 알고리즘을 사용한 관광지 추천
- **카테고리별 추천**: 바다, 힐링, 축제, 카페, 맛집, 호캉스 등 카테고리별 인기 관광지
- **위치 기반 추천**: 사용자 위치 또는 선택한 지역을 기반으로 근처 관광지 추천
- **좋아요 기반 추천**: 다른 사용자들의 좋아요를 기반으로 한 인기 관광지

### 3. AI 여행 가이드 (DocentAI)
- **GPT-4 기반 여행 코스 생성**: 사용자의 성향(일반, 힐링, 로컬, 역사)에 맞춘 스토리텔링 여행 코스
- **위치 및 태그 기반 필터링**: 유클리드 거리와 태그 오버랩을 활용한 관광지 선정
- **퍼소나별 맞춤 설명**: Normal, Healing, Local, History 4가지 퍼소나로 여행 경험을 다르게 제공

### 4. 여행 코스 관리
- 여행 코스 생성, 조회, 수정, 삭제
- 코스에 관광지 추가 및 순서 변경
- 코스 상세 정보 제공

### 5. 좋아요 기능
- 관광지에 좋아요 추가/삭제
- 좋아요 수 실시간 업데이트
- 사용자별 좋아요 목록 조회

## 🔧 환경 설정

### 1. 환경 변수 설정

`.env` 파일을 생성하고 다음 정보를 입력하세요:

```env
# MySQL 데이터베이스 설정
MYSQL_HOSTNAME=your_host
MYSQL_PORT=3306
MYSQL_USERNAME=your_username
MYSQL_PASSWORD=your_password
MYSQL_DATABASE=your_database

# OpenAI API
OPENAI_API_KEY=your_openai_api_key

# SSH Tunnel (선택사항, 보안 연결 시)
SSH_HOSTNAME=your_ssh_host
SSH_PORT=22
SSH_USERNAME=your_ssh_username
SSH_KEY_FILE=path_to_ssh_key
```

### 2. 의존성 설치

```bash
pip install -r requirements.txt
```

### 3. 서버 실행

```bash
python main.py
```

## 📡 API 엔드포인트 구조

모든 API는 `/jeju` prefix를 사용합니다.

### 사용자 관련
- `POST /jeju/check_user` - 사용자 존재 확인
- `POST /jeju/onboarding/` - 온보딩 정보 등록
- `GET /jeju/my_info/{user_id}` - 사용자 정보 조회
- `PUT /jeju/update_onboarding` - 온보딩 정보 수정

### 관광지 관련
- `GET /jeju/main/{user_id}` - 메인 페이지 개인화 추천
- `GET /jeju/details/{content_id}` - 관광지 상세 정보
- `GET /jeju/category/{category}` - 카테고리별 관광지 조회
- `POST /jeju/like` - 좋아요 추가
- `DELETE /jeju/unlike` - 좋아요 삭제

### 코스 관련
- `POST /jeju/course/create` - 코스 생성
- `GET /jeju/course/select/{user_id}` - 사용자 코스 목록
- `GET /jeju/course/detail/{course_id}` - 코스 상세
- `PUT /jeju/course/update` - 코스 정보 수정
- `POST /jeju/course/insert` - 코스에 관광지 추가

### AI 추천
- `POST /jeju/recommend_travel` - GPT 기반 여행 코스 추천
- `GET /jeju/user_recommendations/{userId}` - 사용자 추천 목록
- `POST /jeju/nearby_spots` - 근처 관광지 추천

## 💡 배운 점 & 구현 포인트

### 1. FastAPI를 활용한 RESTful API 설계
- APIRouter를 활용한 모듈화된 라우터 구조 설계
- Pydantic을 활용한 요청/응답 데이터 검증
- 비동기 처리를 통한 성능 최적화

### 2. 데이터베이스 설계 및 연동
- MySQL과 PyMySQL을 활용한 데이터베이스 연동
- SSH Tunnel을 통한 보안 연결 구현
- 태그 기반 추천 시스템을 위한 데이터 구조 설계

### 3. 개인화 추천 알고리즘 구현
- 사용자 온보딩 정보 기반 태그 매칭 알고리즘
- 유클리드 거리를 활용한 위치 기반 필터링
- 태그 오버랩을 활용한 맞춤형 추천

### 4. AI 통합 (OpenAI GPT API)
- GPT-4를 활용한 자연어 기반 여행 코스 생성
- 퍼소나별 시스템 프롬프트 설계
- JSON 형식의 구조화된 응답 파싱

### 5. 안드로이드 클라이언트와의 통신
- RESTful API 설계를 통한 클라이언트-서버 통신
- JSON 기반 데이터 교환
- 사용자 인증 및 세션 관리

## 📊 데이터베이스 스키마

주요 테이블:
- `user_info`: 사용자 기본 정보
- `onboarding_info`: 사용자 온보딩 정보 (연령대, 성별, 여행 테마)
- `likes`: 좋아요 정보
- `courses`: 여행 코스 정보
- `course_items`: 코스에 포함된 관광지 정보
- `visit_main_fix`, `food_main`, `stay_main` 등: 카테고리별 관광지 정보

## 🔐 보안 고려사항
- 환경 변수를 통한 민감 정보 관리 (.env)
- SSH Tunnel을 활용한 데이터베이스 보안 연결
- bcrypt를 활용한 비밀번호 해싱
- SQL Injection 방지를 위한 파라미터화된 쿼리 사용

## 📝 향후 개선 가능한 부분
- 토큰 기반 인증 시스템 도입
- 캐싱 시스템
- API 문서 자동화 (Swagger/OpenAPI)
- 로깅 및 모니터링 시스템 구축
- 단위 테스트 및 통합 테스트 작성

---

이 프로젝트를 통해 FastAPI를 활용한 백엔드 개발, 데이터베이스 설계, AI API 통합, 그리고 클라이언트-서버 통신에 대한 경험을 쌓았습니다.
