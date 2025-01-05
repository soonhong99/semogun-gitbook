# API 예제 모음

## 매물 관리 API

### **매물 등록**

```http
POST /api/v1/properties
Content-Type: multipart/form-data
Authorization: Bearer {token}

# Request Body
{
  "title": "강남역 1번출구 카페",
  "description": "유동인구 많은 카페 매물입니다.",
  "location": {
    "address": "서울시 강남구 강남대로 123",
    "latitude": 37.498095,
    "longitude": 127.027610
  },
  "price": {
    "deposit": 50000000,
    "monthlyRent": 3000000,
    "maintenanceFee": 300000
  },
  "size": {
    "totalArea": 82.5,
    "usableArea": 75.2
  },
  "images": [
    (binary data)
  ]
}

# Response 200 OK
{
  "status": "success",
  "data": {
    "propertyId": "prop_123abc",
    "title": "강남역 1번출구 카페",
    "status": "PENDING",
    "createdAt": "2024-01-04T12:00:00Z"
  }
}
```

### **매물 검색**

```http
GET /api/v1/properties/search
?region=강남구
&category=카페
&priceRange=3000000-5000000
&page=1
&limit=20

# Response 200 OK
{
  "status": "success",
  "data": {
    "items": [
      {
        "id": "prop_123abc",
        "title": "강남역 1번출구 카페",
        "location": {
          "address": "서울시 강남구 강남대로 123",
          "region": "강남구"
        },
        "price": {
          "deposit": 50000000,
          "monthlyRent": 3000000
        },
        "thumbnail": "https://...",
        "metrics": {
          "viewCount": 150,
          "likeCount": 23
        }
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 5,
      "totalItems": 98,
      "itemsPerPage": 20
    }
  }
}
```

***

## 인증 API

### **로그인**

```http
POST /api/v1/auth/login
Content-Type: application/json

# Request Body
{
  "email": "agent@example.com",
  "password": "hashedPassword123"
}

# Response 200 OK
{
  "status": "success",
  "data": {
    "accessToken": "eyJhbG...",
    "refreshToken": "eyJhbG...",
    "expiresIn": 3600,
    "user": {
      "id": "user_123",
      "name": "김중개",
      "role": "AGENT"
    }
  }
}
```

***

## 크레딧 API

### **크레딧 구매**

```http
POST /api/v1/credits/purchase
Content-Type: application/json
Authorization: Bearer {token}

# Request Body
{
  "amount": 1000,
  "paymentMethod": "CARD",
  "cardInfo": {
    "number": "masked_number",
    "expiry": "MM/YY",
    "cvc": "***"
  }
}

# Response 200 OK
{
  "status": "success",
  "data": {
    "transactionId": "tr_123abc",
    "credits": {
      "purchased": 1000,
      "bonus": 100,
      "total": 1100
    },
    "receipt": {
      "url": "https://...",
      "amount": 100000
    }
  }
}
```

***

## HOT 매물 API

### **TOP 5 매물 조회**

```http
GET /api/v1/properties/hot
?period=daily

# Response 200 OK
{
  "status": "success",
  "data": {
    "period": {
      "start": "2024-01-04T00:00:00Z",
      "end": "2024-01-04T23:59:59Z"
    },
    "properties": [
      {
        "id": "prop_123abc",
        "rank": 1,
        "previousRank": 3,
        "metrics": {
          "score": 95.5,
          "viewCount": 500,
          "likeCount": 50
        },
        "property": {
          // Property 상세 정보
        }
      }
    ]
  }
}
```

