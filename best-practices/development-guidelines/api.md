# API 호출 최적화

## 페이지네이션 최적화

```typescript
interface PaginationQuery {
  cursor?: string;
  limit: number;
  sortBy?: string;
  direction?: 'asc' | 'desc';
}

class PropertyRepository {
  async findWithCursor(query: PaginationQuery): Promise<PaginatedResult<Property>> {
    const { cursor, limit, sortBy = 'createdAt', direction = 'desc' } = query;
    
    // 커서 기반 쿼리 생성
    const whereClause = cursor 
      ? { [sortBy]: { [direction === 'desc' ? 'lt' : 'gt']: cursor } }
      : {};

    const properties = await this.prisma.property.findMany({
      where: whereClause,
      take: limit + 1,
      orderBy: { [sortBy]: direction },
    });

    // 다음 페이지 존재 여부 확인
    const hasNext = properties.length > limit;
    if (hasNext) properties.pop();

    // 다음 커서 생성
    const nextCursor = hasNext ? properties[properties.length - 1][sortBy] : null;

    return {
      items: properties,
      hasNext,
      nextCursor
    };
  }
}
```

***

## N+1 문제 해결

```typescript
// 잘못된 예시
async function getPropertiesWithAgent() {
  const properties = await Property.findAll();
  // N+1 문제 발생!
  for (const property of properties) {
    property.agent = await Agent.findById(property.agentId);
  }
}

// 최적화된 예시
async function getPropertiesWithAgent() {
  const properties = await Property.findAll({
    include: [{
      model: Agent,
      attributes: ['id', 'name', 'phone']
    }],
    // 필요한 필드만 선택
    attributes: [
      'id', 'title', 'price', 
      'status', 'createdAt'
    ]
  });
}
```

***

## 벌크 작업 최적화

```typescript
class BulkOperationService {
  async bulkUpdateProperties(updates: PropertyUpdate[]): Promise<Result> {
    // 청크 단위로 분할
    const chunks = this.chunkArray(updates, 100);
    const results = [];

    for (const chunk of chunks) {
      const result = await this.prisma.$transaction(async (tx) => {
        const updates = chunk.map(update => 
          tx.property.update({
            where: { id: update.id },
            data: update.data
          })
        );
        return Promise.all(updates);
      });
      results.push(...result);
    }

    return results;
  }
}
```
