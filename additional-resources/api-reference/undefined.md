# 스키마 정의

## 공통 데이터 타입

```typescript
// 기본 응답 형식
interface ApiResponse<T> {
  status: "success" | "error";
  data?: T;
  error?: {
    code: string;
    message: string;
    details?: any;
  };
  timestamp: string;
}

// 페이지네이션 응답
interface PaginatedResponse<T> {
  items: T[];
  pagination: {
    currentPage: number;
    totalPages: number;
    totalItems: number;
    itemsPerPage: number;
    hasNext: boolean;
    hasPrevious: boolean;
  };
}
```

***

## 비즈니스 모델 스키마

### **Property (매물)**

```json
{
  "type": "object",
  "required": ["id", "title", "status", "location", "price"],
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "매물 고유 식별자"
    },
    "title": {
      "type": "string",
      "minLength": 5,
      "maxLength": 100,
      "description": "매물 제목"
    },
    "status": {
      "type": "string",
      "enum": ["DRAFT", "PENDING", "ACTIVE", "COMPLETED", "EXPIRED"],
      "description": "매물 상태"
    },
    "location": {
      "type": "object",
      "properties": {
        "address": {
          "type": "string",
          "description": "도로명 주소"
        },
        "latitude": {
          "type": "number",
          "minimum": 33,
          "maximum": 38
        },
        "longitude": {
          "type": "number",
          "minimum": 124,
          "maximum": 132
        }
      }
    },
    "price": {
      "type": "object",
      "properties": {
        "deposit": {
          "type": "number",
          "minimum": 0,
          "description": "보증금"
        },
        "monthlyRent": {
          "type": "number",
          "minimum": 0,
          "description": "월세"
        },
        "maintenanceFee": {
          "type": "number",
          "minimum": 0,
          "description": "관리비"
        }
      }
    },
    "size": {
      "type": "object",
      "properties": {
        "totalArea": {
          "type": "number",
          "description": "전체 면적(m²)"
        },
        "usableArea": {
          "type": "number",
          "description": "실사용 면적(m²)"
        }
      }
    },
    "facilities": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": ["parking", "airConditioner", "elevator", "security"]
      }
    },
    "images": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "id": {"type": "string"},
          "url": {"type": "string"},
          "order": {"type": "integer"}
        }
      }
    }
  }
}
```

***

## Request/Response 스키마

### **매물 등록 요청**

```json
{
  "type": "object",
  "required": ["title", "location", "price"],
  "properties": {
    "title": {
      "type": "string",
      "minLength": 5,
      "maxLength": 100
    },
    "description": {
      "type": "string",
      "maxLength": 2000
    },
    "location": {
      "$ref": "#/components/schemas/Location"
    },
    "price": {
      "$ref": "#/components/schemas/Price"
    },
    "images": {
      "type": "array",
      "items": {
        "type": "string",
        "format": "binary"
      },
      "minItems": 1,
      "maxItems": 15
    }
  }
}
```
