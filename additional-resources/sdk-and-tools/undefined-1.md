# 유틸리티

## 공통 유틸리티 함수

```typescript
// utils/validation.ts
export const validatePropertyData = (data: PropertyInput): ValidationResult => {
  const errors: ValidationError[] = [];
  
  if (!data.title || data.title.length < 5) {
    errors.push({
      field: 'title',
      message: '제목은 최소 5자 이상이어야 합니다.'
    });
  }
  
  if (!data.price || data.price.deposit < 0) {
    errors.push({
      field: 'price.deposit',
      message: '보증금은 0 이상이어야 합니다.'
    });
  }
  
  return {
    isValid: errors.length === 0,
    errors
  };
};

// utils/formatter.ts
export const formatCurrency = (amount: number): string => {
  return new Intl.NumberFormat('ko-KR', {
    style: 'currency',
    currency: 'KRW'
  }).format(amount);
};
```

***

## 모니터링 유틸리티

```typescript
// monitoring/elastic.ts
import { Client } from '@elastic/elasticsearch';

export class ElasticSearchClient {
  private client: Client;

  constructor() {
    this.client = new Client({
      node: process.env.ELASTICSEARCH_URL,
      auth: {
        username: process.env.ELASTICSEARCH_USERNAME,
        password: process.env.ELASTICSEARCH_PASSWORD
      }
    });
  }

  async indexPropertyData(property: Property): Promise<void> {
    await this.client.index({
      index: 'properties',
      document: {
        id: property.id,
        title: property.title,
        price: property.price,
        location: property.location,
        timestamp: new Date()
      }
    });
  }
}
```

***

## AWS 유틸리티

```typescript
// utils/aws.ts
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';

export class S3Storage {
  private client: S3Client;
  private bucket: string;

  constructor() {
    this.client = new S3Client({
      region: process.env.AWS_REGION
    });
    this.bucket = process.env.AWS_S3_BUCKET;
  }

  async uploadImage(file: Buffer, key: string): Promise<string> {
    await this.client.send(new PutObjectCommand({
      Bucket: this.bucket,
      Key: `images/${key}`,
      Body: file,
      ContentType: 'image/jpeg'
    }));

    return `https://${this.bucket}.s3.amazonaws.com/images/${key}`;
  }
}
```
