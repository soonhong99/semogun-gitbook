# 웹훅 설정 방법

## 웹 훅이란?

웹페이지 or 웹앱에서 발생하는 특정 행동(이벤트)들을 커스텀 Callback으로 변환해주는 방법.

행동 정보들을 **실시간으로 제공**하는데 사용.

웹훅은 서버에서 특정 이벤트가 발생했을 때, 클라이언트를 호출하는 방식으로써 **역방향 API**라고도 불립니다.

***

## 웹훅 등록

```http
POST /api/v1/webhooks
Content-Type: application/json
Authorization: Bearer {YOUR_API_KEY}

{
  "url": "https://your-domain.com/webhook",
  "events": ["property.created", "property.updated"],
  "description": "매물 정보 동기화",
  "secret": "your-webhook-secret",
  "active": true
}
```

### **등록에 대한 응답:**

```json
{
  "status": "success",
  "data": {
    "webhookId": "whk_123456789",
    "url": "https://your-domain.com/webhook",
    "events": ["property.created", "property.updated"],
    "secret": "your-webhook-secret",
    "createdAt": "2024-01-04T12:00:00Z"
  }
}
```

***

## 보안 설정

```javascript
// 서명 검증 예시 (Node.js)
const crypto = require('crypto');

function verifyWebhookSignature(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');
    
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}
```

***

## 웹훅 테스트

```bash
# 테스트 이벤트 발송
curl -X POST "https://api.realestate-platform.com/v1/webhooks/{webhookId}/test" \
  -H "Authorization: Bearer {YOUR_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{
    "event": "property.created"
  }'
```



###

###
