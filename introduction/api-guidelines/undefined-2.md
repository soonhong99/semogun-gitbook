# 버전 관리 정책

## 버전 체계

* Semantic Versioning 2.0.0 준수
* 형식: `v{Major}.{Minor}.{Patch}`
  * Major: 하위 호환성이 깨지는 변경
  * Minor: 하위 호환성 있는 기능 추가
  * Patch: 버그 수정

***

## API 버전 관리

* URL 기반 버전 관리: `/v1/properties`, `/v2/properties`
* 최소 2개의 API 버전 동시 지원
* 이전 버전 지원 기간: 최소 6개월
* 버전 별 지원 상태:
  * CURRENT: 현재 권장 버전
  * SUPPORTED: 지원 중인 이전 버전
  * DEPRECATED: 지원 종료 예정 버전
  * SUNSET: 지원 종료된 버전

***

## 버전 변경 정책

* Major 버전 업데이트
  * 연 1회 이하 실시
  * 3개월 전 사전 공지
  * 마이그레이션 가이드 제공
* Minor 버전 업데이트
  * 분기별 1회 이하 실시
  * 1개월 전 공지
* Patch 버전 업데이트
  * 긴급 버그 수정 시 즉시 적용
  * 정기 패치는 월 1회

