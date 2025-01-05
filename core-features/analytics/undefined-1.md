# 통계 데이터 활용 가이드

## 제공 데이터 종류

### 1. 기본 통계

```json
{
  "basic_metrics": {
    "daily_stats": {
      "total_views": "일간 총 조회수",
      "unique_visitors": "순 방문자 수",
      "average_duration": "평균 체류 시간",
      "peak_hours": "최다 방문 시간대"
    },
    "property_stats": {
      "view_trend": "시간대별 조회 추이",
      "like_ratio": "좋아요율",
      "conversion_rate": "문의전환율"
    }
  }
}
```

### 2. 고급 분석

```json
{
  "advanced_analytics": {
    "user_behavior": {
      "visit_patterns": "방문 패턴 분석",
      "interest_categories": "관심 매물 유형",
      "search_patterns": "검색어 트렌드"
    },
    "market_insights": {
      "price_trends": "가격 동향",
      "popular_areas": "인기 지역",
      "property_preferences": "선호 매물 특성"
    }
  }
}
```

***

## 데이터 시각화 예시

```typescript
// 시계열 데이터 차트 구성
const timeSeriesConfig = {
  chart: {
    type: 'line',
    metrics: ['views', 'likes', 'inquiries'],
    timeUnit: 'hour',
    aggregation: 'sum'
  },
  filters: {
    dateRange: 'last_7_days',
    propertyType: 'all'
  }
};

// 히트맵 구성
const heatmapConfig = {
  chart: {
    type: 'heatmap',
    metrics: 'view_density',
    dimensions: ['hour_of_day', 'day_of_week']
  }
};
```

***

## 데이터 활용 예시

### 1. 매물 최적화

```json
{
  "optimization_insights": {
    "best_posting_time": "요일/시간대별 효과",
    "optimal_price_range": "시장 반응 기반",
    "effective_keywords": "검색 데이터 기반",
    "photo_effectiveness": "사진별 체류시간"
  }
}
```

### 2. 마케팅 인사이트

```json
{
  "marketing_insights": {
    "target_audience": "주요 관심층 분석",
    "promotional_timing": "프로모션 최적 시점",
    "content_strategy": "효과적인 매물 소개 방식"
  }
}
```

***

## 데이터 접근 권한

| 데이터 유형  | 일반 사용자 | 중개사 | 관리자 |
| ------- | ------ | --- | --- |
| 기본 통계   | ✅      | ✅   | ✅   |
| 트렌드 분석  | ❌      | ✅   | ✅   |
| 상세 인사이트 | ❌      | 💰  | ✅   |
| 원시 데이터  | ❌      | ❌   | ✅   |

> 💰: 크레딧 사용 시 접근 가능
