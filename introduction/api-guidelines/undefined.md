# 응답 형식

## 성공 응답

```json
{
    "status": "success",
    "data": {
        // 실제 응답 데이터
    },
    "message": "성공 메시지",
    "timestamp": "2024-01-04T12:00:00Z"
}
```

***

## 에러 응답

```json
{
    "status": "error",
    "error": {
        "code": "ERROR_CODE",
        "message": "사용자 친화적 에러 메시지",
        "details": [
            {
                "field": "영향받는 필드명",
                "reason": "구체적인 실패 사유",
                "suggestion": "해결 방법 제안"
            }
        ]
    },
    "timestamp": "2024-01-04T12:00:00Z"
}
```

***

## 주요 에러 코드

| 코드                    | 설명       | HTTP 상태 코드 |
| --------------------- | -------- | ---------- |
| INVALID\_INPUT        | 잘못된 입력값  | 400        |
| UNAUTHORIZED          | 인증 필요    | 401        |
| FORBIDDEN             | 권한 없음    | 403        |
| NOT\_FOUND            | 리소스 없음   | 404        |
| INSUFFICIENT\_CREDITS | 크레딧 부족   | 402        |
| RATE\_LIMIT\_EXCEEDED | 요청 제한 초과 | 429        |

***

## 페이지네이션 응답

```json
{
    "status": "success",
    "data": {
        "items": [],
        "pagination": {
            "currentPage": 1,
            "totalPages": 10,
            "totalItems": 100,
            "itemsPerPage": 10,
            "hasNext": true,
            "hasPrevious": false
        }
    }
}
```

***

## 응답 헤더

| 헤더                     | 설명        | 예시         |
| ---------------------- | --------- | ---------- |
| X-Rate-Limit-Limit     | 요청 제한 횟수  | 100        |
| X-Rate-Limit-Remaining | 남은 요청 횟수  | 95         |
| X-Rate-Limit-Reset     | 제한 초기화 시각 | 1704369600 |
