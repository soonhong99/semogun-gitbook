# 알림 서비스 연동

## 알림 서비스 구조

```json
{
  "notification_channels": {
    "email": {
      "provider": "AWS SES",
      "configuration": {
        "region": "ap-northeast-2",
        "sender": "no-reply@platform.com",
        "templatePath": "/templates/email"
      }
    },
    "sms": {
      "provider": "NAVER CLOUD",
      "configuration": {
        "serviceId": "ncp:sms:kr:xxx",
        "sender": "1588-XXXX"
      }
    },
    "push": {
      "provider": "Firebase Cloud Messaging",
      "configuration": {
        "projectId": "project-id",
        "credentialsPath": "/path/to/credentials.json"
      }
    }
  }
}
```

***

## 알림 템플릿

```json
{
  "templates": {
    "property_update": {
      "title": "매물 정보 업데이트",
      "email_template": {
        "subject": "관심 매물 정보가 업데이트되었습니다",
        "template": "property_update.html"
      },
      "sms_template": {
        "format": "[부동산플랫폼] %property_name% 매물이 업데이트되었습니다. %update_details%"
      },
      "push_template": {
        "title": "매물 업데이트",
        "body": "%property_name% 매물 정보가 변경되었습니다",
        "data": {
          "type": "PROPERTY_UPDATE",
          "propertyId": "%property_id%"
        }
      }
    }
  }
}
```

***

## 알림 발송 예시

```javascript
// 통합 알림 발송
async function sendNotification(user, template, data) {
  const notifications = [];
  
  // 이메일 알림
  if (user.emailNotificationsEnabled) {
    notifications.push(
      emailService.send({
        to: user.email,
        template: template.email_template,
        data: data
      })
    );
  }
  
  // SMS 알림
  if (user.smsNotificationsEnabled) {
    notifications.push(
      smsService.send({
        to: user.phone,
        template: template.sms_template,
        data: data
      })
    );
  }
  
  // 푸시 알림
  if (user.pushToken) {
    notifications.push(
      pushService.send({
        token: user.pushToken,
        template: template.push_template,
        data: data
      })
    );
  }
  
  return Promise.allSettled(notifications);
}
```
