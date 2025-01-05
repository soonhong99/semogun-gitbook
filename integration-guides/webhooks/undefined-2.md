# 재시도 정책

## 재시도 설정

```json
{
  "retry_policy": {
    "max_attempts": 5,
    "initial_delay": 30,
    "max_delay": 3600,
    "backoff_multiplier": 2,
    "status_codes_to_retry": [408, 500, 502, 503, 504]
  }
}
```

***

## 재시도 알고리즘

```javascript
// 지수 백오프 알고리즘
function calculateBackoff(attempt, initialDelay, maxDelay, multiplier) {
  const delay = initialDelay * Math.pow(multiplier, attempt - 1);
  return Math.min(delay, maxDelay);
}
```

***

## 재시도 상태 모니터링

```json
{
  "webhook_status": {
    "id": "whk_123456789",
    "last_attempt": "2024-01-04T12:00:00Z",
    "attempts": [
      {
        "timestamp": "2024-01-04T12:00:00Z",
        "status": "failed",
        "response_code": 503,
        "retry_count": 1
      },
      {
        "timestamp": "2024-01-04T12:01:00Z",
        "status": "success",
        "response_code": 200,
        "retry_count": 2
      }
    ],
    "current_status": "active"
  }
}
```
