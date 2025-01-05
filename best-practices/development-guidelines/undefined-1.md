# 캐시 활용 전략

## 다층 캐싱 전략

```typescript
class CacheStrategy {
  constructor(
    private readonly redisCache: RedisCache,
    private readonly memoryCache: MemoryCache
  ) {}

  async get(key: string): Promise<any> {
    // 1. 메모리 캐시 확인
    const memResult = await this.memoryCache.get(key);
    if (memResult) return memResult;

    // 2. Redis 캐시 확인
    const redisResult = await this.redisCache.get(key);
    if (redisResult) {
      // 메모리 캐시 업데이트
      await this.memoryCache.set(key, redisResult, 300); // 5분
      return redisResult;
    }

    return null;
  }
}
```

***

## 캐시 패턴 구현

```typescript
// Cache-Aside 패턴
class CacheAsidePattern {
  async getPropertyWithCache(id: string): Promise<Property> {
    const cacheKey = `property:${id}`;
    
    // 1. 캐시 확인
    let property = await this.cache.get(cacheKey);
    if (property) return property;

    // 2. DB 조회
    property = await this.propertyRepository.findById(id);
    
    // 3. 캐시 업데이트
    if (property) {
      await this.cache.set(cacheKey, property, {
        ttl: 3600, // 1시간
        tags: [`property:${id}`, 'property']
      });
    }

    return property;
  }
}

// Write-Through 패턴
class WriteThroughPattern {
  async updateProperty(id: string, data: Partial<Property>): Promise<Property> {
    // 1. DB 업데이트
    const property = await this.propertyRepository.update(id, data);
    
    // 2. 캐시 즉시 업데이트
    await this.cache.set(
      `property:${id}`, 
      property,
      { ttl: 3600 }
    );

    return property;
  }
}
```

***

## 캐시 무효화 전략

```typescript
class CacheInvalidation {
  // 태그 기반 무효화
  async invalidatePropertyCache(propertyId: string): Promise<void> {
    await this.cache.invalidateByTags([
      `property:${propertyId}`,
      'property-list'
    ]);
  }

  // 패턴 기반 무효화
  async invalidateRegionCache(regionCode: string): Promise<void> {
    await this.cache.invalidateByPattern(
      `properties:region:${regionCode}:*`
    );
  }

  // 계층적 무효화
  async invalidateHierarchicalCache(
    propertyId: string,
    regionCode: string
  ): Promise<void> {
    await Promise.all([
      this.cache.del(`property:${propertyId}`),
      this.cache.invalidateByPattern(`properties:region:${regionCode}:*`),
      this.cache.invalidateByPattern('properties:hot:*')
    ]);
  }
}
```

***

## 캐시 모니터링

```typescript
class CacheMonitor {
  private metrics = {
    hits: 0,
    misses: 0,
    errors: 0,
    latency: [] as number[]
  };

  async trackCacheOperation(operation: () => Promise<any>): Promise<any> {
    const start = performance.now();
    try {
      const result = await operation();
      const duration = performance.now() - start;
      
      this.metrics.latency.push(duration);
      this.metrics[result ? 'hits' : 'misses']++;
      
      return result;
    } catch (error) {
      this.metrics.errors++;
      throw error;
    }
  }

  getMetrics() {
    return {
      ...this.metrics,
      hitRate: this.metrics.hits / (this.metrics.hits + this.metrics.misses),
      avgLatency: this.calculateAverage(this.metrics.latency)
    };
  }
}
```
