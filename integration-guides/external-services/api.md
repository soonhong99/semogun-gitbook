# 외부 API 연동

## 주소/좌표 변환 API

```typescript
interface GeocodingService {
  // 주소 -> 좌표 변환
  getCoordinates(address: string): Promise<{
    latitude: number;
    longitude: number;
    accuracy: number;
  }>;
  
  // 좌표 -> 주소 변환
  getAddress(lat: number, lng: number): Promise<{
    roadAddress: string;
    jibunAddress: string;
    detail: string;
  }>;
}
```

***

## 외부 API 요청 래퍼

```javascript
class ApiClient {
  constructor(config) {
    this.axios = axios.create({
      baseURL: config.baseUrl,
      timeout: config.timeout || 5000,
      headers: {
        'Authorization': `Bearer ${config.apiKey}`,
        'Content-Type': 'application/json'
      }
    });
    
    // 재시도 설정
    this.axios.interceptors.response.use(
      response => response,
      async error => {
        if (this.shouldRetry(error)) {
          return this.retryRequest(error.config);
        }
        throw error;
      }
    );
  }
  
  async retryRequest(config) {
    // 재시도 로직
  }
}
```

***

## API 통합 예시

```typescript
// 리스팅 정보 통합
interface ListingIntegration {
  // 외부 매물 정보 가져오기
  fetchExternalListings(): Promise<Property[]>;
  
  // 매물 정보 동기화
  syncListings(): Promise<{
    added: number;
    updated: number;
    removed: number;
    errors: Error[];
  }>;
}

// 데이터 변환기
interface DataTransformer {
  // 외부 데이터 -> 내부 형식 변환
  transformToInternalFormat(externalData: any): Property;
  
  // 내부 형식 -> 외부 데이터 변환
  transformToExternalFormat(property: Property): any;
}
```

***

## 에러 처리 및 모니터링

```javascript
class ExternalServiceMonitor {
  constructor() {
    this.metrics = {
      requestCount: 0,
      errorCount: 0,
      responseTime: [],
      lastError: null
    };
  }
  
  async trackRequest(serviceName, operation) {
    const startTime = Date.now();
    try {
      const result = await operation();
      this.recordSuccess(serviceName, Date.now() - startTime);
      return result;
    } catch (error) {
      this.recordError(serviceName, error);
      throw error;
    }
  }
  
  getHealthStatus(serviceName) {
    return {
      status: this.isHealthy(serviceName) ? 'healthy' : 'unhealthy',
      metrics: this.metrics[serviceName]
    };
  }
}
```

***

## 설정 관리

```json
{
  "external_services": {
    "payment": {
      "provider": "toss",
      "config": {
        "production": {
          "apiKey": "prod-api-key",
          "webhookSecret": "prod-secret"
        },
        "development": {
          "apiKey": "dev-api-key",
          "webhookSecret": "dev-secret"
        }
      }
    },
    "notification": {
      "email": {
        "provider": "aws-ses",
        "config": {
          "region": "ap-northeast-2",
          "accessKey": "aws-access-key",
          "secretKey": "aws-secret-key"
        }
      }
    },
    "maps": {
      "provider": "kakao",
      "config": {
        "apiKey": "kakao-maps-api-key",
        "rateLimit": 100
      }
    }
  }
}
```
