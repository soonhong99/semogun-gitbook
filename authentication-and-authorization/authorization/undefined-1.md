# 역할별 권한 매트릭스

## 기본 권한 매트릭스

| 권한                  | ADMIN | AGENT | PREMIUM\_USER | USER | GUEST |
| ------------------- | ----- | ----- | ------------- | ---- | ----- |
| **매물 관리**           |       |       |               |      |       |
| property.create     | ✅     | ✅     | ❌             | ❌    | ❌     |
| property.read       | ✅     | ✅     | ✅             | ✅    | ✅     |
| property.update     | ✅     | ⚡️    | ❌             | ❌    | ❌     |
| property.delete     | ✅     | ⚡️    | ❌             | ❌    | ❌     |
| **크레딧**             |       |       |               |      |       |
| credit.purchase     | ✅     | ✅     | ✅             | ✅    | ❌     |
| credit.use          | ✅     | ✅     | ✅             | ✅    | ❌     |
| credit.manage       | ✅     | ❌     | ❌             | ❌    | ❌     |
| **아사단**             |       |       |               |      |       |
| disadvantage.create | ✅     | ✅     | ❌             | ❌    | ❌     |
| disadvantage.read   | ✅     | ✅     | ✅             | ⚡️   | ❌     |
| disadvantage.update | ✅     | ⚡️    | ❌             | ❌    | ❌     |
| **사용자 관리**          |       |       |               |      |       |
| user.manage         | ✅     | ❌     | ❌             | ❌    | ❌     |
| user.report         | ✅     | ✅     | ✅             | ✅    | ❌     |

> ✅ : 전체 권한 ⚡️ : 본인 소유 리소스만 접근 가능 ❌ : 접근 불가

***

## 특수 조건 권한

### 1. 중개사(AGENT) 특수 권한

* 본인이 등록한 매물만 수정/삭제 가능
* 매물당 하나의 아사단만 작성 가능
* 크레딧 적립 기능 사용 가능

### 2. 프리미엄 사용자(PREMIUM\_USER) 특수 권한

* 아사단 무제한 조회
* 고급 검색 기능 사용
* 실시간 알림 서비스
