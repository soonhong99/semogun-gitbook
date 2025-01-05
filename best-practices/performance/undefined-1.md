# 벤치마킹 가이드

## 성능 측정 도구

```typescript
// 1. API 응답 시간 측정
class APIPerformanceMonitor {
  private metrics: Map<string, number[]> = new Map();

  measureResponseTime(endpoint: string, duration: number) {
    if (!this.metrics.has(endpoint)) {
      this.metrics.set(endpoint, []);
    }
    this.metrics.get(endpoint)!.push(duration);
  }

  getStatistics(endpoint: string) {
    const times = this.metrics.get(endpoint) || [];
    return {
      avg: this.calculateAverage(times),
      p95: this.calculatePercentile(times, 95),
      p99: this.calculatePercentile(times, 99),
      count: times.length
    };
  }
}

// 2. 부하 테스트 설정
interface LoadTestConfig {
  endpoint: string;
  method: 'GET' | 'POST' | 'PUT' | 'DELETE';
  concurrent: number;
  duration: number;
  rampUp: number;
}

class LoadTester {
  async runTest(config: LoadTestConfig) {
    const results: TestResult[] = [];
    const startTime = Date.now();

    while (Date.now() - startTime < config.duration) {
      const batchResults = await Promise.all(
        Array(config.concurrent).fill(null).map(() =>
          this.makeRequest(config)
        )
      );
      results.push(...batchResults);
    }

    return this.analyzeResults(results);
  }
}
```

***

## 벤치마크 시나리오

```typescript
// 1. 시나리오 정의
const BENCHMARK_SCENARIOS = {
  property_list: {
    name: '매물 목록 조회',
    endpoint: '/api/v1/properties',
    acceptance: {
      avg_response_time: 200, // ms
      p95_response_time: 500, // ms
      error_rate: 0.01 // 1%
    }
  },
  property_search: {
    name: '매물 검색',
    endpoint: '/api/v1/properties/search',
    acceptance: {
      avg_response_time: 300,
      p95_response_time: 700,
      error_rate: 0.02
    }
  }
};

// 2. 벤치마크 실행기
class BenchmarkRunner {
  async runScenarios() {
    const results = {};
    for (const [key, scenario] of Object.entries(BENCHMARK_SCENARIOS)) {
      results[key] = await this.runScenario(scenario);
    }
    return this.generateReport(results);
  }
}
```
