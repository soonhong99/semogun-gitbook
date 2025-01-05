# 토큰 발급 과정

## 최초 로그인

```mermaid
sequenceDiagram
    Client->>Server: POST /auth/login
    Note right of Client: 이메일/패스워드 전송
    Server->>Server: 인증 정보 검증
    Server->>Server: Access Token 생성
    Server->>Server: Refresh Token 생성
    Server->>Database: Refresh Token 저장
    Server->>Client: 토큰 쌍 반환
```



### 1. 클라이언트가 로그인 요청

```http
POST /auth/login
Content-Type: application/json

{
    "email": "user@example.com",
    "password": "hashedPassword"
}
```

### 2. 서버 응답

```json
{
    "status": "success",
    "data": {
        "accessToken": "eyJhbG...",
        "refreshToken": "eyJhbG...",
        "expiresIn": 7200,
        "tokenType": "Bearer"
    }
}
```

***

## API 요청 시 인증

```http
GET /api/v1/properties
Authorization: Bearer eyJhbG...
```
