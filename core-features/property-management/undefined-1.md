# 매물 상태 관리

## 상태 전이도

```mermaid
stateDiagram-v2
    [*] --> DRAFT
    DRAFT --> PENDING: 제출
    PENDING --> ACTIVE: 승인
    PENDING --> REJECTED: 거절
    ACTIVE --> COMPLETED: 거래완료
    REJECTED --> DRAFT: 수정
    COMPLETED --> [*]
```



***

## 상태별 기능 제한

| 상태        | 수정 가능 | 조회 가능 | 삭제 가능 |
| --------- | ----- | ----- | ----- |
| DRAFT     | ✅     | ❌     | ✅     |
| PENDING   | ❌     | ✅     | ✅     |
| ACTIVE    | ⚡️    | ✅     | ❌     |
| REJECTED  | ✅     | ❌     | ✅     |
| COMPLETED | ❌     | ✅     | ❌     |

> ⚡️ : 일부 필드만 수정 가능
