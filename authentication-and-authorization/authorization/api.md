# API 접근 제어 정책

## 접근 제어 구현

```typescript
// 권한 검증 데코레이터 예시
@RequirePermission('property.create')
async createProperty(propertyData: PropertyDTO) {
  // 구현 로직
}
```

***

## API 엔드포인트별 권한 설정

| 엔드포인트             | HTTP 메소드 | 필요 권한               | 비고     |
| ----------------- | -------- | ------------------- | ------ |
| /properties       | GET      | property.read       | 전체 공개  |
| /properties       | POST     | property.create     | 중개사 전용 |
| /properties/{id}  | PUT      | property.update     | 소유자 확인 |
| /disadvantages    | POST     | disadvantage.create | 중개사 전용 |
| /credits/purchase | POST     | credit.purchase     | 로그인 필수 |

***

## 접근 제어 정책

### **1. 리소스 소유권 확인**

```typescript
interface ResourceOwnership {
  resourceType: 'property' | 'disadvantage';
  resourceId: string;
  userId: string;
}

function checkOwnership(ownership: ResourceOwnership): boolean {
  // 소유권 확인 로직
}
```

### **2. 조건부 접근 제어**

```json
{
  "conditionalAccess": {
    "property.update": {
      "conditions": [
        "isOwner",
        "isNotDeleted",
        "isWithinTimeLimit"
      ]
    }
  }
}
```

***

## 보안 정책

### **1. 권한 검증 실패 시 응답**

```json
{
  "status": "error",
  "error": {
    "code": "PERMISSION_DENIED",
    "message": "해당 작업을 수행할 권한이 없습니다.",
    "details": {
      "requiredPermission": "property.update",
      "userRole": "USER"
    }
  }
}
```

### **2. 권한 감사 로깅**

```json
{
  "auditLog": {
    "timestamp": "2024-01-04T12:00:00Z",
    "userId": "user123",
    "action": "property.update",
    "resource": "property/123",
    "status": "denied",
    "reason": "insufficient_permission"
  }
}
```
