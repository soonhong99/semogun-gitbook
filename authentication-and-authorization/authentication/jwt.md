# JWT 토큰 구조

## Access Token 구조

```json
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "user-id",          // 사용자 고유 ID
    "role": "agent",           // 사용자 역할 (agent/user)
    "permissions": ["read", "write"], // 권한 목록
    "iat": 1704369600,         // 발급 시간
    "exp": 1704376800,         // 만료 시간 (발급 후 2시간)
    "iss": "realestate-platform"  // 발급자
  }
}
```

***

## Refresh Token 구조

```json
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "user-id",
    "tokenId": "unique-token-id", // 토큰 고유 ID (재사용 방지)
    "iat": 1704369600,
    "exp": 1705579200,         // 발급 후 2주
    "iss": "realestate-platform"
  }
}
```
