# 시스템 아키텍처

## 주요 특징

* 마이크로 서비스 기반 구조
* 이벤트 기반 통신
* 분산 캐싱 시스템

### 컴포넌트 다이어 그램

```mermaid fullWidth="true"
graph TD
    Client[클라이언트] --> Gateway[API Gateway]
    Gateway --> Auth[인증/인가 서비스]
    Gateway --> Property[매물 관리 서비스]
    Gateway --> Search[검색 서비스]
    Gateway --> Ranking[HOT 랭킹 서비스]
    Gateway --> Credit[크레딧 서비스]
    Gateway --> Agent[중개사 서비스]

    Property --> ImageS3[(이미지 저장소/S3)]
    Property --> MainDB[(PostgreSQL)]
    Property --> AI[AI 분석 서비스]

    Search --> ES[(Elasticsearch)]
    Search --> RedisCache[(Redis Cache)]

    Ranking --> RedisCache
    Ranking --> MainDB

    Credit --> MainDB
    Credit --> Payment[결제 서비스]

    Auth --> UserDB[(사용자 DB)]
    Auth --> RedisSession[(Redis Session)]

    Agent --> MainDB
    Agent --> NotificationQ[알림 큐]

    NotificationQ --> EmailSVC[이메일 서비스]
    NotificationQ --> SMSSVC[SMS 서비스]
```



#### 시스템의 주요 서비스들과 그들 간의 관계

* API Gateway를 통한 중앙 집중식 접근 제어
* 각 핵심 기능별 마이크로서비스 구조
* 데이터 저장소의 분리 (PostgreSQL, Redis, Elasticsearch)
* 비동기 처리를 위한 메시지 큐 시스템

***

### 시퀀스 다이어 그램

```mermaid fullWidth="true"
sequenceDiagram
    participant A as Agent(중개사)
    participant G as API Gateway
    participant P as Property Service
    participant AI as AI Service
    participant DB as PostgreSQL
    participant S3 as Image Storage
    participant N as Notification Service

    A->>G: 매물 등록 요청
    G->>P: 매물 데이터 전달
    P->>S3: 이미지 업로드
    S3-->>P: 이미지 URL 반환
    P->>AI: 매물 정보 분석 요청
    AI->>AI: 특성 추출 및 태그 생성
    AI-->>P: 분석 결과 반환
    P->>DB: 매물 정보 저장
    DB-->>P: 저장 완료
    P->>N: 신규 매물 알림 발송
    N-->>A: 등록 완료 알림
    P-->>G: 등록 완료 응답
    G-->>A: 최종 응답
```



매물 등록 프로세스 흐름

* 중개사의 매물 등록 요청부터 최종 응답까지의 과정
* AI 서비스를 통한 매물 정보 분석
* 비동기 알림 처리
