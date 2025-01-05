# 성능 최적화 팁

## 데이터베이스 최적화

```typescript
// 1. 인덱스 최적화
const RECOMMENDED_INDEXES = {
  properties: [
    {
      name: 'idx_property_location',
      fields: ['region', 'status'],
      why: '지역별 매물 검색 성능 향상'
    },
    {
      name: 'idx_property_metrics',
      fields: ['viewCount', 'likeCount', 'status'],
      why: 'HOT 매물 집계 성능 향상'
    }
  ],
  propertyViews: [
    {
      name: 'idx_views_date',
      fields: ['propertyId', 'createdAt'],
      why: '기간별 조회수 집계 성능 향상'
    }
  ]
};

// 2. 쿼리 최적화 예시
class OptimizedQueries {
  // 배치 처리를 통한 최적화
  async getPropertiesWithRelations(ids: string[]) {
    const properties = await this.prisma.$transaction([
      this.prisma.property.findMany({
        where: { id: { in: ids } }
      }),
      this.prisma.propertyImage.findMany({
        where: { propertyId: { in: ids } }
      }),
      this.prisma.propertyStats.findMany({
        where: { propertyId: { in: ids } }
      })
    ]);

    return this.mergeResults(properties);
  }

  // 페이지네이션 최적화
  async getPaginatedProperties(cursor: string, limit: number) {
    return this.prisma.property.findMany({
      take: limit,
      skip: 1,
      cursor: cursor ? { id: cursor } : undefined,
      orderBy: { createdAt: 'desc' },
      select: {
        id: true,
        title: true,
        price: true,
        // 필요한 필드만 선택
      }
    });
  }
}
```

***

## 캐시 최적화

```typescript
// 1. 다층 캐시 전략
class CacheOptimization {
  private readonly localCache = new NodeCache({ stdTTL: 60 }); // 1분
  private readonly redis: Redis;

  async getWithCaching(key: string) {
    // 1단계: 로컬 캐시
    const localResult = this.localCache.get(key);
    if (localResult) return localResult;

    // 2단계: Redis 캐시
    const redisResult = await this.redis.get(key);
    if (redisResult) {
      this.localCache.set(key, redisResult);
      return redisResult;
    }

    // 3단계: DB 조회
    const dbResult = await this.fetchFromDB(key);
    await this.setCacheHierarchy(key, dbResult);
    return dbResult;
  }

  // 계층적 캐시 설정
  private async setCacheHierarchy(key: string, data: any) {
    this.localCache.set(key, data);
    await this.redis.setex(key, 3600, JSON.stringify(data));
  }
}
```

***

## API 응답 최적화

```typescript
// 1. 응답 압축
import compression from 'compression';
app.use(compression({
  filter: (req, res) => {
    return req.headers['x-no-compression'] ? false : compression.filter(req, res);
  },
  threshold: 0
}));

// 2. JSON 직렬화 최적화
class JSONOptimizer {
  static serialize(data: any) {
    const cache = new WeakSet();
    return JSON.stringify(data, (key, value) => {
      if (typeof value === 'object' && value !== null) {
        if (cache.has(value)) {
          return '[Circular]';
        }
        cache.add(value);
      }
      return value;
    });
  }
}

// 3. 필드 필터링
class ResponseOptimizer {
  static filterFields(data: any, fields: string[]) {
    if (Array.isArray(data)) {
      return data.map(item => this.filterObject(item, fields));
    }
    return this.filterObject(data, fields);
  }

  private static filterObject(obj: any, fields: string[]) {
    const result: any = {};
    fields.forEach(field => {
      if (obj.hasOwnProperty(field)) {
        result[field] = obj[field];
      }
    });
    return result;
  }
}
```
