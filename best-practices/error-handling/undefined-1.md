# 디버깅 가이드

## 디버깅 전략

```typescript
// 디버깅 컨텍스트 관리
class DebugContext {
  private context: Map<string, any> = new Map();

  setContext(key: string, value: any) {
    this.context.set(key, value);
  }

  getContext(key: string) {
    return this.context.get(key);
  }

  clearContext() {
    this.context.clear();
  }
}

// 디버그 로그 래퍼
function debugLog(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;

  descriptor.value = async function (...args: any[]) {
    console.debug(`Entering ${propertyKey} with args:`, args);
    const start = performance.now();
    
    try {
      const result = await originalMethod.apply(this, args);
      console.debug(
        `Exiting ${propertyKey}`,
        `Duration: ${performance.now() - start}ms`,
        `Result:`, result
      );
      return result;
    } catch (error) {
      console.debug(`Error in ${propertyKey}:`, error);
      throw error;
    }
  };

  return descriptor;
}
```

***

## 문제 해결 체크리스트

```typescript
class TroubleshootingGuide {
  async checkSystem() {
    const checks = [
      this.checkDatabaseConnection(),
      this.checkRedisConnection(),
      this.checkExternalAPIs(),
      this.checkDiskSpace(),
      this.checkMemoryUsage()
    ];

    const results = await Promise.allSettled(checks);
    return results.map((result, index) => ({
      check: this.getCheckName(index),
      status: result.status,
      value: result.status === 'fulfilled' ? result.value : result.reason
    }));
  }

  private getCheckName(index: number): string {
    const checks = [
      'Database',
      'Redis',
      'External APIs',
      'Disk Space',
      'Memory Usage'
    ];
    return checks[index];
  }
}
```
