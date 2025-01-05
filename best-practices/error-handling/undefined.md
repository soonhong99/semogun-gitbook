# 일반적인 오류 해결

## 에러 분류 체계

```typescript
// 기본 에러 클래스 계층
class BaseError extends Error {
  constructor(
    public code: string,
    message: string,
    public status: number,
    public details?: any
  ) {
    super(message);
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }
}

// 비즈니스 로직 에러
class BusinessError extends BaseError {
  constructor(code: string, message: string, details?: any) {
    super(code, message, 400, details);
  }
}

// 유효성 검증 에러
class ValidationError extends BaseError {
  constructor(code: string, message: string, details?: any) {
    super(code, message, 422, details);
  }
}

// 권한 관련 에러
class AuthorizationError extends BaseError {
  constructor(code: string, message: string) {
    super(code, message, 403);
  }
}
```

***

## 공통 에러 처리

```typescript
// 전역 에러 핸들러
function globalErrorHandler(
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  // 에러 로깅
  logger.error('Unhandled error:', {
    error: {
      name: error.name,
      message: error.message,
      stack: error.stack,
    },
    request: {
      url: req.url,
      method: req.method,
      body: req.body,
      headers: req.headers,
    },
  });

  // 에러 응답 생성
  if (error instanceof BaseError) {
    return res.status(error.status).json({
      status: 'error',
      code: error.code,
      message: error.message,
      details: error.details,
    });
  }

  // 기타 예상치 못한 에러
  return res.status(500).json({
    status: 'error',
    code: 'INTERNAL_SERVER_ERROR',
    message: 'An unexpected error occurred',
  });
}
```

***

## 일반적인 에러 해결 패턴

```typescript
// 재시도 패턴
async function withRetry<T>(
  operation: () => Promise<T>,
  options: RetryOptions
): Promise<T> {
  const { maxAttempts, delay, backoff } = options;
  let attempt = 1;

  while (attempt <= maxAttempts) {
    try {
      return await operation();
    } catch (error) {
      if (attempt === maxAttempts) throw error;
      
      const waitTime = delay * Math.pow(backoff, attempt - 1);
      await new Promise(resolve => setTimeout(resolve, waitTime));
      attempt++;
    }
  }

  throw new Error('Retry failed');
}

// 서킷 브레이커 패턴
class CircuitBreaker {
  private failures = 0;
  private lastFailure: number = 0;
  private state: 'CLOSED' | 'OPEN' | 'HALF_OPEN' = 'CLOSED';

  constructor(
    private threshold: number,
    private timeout: number
  ) {}

  async execute<T>(operation: () => Promise<T>): Promise<T> {
    if (this.state === 'OPEN') {
      if (Date.now() - this.lastFailure > this.timeout) {
        this.state = 'HALF_OPEN';
      } else {
        throw new Error('Circuit breaker is OPEN');
      }
    }

    try {
      const result = await operation();
      this.reset();
      return result;
    } catch (error) {
      this.handleFailure();
      throw error;
    }
  }

  private handleFailure() {
    this.failures++;
    this.lastFailure = Date.now();
    if (this.failures >= this.threshold) {
      this.state = 'OPEN';
    }
  }

  private reset() {
    this.failures = 0;
    this.state = 'CLOSED';
  }
}
```
