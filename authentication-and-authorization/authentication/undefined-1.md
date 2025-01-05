# 토큰 갱신 정책

## Access Token 만료 시

### 1. 클라이언트의 토큰 갱신 요청

```http
POST /auth/refresh
Content-Type: application/json

{
    "refreshToken": "eyJhbG..."
}
```

### 2. 서버의 갱신 응답

```json
{
    "status": "success",
    "data": {
        "accessToken": "newEyJhbG...",
        "expiresIn": 7200
    }
}
```

***

## Refresh Token 관리

*   저장 방식: Redis 활용

    ```
    KEY: user:{userId}:refresh_token
    VALUE: tokenId
    EXPIRY: 2주
    ```

***

## 토큰 무효화 정책

### 1. 자동 무효화 조건

* Access Token: 만료 시간 도달
* Refresh Token:
  * 만료 시간 도달
  * 사용자 로그아웃
  * 비밀번호 변경

### 2. 강제 무효화 조건

* 보안 위협 감지
* 관리자에 의한 계정 잠금
* 비정상적인 접근 패턴 감지

***

## 보안 정책

### 1. Token Rotation

* Refresh Token 사용 시마다 새로운 토큰 발급
* 이전 토큰 즉시 무효화

### 2. 동시 접속 제어

* 디바이스당 하나의 유효한 토큰 쌍 유지
* 새로운 로그인 시 이전 세션 종료

### 3. 토큰 저장 보안

* Access Token: 메모리에만 저장
* Refresh Token: HttpOnly 쿠키 사용
