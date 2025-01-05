# 에러 처리

## 주요 에러 코드

| 코드             | 설명         | 해결 방법               |
| -------------- | ---------- | ------------------- |
| TOKEN\_EXPIRED | 토큰 만료      | Refresh Token으로 갱신  |
| TOKEN\_INVALID | 유효하지 않은 토큰 | 재로그인 필요             |
| TOKEN\_MISSING | 토큰 누락      | Authorization 헤더 확인 |

## 에러 응답 예시

```json
{
    "status": "error",
    "error": {
        "code": "TOKEN_EXPIRED",
        "message": "인증이 만료되었습니다. 토큰을 갱신해주세요.",
        "details": {
            "expiredAt": "2024-01-04T14:00:00Z"
        }
    }
}
```
