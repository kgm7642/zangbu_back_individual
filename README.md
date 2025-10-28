좋아요 👍
이제 진짜 **한 번에 복사 → GitHub README.md에 붙여넣으면 완벽하게 나오는 버전**으로 만들어드릴게요.
줄바꿈, 코드블록, 인덴트 전부 정리된 **최종 완성판**입니다.

---

# 🏡 안전한 부동산 거래 정보 도우미 (Backend)

> 신뢰 기반의 부동산 직거래 플랫폼 백엔드 API
> **Spring MVC + MyBatis + MySQL 기반 | 백진사 팀 프로젝트**

---

## 📘 프로젝트 개요

**“안전한 부동산 거래 도우미”**는 부동산 직거래 시장의 **정보 비대칭 문제**를 해결하기 위해
AI 분석, 실거주자 리뷰, 실시간 채팅·알림 기능을 제공하는 **신뢰 중심 부동산 서비스**입니다.

이 저장소는 해당 서비스의 **백엔드 API 서버**로,
`Spring MVC (레거시)` + `MyBatis` + `MySQL` 기반으로 구축되었습니다.
필요에 따라 `Redis`, `Firebase FCM`, `Swagger`, `WebSocket(STOMP)` 등을 확장 지원합니다.

> 🔗 프론트 저장소: [zangbu_back_individual](https://github.com/kgm7642/zangbu_front_individual)

---

## ⚙️ Tech Stack

| 구분                      | 기술                                                    |
| ----------------------- | ----------------------------------------------------- |
| **Language / Build**    | Java 17, Gradle                                       |
| **Framework**           | Spring MVC (Legacy), Spring Security *(선택)*           |
| **ORM / SQL Mapper**    | MyBatis (+ PageHelper)                                |
| **Database**            | MySQL                                                 |
| **Cache / Session**     | Redis *(선택)*                                          |
| **Push / Notification** | Firebase Admin SDK (FCM) *(선택)*                       |
| **Realtime**            | STOMP WebSocket / RabbitMQ *(선택)*                     |
| **Docs / API Spec**     | Swagger (springfox) *(선택)*                            |
| **Infra**               | Naver Cloud Platform (NCP), Cloudflare, Docker, Nginx |

---

## 🧩 주요 기능

| 기능                       | 설명                                |
| ------------------------ | --------------------------------- |
| 🧾 **매물 관리 API**         | 매물 등록, 상세 조회, 찜, 필터링              |
| 🔔 **실시간 알림 API**        | FCM 기반 푸시 (시세 변동 / 거래 발생 / 리뷰 등록) |
| 💬 **채팅 API**            | WebSocket + STOMP 기반 실시간 1:1 상담   |
| 🧠 **AI 등기부 분석 리포트 API** | 공식 문서 기반 위험요소 분석 리포트 제공           |
| 🧍 **인증 / 사용자 API**      | JWT 로그인·회원가입, 본인인증 API 연동         |
| 💳 **결제 API**            | Toss Payments 결제, 멤버십 구독          |
| 🗺 **지도 연동 API**         | Kakao Map 기반 매물 탐색 및 필터링          |

---

## 🧱 프로젝트 구조

```
src/
├─ main/
│  ├─ java/your/package/
│  │  ├─ config/         # DB/Swagger/Security/WebMvc 설정
│  │  ├─ controller/     # REST 컨트롤러 (/api/**)
│  │  ├─ service/        # 비즈니스 로직 계층
│  │  ├─ mapper/         # MyBatis Mapper 인터페이스
│  │  ├─ domain/ dto/    # VO/DTO 클래스
│  │  ├─ scheduler/      # 스케줄러/배치 (선택)
│  │  └─ websocket/ fcm/ # 실시간 통신 / 푸시 기능 (선택)
│  └─ resources/
│     ├─ application.yml
│     ├─ mapper/*.xml     # MyBatis SQL 매핑 파일
│     └─ firebase-key.json (선택, Git 커밋 금지)
└─ test/                  # JUnit 테스트 코드
```

---

## ⚙️ 환경 설정 (`application.yml` 예시)

> 🔐 **주의:** 비밀번호, API Key, FCM 키 등은 절대 커밋 금지
> `application-example.yml`만 커밋하고, 실 환경은 환경변수 또는 CI/CD 시크릿으로 관리하세요.

```yaml
server:
  port: 8080

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/realestate?characterEncoding=UTF-8&serverTimezone=Asia/Seoul
    username: root
    password: your_password

mybatis:
  mapper-locations: classpath:mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true

logging:
  level:
    root: INFO
    com.example.project.mapper: DEBUG

app:
  cors:
    allowed-origins: "http://localhost:5173,https://your-frontend-domain"
    allowed-methods: "GET,POST,PATCH,PUT,DELETE,OPTIONS"
    allowed-headers: "*"

# (선택) Redis
redis:
  host: localhost
  port: 6379

# (선택) JWT
jwt:
  secret: "your-jwt-secret"

# (선택) Firebase Admin SDK
fcm:
  serviceAccount: "classpath:firebase-key.json"

# (선택) WebSocket(STOMP)
stomp:
  endpoint: "/chat"
  app-destination-prefixes: "/app"
  topic-prefix: "/topic"
```

---

## 🚀 실행 방법

### 🔧 1️⃣ 빌드

```bash
./gradlew clean build
```

### ▶️ 2️⃣ 실행

**JAR 실행**

```bash
java -jar build/libs/*.jar
```

**WAR 배포**

```bash
# Tomcat webapps 디렉토리에 배포
cp build/libs/*.war /usr/local/tomcat/webapps/
```

---

## 🧾 Swagger / API 문서

서버 실행 후 아래 주소로 접속합니다.

```
http://localhost:8080/swagger-ui.html
```

> Swagger 설정이 활성화되어 있다면 모든 REST API 명세를 확인할 수 있습니다.

---

## 🧠 데이터베이스 구조 (요약)

| 테이블                         | 설명             |
| --------------------------- | -------------- |
| `member`                    | 사용자 정보 (인증/권한) |
| `building`                  | 매물 정보          |
| `review`                    | 실거주자 리뷰        |
| `notification`              | 알림 내역 (FCM 연동) |
| `payment`                   | 결제 내역          |
| `deal`                      | 거래 정보          |
| `chat_room`, `chat_message` | 채팅 시스템         |
| `fcm_token`                 | 디바이스 토큰 저장     |

---

## 📈 배포 및 인프라 구성

```
Frontend (Vue + TailwindCSS)
   ↓ Axios
Backend (Spring MVC + MyBatis + MySQL)
   ↓
NCP Object Storage / Redis / FCM / RabbitMQ
```

* **CI/CD:** GitHub Actions
* **Infra:** Naver Cloud Platform (NCP)
* **Security:** Cloudflare HTTPS, JWT 인증
* **Containerization:** Docker + Nginx Reverse Proxy

---

## 🛠 협업 도구

* **GitHub** – 이슈 및 PR 기반 협업
* **Notion / Jira** – 기획 및 일정 관리
* **Figma** – UI/UX 설계
* **Swagger / Postman** – API 문서화 및 테스트

---

## 🧩 향후 개선 계획

* 🤖 AI 기반 리뷰 부적절 콘텐츠 필터링
* 🔔 사용자 맞춤형 알림 설정 (조건형 필터)
* 📍 GPS 기반 매물 추천 / 거래 히트맵 시각화
* 💎 포인트 상점 기능 (리뷰 포인트 보상)

---

## 👥 팀 정보

| 이름      | 역할           | 주요 담당                  |
| ------- | ------------ | ---------------------- |
| **백현빈** | 팀장 / 백엔드·프론트 | 매물 API, 인프라 구축         |
| **강경민** | 백엔드·프론트      | 알림 API, 알림 페이지         |
| **김영오** | 백엔드·프론트      | 지도, 리뷰, 결제 기능          |
| **박소정** | 백엔드·프론트      | 인증 및 사용자 관리            |
| **안수연** | 백엔드·프론트      | 채팅 기능                  |
| **이해인** | 백엔드·프론트      | 거래 API                 |
| **전경환** | 백엔드·프론트      | PDF 다운로드, Codef/LLM 연동 |

---

## 📜 License

이 프로젝트는 **교육 및 포트폴리오용 비상업적 프로젝트**입니다.
무단 복제 및 배포를 금합니다.

---
