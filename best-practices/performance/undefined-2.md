# 모니터링 방법

## 성능 메트릭 수집

```typescript
// 1. 메트릭 수집기
class MetricsCollector {
  private metrics = {
    api: new Map<string, APIMetrics>(),
    db: new Map<string, DBMetrics>(),
    cache: new Map<string, CacheMetrics>()
  };

  // API 메트릭
  trackAPICall(endpoint: string, duration: number, status: number) {
    const metrics = this.getOrCreateMetrics(this.metrics.api, endpoint);
    metrics.calls++;
    metrics.totalDuration += duration;
    metrics.statuses[status] = (metrics.statuses[status] || 0) + 1;
  }

  // DB 메트릭
  trackDBQuery(query: string, duration: number) {
    const metrics = this.getOrCreateMetrics(this.metrics.db, query);
    metrics.queries++;
    metrics.totalDuration += duration;
  }

  // 캐시 메트릭
  trackCacheOperation(key: string, hit: boolean) {
    const metrics = this.getOrCreateMetrics(this.metrics.cache, key);
    metrics.operations++;
    metrics.hits += hit ? 1 : 0;
  }
}
```

***

## 알림 설정

```typescript
// 1. 알림 규칙
const ALERT_RULES = {
  response_time: {
    condition: (value: number) => value > 1000,
    message: 'API 응답 시간이 1초를 초과했습니다',
    severity: 'warning'
  },
  error_rate: {
    condition: (value: number) => value > 0.05,
    message: '에러율이 5%를 초과했습니다',
    severity: 'critical'
  },
  cache_miss_rate: {
    condition: (value: number) => value > 0.3,
    message: '캐시 미스율이 30%를 초과했습니다',
    severity: 'warning'
  }
};

// 2. 알림 매니저
class AlertManager {
  checkMetrics(metrics: Metrics) {
    for (const [rule, config] of Object.entries(ALERT_RULES)) {
      if (config.condition(metrics[rule])) {
        this.sendAlert({
          rule,
          message: config.message,
          severity: config.severity,
          value: metrics[rule]
        });
      }
    }
  }
}
```

***

## 대시보드 구성

```typescript
// 1. 대시보드 데이터 수집
class DashboardDataCollector {
  async collectMetrics(): Promise<DashboardData> {
    return {
      system: await this.collectSystemMetrics(),
      api: await this.collectAPIMetrics(),
      business: await this.collectBusinessMetrics()
    };
  }

  private async collectSystemMetrics() {
    return {
      cpu: await this.getCPUUsage(),
      memory: await this.getMemoryUsage(),
      disk: await this.getDiskUsage()
    };
  }

  private async collectAPIMetrics() {
    return {
      responseTime: await this.getAverageResponseTime(),
      errorRate: await this.getErrorRate(),
      requestRate: await this.getRequestRate()
    };
  }
}

// 2. 실시간 모니터링
class RealTimeMonitor {
  private readonly ws: WebSocket;
  private readonly metrics: MetricsCollector;

  startMonitoring() {
    setInterval(() => {
      const data = this.metrics.collect();
      this.ws.send(JSON.stringify(data));
    }, 5000); // 5초마다 업데이트
  }
}
```
