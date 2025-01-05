# API 사용 정책

## API 접근 제한

```json
{
  "api_limits": {
    "free_tier": {
      "requests_per_second": 1,
      "daily_limit": 1000,
      "concurrent_connections": 5
    },
    "basic_tier": {
      "requests_per_second": 5,
      "daily_limit": 10000,
      "concurrent_connections": 20
    },
    "premium_tier": {
      "requests_per_second": 20,
      "daily_limit": 100000,
      "concurrent_connections": 50
    }
  }
}
```

***

## API 키 관리

```json
{
  "api_key_policies": {
    "generation": {
      "format": "rpk_live_xxxxxxxxxxxxxxxxx",
      "expiration": "1년",
      "renewal": "만료 30일 전부터 가능"
    },
    "security": {
      "storage": "해시화하여 저장",
      "transmission": "HTTPS only",
      "access_control": "IP 기반 제한 가능"
    }
  }
}
```

***

## 데이터 사용 제한

### 1. 허용되는 사용

* 매물 정보 조회 및 분석
* 통계 데이터 활용
* 사용자 피드백 수집

### 2. 금지되는 사용

* 개인정보 무단 수집
* 데이터 대량 크롤링
* 서비스 복제 또는 모방

***

## 제재 정책

```json
{
  "violation_penalties": {
    "warning": {
      "conditions": ["일일 한도 초과", "경미한 정책 위반"],
      "actions": ["경고 메시지 발송", "일시적 사용 제한"]
    },
    "suspension": {
      "conditions": ["반복적 위반", "심각한 정책 위반"],
      "actions": ["API 키 비활성화", "서비스 이용 정지"]
    },
    "termination": {
      "conditions": ["불법적 사용", "타인의 권리 침해"],
      "actions": ["계정 영구 정지", "법적 조치 진행"]
    }
  }
}
```

***

## 책임과 보증

### 1. 서비스 제공자의 책임

* API 가용성 보장
* 데이터 정확성 유지
* 보안 취약점 관리

### 2. 사용자의 책임

* API 키 보안 관리
* 사용 제한 준수
* 법령 및 규정 준수
