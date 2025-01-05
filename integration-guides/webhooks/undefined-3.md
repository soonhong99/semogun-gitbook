# 구현 예시

## 웹훅 수신 서버 (Node.js/Express)

```javascript
const express = require('express');
const app = express();

app.post('/webhook', express.json(), async (req, res) => {
  const signature = req.headers['x-webhook-signature'];
  
  // 서명 검증
  if (!verifyWebhookSignature(req.body, signature, process.env.WEBHOOK_SECRET)) {
    return res.status(401).json({ error: 'Invalid signature' });
  }
  
  const { event, data } = req.body;
  
  try {
    switch (event) {
      case 'property.created':
        await handleNewProperty(data);
        break;
      case 'property.updated':
        await handlePropertyUpdate(data);
        break;
      // ... 기타 이벤트 처리
    }
    
    res.status(200).json({ status: 'success' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

***

## 이벤트 처리 예시

```javascript
async function handleNewProperty(data) {
  // 데이터베이스 업데이트
  await db.properties.create({
    externalId: data.propertyId,
    status: data.status,
    metadata: data
  });
  
  // 알림 발송
  await notificationService.send({
    type: 'NEW_PROPERTY',
    recipients: await getInterestedUsers(data),
    data: {
      propertyId: data.propertyId,
      title: data.title
    }
  });
}
```

***

## 에러 처리

```json
{
  "error_responses": {
    "invalid_signature": {
      "status": 401,
      "code": "INVALID_SIGNATURE",
      "message": "웹훅 서명이 유효하지 않습니다"
    },
    "event_processing_failed": {
      "status": 500,
      "code": "PROCESSING_ERROR",
      "message": "이벤트 처리 중 오류가 발생했습니다"
    },
    "invalid_payload": {
      "status": 400,
      "code": "INVALID_PAYLOAD",
      "message": "잘못된 이벤트 페이로드입니다"
    }
  }
}
```
