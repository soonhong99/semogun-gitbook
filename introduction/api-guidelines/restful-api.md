# RESTful API 규칙

## URI 설계 규칙

*   복수형 명사 사용

    ```
    ✅ /properties
    ❌ /property
    ```
*   계층 관계 표현

    ```
    ✅ /agents/{agentId}/properties
    ❌ /agent-properties
    ```
*   검색/필터링

    ```
    ✅ /properties?region=gangnam&type=cafe
    ❌ /properties/search/gangnam/cafe
    ```

***

## HTTP 메소드 사용

| 메소드    | 용도        | 예시                      |
| ------ | --------- | ----------------------- |
| GET    | 리소스 조회    | GET /properties         |
| POST   | 리소스 생성    | POST /properties        |
| PUT    | 리소스 전체 수정 | PUT /properties/{id}    |
| PATCH  | 리소스 부분 수정 | PATCH /properties/{id}  |
| DELETE | 리소스 삭제    | DELETE /properties/{id} |

***

## 쿼리 파라미터 규칙

*   페이지네이션

    ```
    /properties?page=1&limit=20
    ```
*   정렬

    ```
    /properties?sortBy=createdAt&orderBy=desc
    ```
*   필터링

    ```
    /properties?region=gangnam&priceMin=1000000&priceMax=5000000
    ```
