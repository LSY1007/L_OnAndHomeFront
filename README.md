 전체 구조도

┌─────────────────────────────────────────────────────────┐
│                    사용자 (Browser)                      │ 
└────────────────────┬────────────────────────────────────┘
                     │ HTTPS
                     ↓
┌─────────────────────────────────────────────────────────┐
│              Frontend (React + TypeScript)              │
│                      Port: 3000                         │
│  • Redux Toolkit (상태관리)                              │
│  • Axios (API 통신)                                      │
│  • React Router (라우팅)                                 │
└────────────────────┬────────────────────────────────────┘
                     │ REST API
                     ↓
┌─────────────────────────────────────────────────────────┐
│           Backend (Spring Boot + Java)                  │
│                    Port: 8080                           │
│  • Spring Security + JWT                                │
│  • JPA/Hibernate                                        │
│  • OAuth2 (Naver 소셜 로그인)                            │
│  • WebSocket (실시간 알림)                               │
└────────────┬──────────────────┬─────────────────────────┘
             │                  │
             ↓                  ↓
    ┌────────────────┐  ┌──────────────────┐
    │  MySQL DB      │  │  File Storage    │
    │  Port: 3306    │  │  (이미지 등)      │ 
    │  • 사용자 정보  │  │                  │
    │  • 주문/상품    │  │                  │
    │  • 리뷰/Q&A    │   │                 │
    └────────────────┘  └──────────────────┘

🔄 요청 처리 흐름
1️⃣ 일반 요청 (상품 조회, 주문 등)

[사용자] 
    ↓ (클릭/입력)
[React Frontend]
    ↓ (Axios HTTP 요청)
[Spring Boot Backend]
    ↓ (JPA 쿼리)
[MySQL Database]
    ↓ (데이터 반환)
[Spring Boot Backend]
    ↓ (JSON 응답)
[React Frontend]
    ↓ (Redux 상태 업데이트)
[화면 렌더링]

2️⃣ 인증 요청 (로그인/회원가입)

[사용자]
    ↓ (로그인 시도)
[React Frontend]
    ↓ (POST /api/login)
[Spring Security]
    ↓ (사용자 인증)
[JWT Token 생성]
    ↓ (Access + Refresh Token)
[React Frontend]
    ↓ (localStorage 저장)
[이후 요청 시 Header에 포함]

3️⃣ 소셜 로그인 (Naver OAuth2)

[사용자]
    ↓ (네이버 로그인 클릭)
[Naver OAuth2 Server]
    ↓ (인증 코드 반환)
[Spring Boot Backend]
    ↓ (토큰 교환)
[사용자 정보 조회]
    ↓ (자동 회원가입/로그인)
[JWT Token 발급]
    ↓
[React Frontend]

🛠️ 기술 스택
Frontend
  ● Language : JavaScript
  ● Framework : React
  ● Styling: Tailwind CSS
  ● State Management: Redux Toolkit

Backend (Main - Spring Boot)
  ● Language: Java
  ● Framework: Spring Boot
  ● Security: Spring Security + JWT + OAuth2 Client
  ● Architecture: RESTful API, MVC Pattern
  ● Build Tool: Gradle
  ● DB : MySQL

DevOps & Tools
  ● 버전 관리: Git, GitHub
  ● 개발 도구: VS Code, IntelliJ IDEA
  ● API 테스트: Postman

📁 프로젝트 구조
Frontend (C:\OnAndHomeFront)
```
onandhomefront/
├── public/                          # 정적 파일
│   ├── images/                      # 이미지 리소스
│   │   ├── products/                # 제품 이미지
│   │   ├── categories/              # 카테고리 이미지
│   │   └── common/                  # 공통 이미지
│   └── index.html                   # HTML 템플릿
│
├── src/
│   ├── components/                  # 재사용 컴포넌트
│   │   ├── admin/                   # 관리자 컴포넌트
│   │   │   ├── AdminHeader.jsx
│   │   │   ├── AdminNoticeList.jsx
│   │   │   ├── AdminQnAList.jsx
│   │   │   └── AdminReviewList.jsx
│   │   ├── cart/                    # 장바구니 컴포넌트
│   │   │   ├── CartItem.jsx
│   │   │   └── CartSummary.jsx
│   │   ├── common/                  # 공통 컴포넌트
│   │   │   ├── Footer.jsx
│   │   │   ├── Header.jsx
│   │   │   ├── Loading.jsx
│   │   │   ├── Pagination.jsx
│   │   │   └── SearchBar.jsx
│   │   ├── layout/                  # 레이아웃 컴포넌트
│   │   │   └── MainLayout.jsx
│   │   ├── notice/                  # 공지사항 컴포넌트
│   │   │   ├── NoticeDetail.jsx
│   │   │   └── NoticeList.jsx
│   │   ├── product/                 # 상품 컴포넌트
│   │   │   ├── ProductCard.jsx
│   │   │   ├── ProductGrid.jsx
│   │   │   ├── ProductDetail.jsx
│   │   │   └── ProductCompare.jsx   # 상품 비교
│   │   ├── qna/                     # Q&A 컴포넌트
│   │   │   ├── QnADetail.jsx
│   │   │   ├── QnAForm.jsx
│   │   │   └── QnAList.jsx
│   │   └── review/                  # 리뷰 컴포넌트
│   │       ├── ReviewCard.jsx
│   │       ├── ReviewForm.jsx
│   │       └── ReviewList.jsx
│   │
│   ├── pages/                       # 페이지 컴포넌트
│   │   ├── admin/                   # 관리자 페이지
│   │   │   ├── AdminDashboard.jsx
│   │   │   ├── AdminNotice.jsx
│   │   │   ├── AdminQnA.jsx
│   │   │   └── AdminReview.jsx
│   │   ├── auth/                    # 인증 페이지
│   │   │   ├── Login.jsx
│   │   │   ├── Register.jsx
│   │   │   └── OAuth2Redirect.jsx
│   │   ├── cart/                    # 장바구니 페이지
│   │   │   └── CartPage.jsx
│   │   ├── mypage/                  # 마이페이지
│   │   │   ├── MyPage.jsx
│   │   │   ├── OrderHistory.jsx
│   │   │   └── Profile.jsx
│   │   ├── notice/                  # 공지사항 페이지
│   │   │   ├── NoticePage.jsx
│   │   │   └── NoticeDetailPage.jsx
│   │   ├── order/                   # 주문 페이지
│   │   │   ├── OrderPage.jsx
│   │   │   └── OrderComplete.jsx
│   │   ├── product/                 # 상품 페이지
│   │   │   ├── ProductList.jsx
│   │   │   ├── ProductDetail.jsx
│   │   │   └── ProductCompare.jsx
│   │   ├── qna/                     # Q&A 페이지
│   │   │   ├── QnAPage.jsx
│   │   │   ├── QnADetailPage.jsx
│   │   │   └── QnAWritePage.jsx
│   │   └── Home.jsx                 # 홈페이지
│   │
│   ├── services/                    # API 서비스
│   │   ├── api.js                   # Axios 인스턴스
│   │   ├── authService.js           # 인증 API
│   │   ├── productService.js        # 상품 API
│   │   ├── cartService.js           # 장바구니 API
│   │   ├── orderService.js          # 주문 API
│   │   ├── noticeService.js         # 공지사항 API
│   │   ├── qnaService.js            # Q&A API
│   │   └── reviewService.js         # 리뷰 API
│   │
│   ├── store/                       # Redux 스토어
│   │   ├── slices/                  # 슬라이스
│   │   │   ├── authSlice.js         # 인증 상태
│   │   │   ├── cartSlice.js         # 장바구니 상태
│   │   │   ├── productSlice.js      # 상품 상태
│   │   │   └── compareSlice.js      # 상품 비교 상태
│   │   └── store.js                 # 스토어 설정
│   │
│   ├── utils/                       # 유틸리티
│   │   ├── formatters.js            # 포맷 함수
│   │   ├── validators.js            # 검증 함수
│   │   └── constants.js             # 상수
│   │
│   ├── App.jsx                      # 메인 앱
│   └── index.jsx                    # 엔트리 포인트
│
├── package.json                     # 의존성 관리
└── README.md
```

Backend (C:\OnAndHomeBack)

```
OnAndHome/
├── src/main/java/com/home/onhome/
│   ├── config/                      # 설정 클래스
│   │   ├── SecurityConfig.java      # Spring Security 설정
│   │   ├── WebConfig.java           # CORS, Interceptor 설정
│   │   ├── JwtConfig.java           # JWT 설정
│   │   └── WebSocketConfig.java     # WebSocket 설정
│   │
│   ├── controller/                  # REST 컨트롤러
│   │   ├── admin/                   # 관리자 API
│   │   │   ├── AdminNoticeController.java
│   │   │   ├── AdminQnAController.java
│   │   │   └── AdminReviewController.java
│   │   ├── auth/                    # 인증 API
│   │   │   ├── AuthController.java
│   │   │   └── OAuth2Controller.java
│   │   ├── cart/                    # 장바구니 API
│   │   │   └── CartController.java
│   │   ├── notice/                  # 공지사항 API
│   │   │   └── NoticeController.java
│   │   ├── order/                   # 주문 API
│   │   │   └── OrderController.java
│   │   ├── product/                 # 상품 API
│   │   │   ├── ProductController.java
│   │   │   └── ProductCompareController.java
│   │   ├── qna/                     # Q&A API
│   │   │   └── QnAController.java
│   │   ├── review/                  # 리뷰 API
│   │   │   └── ReviewController.java
│   │   └── user/                    # 사용자 API
│   │       └── UserController.java
│   │
│   ├── domain/                      # 도메인 모델
│   │   ├── entity/                  # JPA 엔티티
│   │   │   ├── User.java            # 사용자
│   │   │   ├── Product.java         # 상품
│   │   │   ├── Category.java        # 카테고리
│   │   │   ├── Cart.java            # 장바구니
│   │   │   ├── CartItem.java        # 장바구니 아이템
│   │   │   ├── Order.java           # 주문
│   │   │   ├── OrderItem.java       # 주문 아이템
│   │   │   ├── Notice.java          # 공지사항
│   │   │   ├── QnA.java             # Q&A
│   │   │   └── Review.java          # 리뷰
│   │   └── dto/                     # DTO
│   │       ├── request/             # 요청 DTO
│   │       └── response/            # 응답 DTO
│   │
│   ├── repository/                  # JPA 리포지토리
│   │   ├── UserRepository.java
│   │   ├── ProductRepository.java
│   │   ├── CategoryRepository.java
│   │   ├── CartRepository.java
│   │   ├── CartItemRepository.java
│   │   ├── OrderRepository.java
│   │   ├── OrderItemRepository.java
│   │   ├── NoticeRepository.java
│   │   ├── QnARepository.java
│   │   └── ReviewRepository.java
│   │
│   ├── service/                     # 비즈니스 로직
│   │   ├── admin/                   # 관리자 서비스
│   │   ├── auth/                    # 인증 서비스
│   │   │   ├── AuthService.java
│   │   │   ├── JwtService.java
│   │   │   └── OAuth2Service.java
│   │   ├── cart/                    # 장바구니 서비스
│   │   ├── notice/                  # 공지사항 서비스
│   │   ├── order/                   # 주문 서비스
│   │   ├── product/                 # 상품 서비스
│   │   ├── qna/                     # Q&A 서비스
│   │   ├── review/                  # 리뷰 서비스
│   │   └── user/                    # 사용자 서비스
│   │
│   ├── security/                    # 보안 관련
│   │   ├── jwt/                     # JWT 관련
│   │   │   ├── JwtTokenProvider.java
│   │   │   ├── JwtAuthenticationFilter.java
│   │   │   └── JwtTokenValidator.java
│   │   ├── oauth2/                  # OAuth2 관련
│   │   │   ├── OAuth2UserService.java
│   │   │   └── OAuth2SuccessHandler.java
│   │   └── CustomUserDetailsService.java
│   │
│   ├── exception/                   # 예외 처리
│   │   ├── GlobalExceptionHandler.java
│   │   ├── CustomException.java
│   │   └── ErrorCode.java
│   │
│   ├── util/                        # 유틸리티
│   │   ├── FileUtil.java
│   │   └── DateUtil.java
│   │
│   └── OnAndHomeApplication.java    # 메인 클래스
│
├── src/main/resources/
│   ├── application.yml              # 설정 파일
│   ├── application-dev.yml          # 개발 환경
│   ├── application-prod.yml         # 운영 환경
│   └── static/                      # 정적 리소스
│       └── uploads/                 # 업로드 파일
│
├── build.gradle                     # Gradle 설정
└── README.md
```

🎯 핵심 기능
🔐 1. 인증 및 보안
JWT 기반 인증
  ● Access Token: 15분 유효기간
  ● Refresh Token: 7일 유효기간
  ● 자동 토큰 갱신: Access Token 만료 시 자동 갱신
  ● Refresh Token Rotation: 보안 강화를 위한 토큰 로테이션
```
// Frontend: Axios Interceptor로 자동 토큰 갱신
axios.interceptors.response.use(
  response => response,
  async error => {
    if (error.response?.status === 401) {
      const newAccessToken = await refreshAccessToken();
      // 재시도
    }
  }
);
```
소셜 로그인 (Naver OAuth2)
  ● 네이버 계정으로 간편 로그인
  ● 자동 회원가입 및 프로필 정보 동기화
  ● OAuth2 인증 플로우 완벽 구현

🛒 2. E-commerce 기능
상품 관리
● 카테고리별 상품 조회
  ● TV, 냉장고, 세탁기 등 카테고리 분류
  ● 무한 스크롤 페이징
  ● 정렬 기능 (인기순, 가격순, 최신순)


● 상품 상세
  ● 고해상도 이미지 갤러리
  ● 상세 스펙 정보
  ● 관련 상품 추천



● 장바구니 시스템
  ● 실시간 수량 조절
  ● 선택 삭제 / 전체 삭제
  ● 총 금액 자동 계산
  ● 로그인 사용자 DB 저장

● 주문 및 결제
  ● 주문자 정보 입력
  ● 배송지 관리
  ● 주문 내역 조회
  ● 주문 상태 추적

🔍 3. 제품 비교 시스템
최대 4개 제품 동시 비교
```
// 비교 기능 핵심 로직
const addToCompare = (product) => {
  if (compareList.length >= 4) {
    alert('최대 4개까지 비교 가능합니다');
    return;
  }
  
  // 같은 카테고리만 비교 가능
  if (compareList[0]?.category !== product.category) {
    alert('같은 카테고리 제품만 비교할 수 있습니다');
    return;
  }
  
  dispatch(addToCompareList(product));
};
```
비교 항목:
  ● 가격, 브랜드, 모델명
  ● 주요 스펙 (크기, 용량, 에너지 등급 등)
  ● 사용자 평점
  ● 리뷰 수

💬 4. 커뮤니티 기능
상품 리뷰
  ● ⭐ 별점 평가 (1-5점)
  ● 사진 첨부 가능
  ● 구매 확정 후 작성 가능
  ● 좋아요 기능

Q&A (비공개 문의)
● 비공개 문의 시스템
   ● 작성자와 관리자만 조회 가능
   ● 🔒 아이콘으로 비공개 표시

● 답변 알림 기능
● 카테고리 분류 (상품, 배송, 교환/환불 등)
 ```
// Backend: Q&A 비공개 처리
@GetMapping("/qna/{id}")
public ResponseEntity<QnAResponse> getQnA(
    @PathVariable Long id,
    @AuthenticationPrincipal UserDetails userDetails
) {
    QnA qna = qnaService.findById(id);
    
    // 비공개 문의는 작성자와 관리자만 조회 가능
    if (qna.isPrivate() && 
        !qna.getAuthor().equals(userDetails.getUsername()) && 
        !userDetails.getAuthorities().contains("ROLE_ADMIN")) {
        throw new AccessDeniedException("접근 권한이 없습니다");
    }
    
    return ResponseEntity.ok(qna);
}
```

공지사항

  ● 중요 공지 상단 고정
  ● 카테고리별 분류
  ● 조회수 집계

📢 5. 실시간 알림 (WebSocket)

  ● 주문 상태 변경 알림
  ● Q&A 답변 알림
  ● 장바구니 품절 알림
  ● 이벤트 알림
```
  // Frontend: WebSocket 연결
const connectWebSocket = () => {
  const socket = new WebSocket('ws://localhost:8080/ws/notifications');
  
  socket.onmessage = (event) => {
    const notification = JSON.parse(event.data);
    showNotification(notification);
  };
};
```
👨‍💼 6. 관리자 기능

  ● 상품 등록/수정/삭제
  ● 주문 관리 및 배송 처리
  ● 공지사항 관리
  ● Q&A 답변
  ● 리뷰 관리 (부적절한 리뷰 삭제)
  ● 사용자 관리
  ● 통계 대시보드


🚀 실행 방법
📋 사전 요구사항

  ● Node.js: 18.x 이상
  ● Java: 17 이상
  ● MySQL: 8.0 이상
  ● Gradle: 8.x 이상

  1️⃣ 데이터베이스 설정
```
  -- MySQL 데이터베이스 생성
CREATE DATABASE onandhome CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 사용자 생성 (선택사항)
CREATE USER 'onandhome'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON onandhome.* TO 'onandhome'@'localhost';
FLUSH PRIVILEGES;
```
2️⃣ Backend 실행
```
# 1. 프로젝트 이동
cd C:\OnAndHomeBack

# 2. application.yml 설정
# src/main/resources/application.yml 파일 수정
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/onandhome
    username: root
    password: your_password
  
  jpa:
    hibernate:
      ddl-auto: update  # 첫 실행 시 create, 이후 update
    show-sql: true
  
  # OAuth2 설정
  security:
    oauth2:
      client:
        registration:
          naver:
            client-id: YOUR_NAVER_CLIENT_ID
            client-secret: YOUR_NAVER_CLIENT_SECRET
            redirect-uri: http://localhost:8080/login/oauth2/code/naver

# JWT 설정
jwt:
  secret: YOUR_JWT_SECRET_KEY_AT_LEAST_256_BITS
  access-token-validity: 900000    # 15분
  refresh-token-validity: 604800000 # 7일

# 3. Gradle 빌드 및 실행
./gradlew clean build
./gradlew bootRun

# 또는 JAR 파일로 실행
java -jar build/libs/OnAndHome-0.0.1-SNAPSHOT.jar
```

Backend 실행 확인:
  ● 서버: http://localhost:8080
  ● Swagger UI (있는 경우): http://localhost:8080/swagger-ui.html
  ● H2 Console (개발용): http://localhost:8080/h2-console

3️⃣ Frontend 실행
```
# 1. 프로젝트 이동
cd C:\OnAndHomeFront

# 2. 의존성 설치
npm install

# 3. .env 파일 생성 (프로젝트 루트)
REACT_APP_API_URL=http://localhost:8080/api
REACT_APP_WS_URL=ws://localhost:8080/ws

# 4. 개발 서버 실행
npm start

# 또는 프로덕션 빌드
npm run build
```

Frontend 실행 확인:
  ● 개발 서버: http://localhost:3000
  ● 자동으로 브라우저가 열립니다

4️⃣ 초기 데이터 설정 (선택사항)
```
# Backend에서 샘플 데이터 생성 API 호출
curl -X POST http://localhost:8080/api/admin/init-data \
  -H "Authorization: Bearer YOUR_ADMIN_TOKEN"
```

📡 API 엔드포인트

● POST /login - 로그인
● POST /register - 회원가입
● POST /refresh - 토큰 갱신
● POST /logout - 로그아웃
● GET /me - 현재 사용자 정보

```
POST /api/auth/login
{
  "username": "user@example.com",
  "password": "password123"
}

// 응답
{
  "accessToken": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
  "tokenType": "Bearer",
  "expiresIn": 900
}
```

### 🛍️ 상품 API (`/api/products`)

| Method | Endpoint | 설명 | 인증 필요 |
|--------|----------|------|-----------|
| GET | `/` | 상품 목록 조회 | ❌ |
| GET | `/{id}` | 상품 상세 조회 | ❌ |
| GET | `/category/{categoryId}` | 카테고리별 조회 | ❌ |
| GET | `/search` | 상품 검색 | ❌ |
| POST | `/compare` | 상품 비교 | ❌ |
| POST | `/` | 상품 등록 (관리자) | ✅ (ADMIN) |
| PUT | `/{id}` | 상품 수정 (관리자) | ✅ (ADMIN) |
| DELETE | `/{id}` | 상품 삭제 (관리자) | ✅ (ADMIN) |

**상품 목록 조회 예시**:
```
GET /api/products?page=0&size=20&sort=createdAt,desc&category=TV
```
🛒 장바구니 API (/api/cart)

● GET / - 장바구니 조회
● POST /items - 상품 추가
● PUT /items/{itemId} - 수량 변경
● DELETE /items/{itemId} - 상품 삭제
● DELETE /clear - 장바구니 비우기

📦 주문 API (/api/orders)
● POST / - 주문 생성
● GET / - 주문 내역 조회
● GET /{orderId} - 주문 상세 조회
● PUT /{orderId}/cancel - 주문 취소
● PUT /{orderId}/status - 주문 상태 변경 (관리자)

💬 리뷰 API (/api/reviews)
● GET /product/{productId} - 상품 리뷰 조회
● POST / - 리뷰 작성
● PUT /{reviewId} - 리뷰 수정
● DELETE /{reviewId} - 리뷰 삭제
● POST /{reviewId}/like - 리뷰 좋아요

❓ Q&A API (/api/qna)
● GET /product/{productId} - 상품 Q&A 조회
● GET /{qnaId} - Q&A 상세 조회
● POST / - Q&A 작성
● PUT /{qnaId} - Q&A 수정
● DELETE /{qnaId} - Q&A 삭제
● POST /{qnaId}/answer - Q&A 답변 (관리자) 

📢 공지사항 API (/api/notices)
● GET / - 공지사항 목록
● GET /{noticeId} - 공지사항 상세
● POST / - 공지사항 작성 (관리자)
● PUT /{noticeId} - 공지사항 수정 (관리자)
● DELETE /{noticeId} - 공지사항 삭제 (관리자)

👨‍💼 관리자 API (/api/admin)
● GET /dashboard - 대시보드 통계
● GET /users - 사용자 목록
● GET /orders - 전체 주문 관리
● PUT /qna/{qnaId}/answer - Q&A 답변
● DELETE /reviews/{reviewId} - 부적절한 리뷰 삭제

🔧 환경 변수 설정
Backend (application.yml)
```
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/onandhome
    username: ${DB_USERNAME:root}
    password: ${DB_PASSWORD:your_password}
    driver-class-name: com.mysql.cj.jdbc.Driver
  
  jpa:
    hibernate:
      ddl-auto: ${DDL_AUTO:update}
    show-sql: ${SHOW_SQL:true}
    properties:
      hibernate:
        format_sql: true
        dialect: org.hibernate.dialect.MySQL8Dialect
  
  # 파일 업로드 설정
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 10MB
  
  # OAuth2 설정
  security:
    oauth2:
      client:
        registration:
          naver:
            client-id: ${NAVER_CLIENT_ID}
            client-secret: ${NAVER_CLIENT_SECRET}
            redirect-uri: ${REDIRECT_URI:http://localhost:8080}/login/oauth2/code/naver
            authorization-grant-type: authorization_code
            scope: name,email,profile_image
            client-name: Naver
        provider:
          naver:
            authorization-uri: https://nid.naver.com/oauth2.0/authorize
            token-uri: https://nid.naver.com/oauth2.0/token
            user-info-uri: https://openapi.naver.com/v1/nid/me
            user-name-attribute: response

# JWT 설정
jwt:
  secret: ${JWT_SECRET:your-secret-key-must-be-at-least-256-bits-long}
  access-token-validity: ${ACCESS_TOKEN_VALIDITY:900000}    # 15분
  refresh-token-validity: ${REFRESH_TOKEN_VALIDITY:604800000} # 7일

# 파일 저장 경로
file:
  upload-dir: ${UPLOAD_DIR:./uploads}

# CORS 설정
cors:
  allowed-origins: ${ALLOWED_ORIGINS:http://localhost:3000}
  allowed-methods: GET,POST,PUT,DELETE,PATCH,OPTIONS
  allowed-headers: '*'
  allow-credentials: true

# 로깅
logging:
  level:
    com.home.onhome: ${LOG_LEVEL:DEBUG}
    org.springframework.security: ${SECURITY_LOG_LEVEL:DEBUG}
```
Frontend (.env)
```
# API URL
REACT_APP_API_URL=http://localhost:8080/api

# WebSocket URL
REACT_APP_WS_URL=ws://localhost:8080/ws

# OAuth2 설정
REACT_APP_NAVER_CLIENT_ID=your_naver_client_id
REACT_APP_REDIRECT_URI=http://localhost:3000/oauth2/redirect

# 기타 설정
REACT_APP_PAGE_SIZE=20
REACT_APP_MAX_COMPARE_PRODUCTS=4
```
🌐 배포
프로덕션 빌드
Backend
```
# JAR 파일 생성
./gradlew clean build -Pprofile=prod

# 실행
java -jar -Dspring.profiles.active=prod build/libs/OnAndHome-0.0.1-SNAPSHOT.jar
```

Frontend
```
# 프로덕션 빌드
npm run build

# 빌드 파일은 build/ 디렉토리에 생성됨
# Nginx, Apache 등으로 서빙
```

Nginx 설정 예시
```
server {
    listen 80;
    server_name yourdomain.com;

    # Frontend
    location / {
        root /var/www/onandhome/frontend/build;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # WebSocket
    location /ws {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```
🔒 보안 고려사항
1. JWT 보안
  ● ✅ 256비트 이상의 강력한 시크릿 키 사용
  ● ✅ Access Token 짧은 유효기간 (15분)
  ● ✅ Refresh Token Rotation
  ● ✅ XSS 방지를 위한 httpOnly 쿠키 사용 고려

2. CORS 설정
```
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000")  // 프로덕션에서는 실제 도메인
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowCredentials(true);
    }
}
```

3. SQL Injection 방지
  ● JPA를 통한 파라미터 바인딩 자동 처리
  ● Native Query 사용 시 파라미터 바인딩 명시
4. XSS 방지
  ● React의 자동 이스케이프 처리
  ● DOMPurify 라이브러리 사용 (HTML 입력 시)
5. CSRF 방지
  ● REST API는 Stateless하므로 CSRF 토큰 불필요
  ● 대신 JWT 토큰으로 인증

📝 개발 가이드
코드 컨벤션
Java (Backend)
  ● 패키지명: 소문자, 도메인 역순
  ● 클래스명: PascalCase
  ● 메서드명: camelCase
  ● 상수: UPPER_SNAKE_CASE
  ```
// 좋은 예
public class UserService {
    private static final int MAX_LOGIN_ATTEMPTS = 5;
    
    public UserResponse findUserById(Long userId) {
        // ...
    }
}
```

JavaScript/React (Frontend)

  ● 파일명: PascalCase (컴포넌트), camelCase (유틸)
  ● 컴포넌트: PascalCase
  ● 함수/변수: camelCase
  ● 상수: UPPER_SNAKE_CASE
```
// 좋은 예
const API_BASE_URL = 'http://localhost:8080/api';

const ProductCard = ({ product }) => {
  const handleAddToCart = () => {
    // ...
  };
  
  return (
    <div className="product-card">
      {/* ... */}
    </div>
  );
};
```

### Git 커밋 컨벤션
```
feat: 새로운 기능 추가
fix: 버그 수정
docs: 문서 수정
style: 코드 포맷팅, 세미콜론 누락 등
refactor: 코드 리팩토링
test: 테스트 코드 추가
chore: 빌드 업무, 패키지 매니저 설정 등

예시:
feat: 상품 비교 기능 추가
fix: 장바구니 수량 업데이트 버그 수정
docs: README에 API 문서 추가
```
🐛 트러블슈팅
1. CORS 에러
증상: Access to XMLHttpRequest at 'http://localhost:8080' from origin 'http://localhost:3000' has been blocked by CORS policy
해결:
```
// Backend: SecurityConfig.java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.cors().configurationSource(corsConfigurationSource());
    // ...
}

@Bean
public CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration configuration = new CorsConfiguration();
    configuration.setAllowedOrigins(Arrays.asList("http://localhost:3000"));
    configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE"));
    configuration.setAllowCredentials(true);
    // ...
}
```
2. JWT 토큰 만료
증상: 401 Unauthorized 에러 지속 발생
해결:
```
// Frontend: api.js
axios.interceptors.response.use(
  response => response,
  async error => {
    const originalRequest = error.config;
    
    if (error.response.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      const refreshToken = localStorage.getItem('refreshToken');
      const { data } = await axios.post('/api/auth/refresh', { refreshToken });
      localStorage.setItem('accessToken', data.accessToken);
      return axios(originalRequest);
    }
    
    return Promise.reject(error);
  }
);
```
3. 이미지 경로 문제
증상: 프로덕션 빌드 후 이미지가 표시되지 않음
해결:
```
# application.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/onandhome?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
```
👥 개발자 정보
개발자: 이상연
GitHub: [https://github.com/LSY1007/L_OnAndHomeFront]
이메일: [dltkddus50@naver.com]
포트폴리오: [포트폴리오 링크]

🙏 감사의 말
이 프로젝트는 레거시 시스템을 현대적인 아키텍처로 마이그레이션하는 경험을 통해 많은 것을 배울 수 있었습니다. 특히:
  ● Monolithic에서 분리 아키텍처로의 전환
  ● JWT 기반 인증 시스템 구현
  ● React + Redux를 활용한 상태 관리
  ● RESTful API 설계 및 구현
앞으로도 더 나은 사용자 경험과 코드 품질을 위해 지속적으로 개선해 나가겠습니다.
