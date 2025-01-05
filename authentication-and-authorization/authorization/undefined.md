# 권한 체계 설명

## 권한 체계 설명

### 1. 역할(Role) 정의

```json
{
  "roles": {
    "ADMIN": "시스템 관리자",
    "AGENT": "공인중개사",
    "PREMIUM_USER": "프리미엄 사용자",
    "USER": "일반 사용자",
    "GUEST": "비로그인 사용자"
  }
}
```

### 2. 권한(Permission) 정의

```json
{
  "permissions": {
    "property": {
      "create": "매물 등록",
      "read": "매물 조회",
      "update": "매물 수정",
      "delete": "매물 삭제",
      "manage": "매물 관리"
    },
    "credit": {
      "purchase": "크레딧 구매",
      "use": "크레딧 사용",
      "manage": "크레딧 관리"
    },
    "disadvantage": {
      "create": "아사단 작성",
      "read": "아사단 조회",
      "update": "아사단 수정"
    },
    "user": {
      "manage": "사용자 관리",
      "report": "신고 관리"
    }
  }
}
```

