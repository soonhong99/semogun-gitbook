# 크레딧 정책

## 크레딧 개요

```json
{
  "credit_types": {
    "paid_credit": {
      "description": "구매한 크레딧",
      "validity": "영구적",
      "transferable": false
    },
    "free_credit": {
      "description": "보너스/이벤트 크레딧",
      "validity": "발급일로부터 30일",
      "transferable": false
    },
    "earned_credit": {
      "description": "활동으로 얻은 크레딧",
      "validity": "발급일로부터 90일",
      "transferable": false
    }
  }
}
```

***

## 크레딧 가격 정책

| 구매 금액    | 크레딧 수량 | 보너스 | 총 크레딧 |
| -------- | ------ | --- | ----- |
| 10,000원  | 100    | 0   | 100   |
| 30,000원  | 300    | 30  | 330   |
| 50,000원  | 500    | 75  | 575   |
| 100,000원 | 1,000  | 200 | 1,200 |
