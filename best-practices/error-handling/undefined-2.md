# 로깅 가이드

## 로깅 구성

```typescript
// 로깅 설정
interface LogConfig {
  level: 'debug' | 'info' | 'warn' | 'error';
  format: 'json' | 'text';
  destinations: ('console' | 'file' | 'elk')[];
  retention: {
    maxFiles: number;
    maxSize: string;
  };
}

// 로거 구현
class Logger {
  private static instance: Logger;
  private config: LogConfig;

  static getInstance(): Logger {
    if (!Logger.instance) {
      Logger.instance = new Logger();
    }
    return Logger.instance;
  }

  setConfig(config: LogConfig) {
    this.config = config;
  }

  log(level: string, message: string, meta?: any) {
    const logEntry = {
      timestamp: new Date().toISOString(),
      level,
      message,
      ...meta,
      correlationId: this.getCorrelationId()
    };

    this.writeLog(logEntry);
  }
}
```

***

## 구조화된 로깅

```typescript
// 로그 포맷터
class LogFormatter {
  static format(entry: LogEntry): string {
    return JSON.stringify({
      timestamp: entry.timestamp,
      level: entry.level,
      message: entry.message,
      context: {
        service: entry.service,
        environment: entry.environment,
        correlationId: entry.correlationId
      },
      metadata: entry.metadata,
      tags: entry.tags
    });
  }
}

// 로그 수집기
class LogCollector {
  private buffer: LogEntry[] = [];
  private readonly batchSize = 100;

  async collect(entry: LogEntry) {
    this.buffer.push(entry);

    if (this.buffer.length >= this.batchSize) {
      await this.flush();
    }
  }

  private async flush() {
    try {
      await this.sendToLogStore(this.buffer);
      this.buffer = [];
    } catch (error) {
      console.error('Failed to flush logs:', error);
    }
  }
}
```

***

## 로깅 모범 사례

```typescript
class BestPractices {
  // 로그 레벨 가이드라인
  static readonly LOG_LEVELS = {
    ERROR: {
      level: 'error',
      description: '시스템 오류, 즉각적인 조치 필요',
      examples: ['데이터베이스 연결 실패', '크리티컬한 비즈니스 로직 실패']
    },
    WARN: {
      level: 'warn',
      description: '잠재적 문제, 모니터링 필요',
      examples: ['API 응답 지연', '재시도 발생']
    },
    INFO: {
      level: 'info',
      description: '중요 비즈니스 이벤트',
      examples: ['사용자 로그인', '매물 등록 완료']
    },
    DEBUG: {
      level: 'debug',
      description: '개발 및 디버깅용 상세 정보',
      examples: ['함수 호출 파라미터', '중간 처리 결과']
    }
  };

  // 민감 정보 필터링
  static filterSensitiveData(data: any): any {
    const sensitiveFields = ['password', 'token', 'creditCard'];
    return this.recursiveFilter(data, sensitiveFields);
  }

  // 로그 컨텍스트 관리
  static createLogContext(req: Request): LogContext {
    return {
      correlationId: req.headers['x-correlation-id'],
      userId: req.user?.id,
      ipAddress: req.ip,
      userAgent: req.headers['user-agent'],
      path: req.path,
      method: req.method
    };
  }
}
```
