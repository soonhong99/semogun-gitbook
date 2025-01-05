# 코드 예제

## 매물 CRUD 구현

```typescript
// Property Service 구현 예시
class PropertyService {
  constructor(
    private readonly propertyRepository: PropertyRepository,
    private readonly cacheService: CacheService,
    private readonly eventEmitter: EventEmitter
  ) {}

  // 매물 생성
  async createProperty(data: CreatePropertyDto): Promise<Property> {
    try {
      // 1. 입력값 검증
      this.validatePropertyData(data);
      
      // 2. 매물 생성
      const property = await this.propertyRepository.create({
        ...data,
        status: PropertyStatus.DRAFT,
        createdAt: new Date()
      });
      
      // 3. 이벤트 발행
      this.eventEmitter.emit('property.created', { propertyId: property.id });
      
      // 4. 캐시 무효화
      await this.cacheService.invalidate(`properties:*`);
      
      return property;
    } catch (error) {
      throw new PropertyServiceError('CREATE_FAILED', error.message);
    }
  }

  // 매물 상세 조회
  async getPropertyById(id: string): Promise<Property> {
    // 1. 캐시 확인
    const cached = await this.cacheService.get(`property:${id}`);
    if (cached) return cached;

    // 2. DB 조회
    const property = await this.propertyRepository.findById(id);
    if (!property) {
      throw new NotFoundError(`Property ${id} not found`);
    }

    // 3. 캐시 저장
    await this.cacheService.set(`property:${id}`, property, 3600);

    return property;
  }
}
```

***

## 에러 처리 예시

```typescript
// 커스텀 에러 클래스
class PropertyServiceError extends Error {
  constructor(
    public code: string,
    message: string,
    public details?: any
  ) {
    super(message);
    this.name = 'PropertyServiceError';
  }
}

// 에러 핸들러 미들웨어
function errorHandler(
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  if (error instanceof PropertyServiceError) {
    return res.status(400).json({
      status: 'error',
      code: error.code,
      message: error.message,
      details: error.details
    });
  }

  // 기본 에러 응답
  return res.status(500).json({
    status: 'error',
    message: 'Internal Server Error'
  });
}
```
