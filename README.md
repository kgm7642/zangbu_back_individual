# Backend (Spring MVC + MyBatis + MySQL)

부동산 서비스의 백엔드 API입니다.  
**Spring MVC(레거시) + MyBatis + MySQL** 기반이며, (선택) **Redis**, **FCM(Firebase Admin)**, **Swagger(OpenAPI)**, **WebSocket(STOMP)** 를 사용할 수 있습니다.

## 🧱 Tech Stack
- **Language/Build**: Java 17, Gradle
- **Web**: Spring MVC, Spring Security (선택)
- **Persistence**: MyBatis (+ PageHelper)
- **DB**: MySQL
- **Cache/Session(선택)**: Redis
- **Push(선택)**: Firebase Admin SDK (FCM)
- **Docs(선택)**: Swagger(OpenAPI) / springfox
- **Realtime(선택)**: STOMP WebSocket (RabbitMQ relay 등)

## 📂 프로젝트 구조
```
src/
├─ main/
│ ├─ java/your/package/
│ │ ├─ config/ # DB/Swagger/Security/WebMvc 설정
│ │ ├─ controller/ # REST 컨트롤러 (/api/**)
│ │ ├─ service/ # 비즈니스 로직
│ │ ├─ mapper/ # MyBatis Mapper 인터페이스
│ │ ├─ domain/ dto/ # VO/DTO
│ │ ├─ scheduler/ # 배치/스케줄러 (선택)
│ │ └─ websocket/ fcm/ # 실시간/푸시 (선택)
│ └─ resources/
│ ├─ application.yml
│ ├─ mapper/*.xml # MyBatis SQL
│ └─ firebase-key.json (선택, git 커밋 금지)
└─ test/ # JUnit 테스트
```

## ⚙️ 환경 설정 (`src/main/resources/application.yml`)
> **주의**: 실 키/비밀번호는 레포에 커밋하지 말고, 로컬/서버 별 프로파일로 분리하세요.

```yaml
server:
  port: 8080

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/<DB_NAME>?characterEncoding=UTF-8&serverTimezone=Asia/Seoul
    username: <DB_USER>
    password: <DB_PASS>

mybatis:
  mapper-locations: classpath:mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true

logging:
  level:
    root: INFO
    your.package.mapper: DEBUG   # SQL 로그 확인 시

# CORS (프론트 도메인에 맞게 수정)
app:
  cors:
    allowed-origins: "http://localhost:5173,https://<your-frontend-domain>"
    allowed-methods: "GET,POST,PATCH,PUT,DELETE,OPTIONS"
    allowed-headers: "*"

# (선택) JWT
jwt:
  secret: "<your-jwt-secret>"

# (선택) Redis
redis:
  host: localhost
  port: 6379

# (선택) FCM (Firebase Admin) - 파일은 git 커밋 금지
fcm:
  serviceAccount: "classpath:firebase-key.json"

# (선택) STOMP(WebSocket) - 브로커/엔드포인트는 실제 설정에 맞추기
stomp:
  endpoint: "/chat"
  app-destination-prefixes: "/app"
  topic-prefix: "/topic"
  queue-prefix: "/queue"

```

중요: 실제 키/비밀번호는 환경변수/프로필/CI 시크릿으로 관리하고, application-example.yml만 커밋하세요.
firebase-key.json 같은 민감 파일은 절대 커밋하지 마세요.

## 🚀 실행 방법
```
./gradlew clean build
# Boot Jar:
java -jar build/libs/*.jar

# WAR:
# build/libs/*.war 를 Tomcat에 배포
```

## 📄 Swagger / API 문서
http://localhost:8080/swagger-ui.html


