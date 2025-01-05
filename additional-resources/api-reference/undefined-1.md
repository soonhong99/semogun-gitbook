# 에러 코드 정의

```json
{
  "error_codes": {
    "VALIDATION_ERROR": {
      "code": "400001",
      "message": "입력값 검증 실패",
      "description": "요청 데이터가 유효성 검사를 통과하지 못했습니다"
    },
    "INSUFFICIENT_CREDITS": {
      "code": "400002",
      "message": "크레딧 부족",
      "description": "요청한 작업을 수행하기 위한 크레딧이 부족합니다"
    },
    "UNAUTHORIZED": {
      "code": "401001",
      "message": "인증 실패",
      "description": "유효한 인증 정보가 제공되지 않았습니다"
    },
    "FORBIDDEN": {
      "code": "403001",
      "message": "권한 없음",
      "description": "해당 작업을 수행할 권한이 없습니다"
    }
  }
}
```
