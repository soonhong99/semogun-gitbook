# 결제 시스템 연동

## 결제 프로세스 흐름

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant Server
    participant PG as Payment Gateway
    
    User->>Client: 결제 요청
    Client->>Server: 결제 정보 전송
    Server->>PG: 결제 초기화
    PG-->>Server: 결제 토큰
    Server-->>Client: 결제 페이지 URL
    Client->>PG: 결제 진행
    PG-->>Server: 결제 완료 웹훅
    Server->>Server: 결제 검증
    Server-->>Client: 결제 결과
```

***

## PG사 연동 설정

```typescript
interface PaymentConfig {
  // PG사 설정
  provider: 'toss' | 'inicis' | 'kakao';
  merchantId: string;
  apiKey: string;
  secretKey: string;
  
  // 환경 설정
  mode: 'sandbox' | 'production';
  
  // 콜백 URL
  successUrl: string;
  failureUrl: string;
  webhookUrl: string;
}
```

***

## 결제 구현 예시

```javascript
// 결제 초기화
async function initializePayment(paymentData) {
  try {
    const paymentInfo = {
      amount: paymentData.amount,
      orderId: generateOrderId(),
      orderName: `${paymentData.credits} 크레딧`,
      customerName: paymentData.userName,
      successUrl: `${BASE_URL}/payments/success`,
      failureUrl: `${BASE_URL}/payments/failure`,
      webhookUrl: `${BASE_URL}/payments/webhook`
    };
    
    return await paymentGateway.initialize(paymentInfo);
  } catch (error) {
    logger.error('Payment initialization failed:', error);
    throw new PaymentError('INIT_FAILED', error.message);
  }
}
```

