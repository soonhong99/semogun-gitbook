# AI 분석 프로세스

## 분석 항목

### 1. 이미지 분석

* 공간 구조 파악
* 시설물 인식
* 인테리어 상태 평가

### 2. 텍스트 분석

* 매물 특성 추출
* 키워드 분석
* 가격 적정성 평가

***

## AI 분석 결과 예시

```json
{
  "ai_analysis": {
    "property_features": {
      "space_type": "open_floor",
      "condition": "renovated",
      "facilities": ["kitchen", "bathroom", "storage"],
      "highlights": ["natural_light", "high_ceiling"]
    },
    "market_analysis": {
      "price_rating": "reasonable",
      "location_rating": "excellent",
      "potential_score": 85
    },
    "suggested_tags": [
      "역세권",
      "신축",
      "주차가능",
      "테라스"
    ]
  }
}
```

***

## AI 분석 활용

1. 자동 태그 생성
2. 매물 추천 시스템
3. 가격 제안
4. 트렌드 분석

***

## 오류 처리

```json
{
  "error_handling": {
    "image_analysis_failed": {
      "fallback": "수동 검토 요청",
      "retry_count": 3
    },
    "text_analysis_failed": {
      "fallback": "기본 태그 적용",
      "manual_review": true
    }
  }
}
```
