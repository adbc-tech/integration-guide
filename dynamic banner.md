# CP dynamic banner integration guide

### Getting Started

CPS 캠페인을 진행하기 위해 포스트백을 연동합니다  
추가로 귀사에서 사용될 링크 템플릿을 작성하여 ADBC 캠페인의 링크 발급시 사용되는 링크 형태를 미리 정의합니다


# 1.Postback

- 구매에 대한 이벤트 포스트백을 받습니다
- method : GET
- timeout : 3000ms
- 아래 매크로를 참고하여 귀사가 받을 포스트백 주소를 전달해주세요


| Macro | 설명 | 예시                                   | 필수 |
| ----- | ------ |--------------------------------------| ------ |
| {sub1} | 트래킹 링크의 sub1 파라미터에 넣은 값. 일반적으로 Click ID를 전달받기 위해 사용 | aaaaaaaa-bbbb | ○ |
| {sub2} | 트래킹 링크의 sub2 파라미터에 넣은 값 |                                      |  |
| {sub3} | 트래킹 링크의 sub3 파라미터에 넣은 값 |                                      |  |
| {sub4} | 트래킹 링크의 sub4 파라미터에 넣은 값 |                                      |  |
| {sub5} | 트래킹 링크의 sub5 파라미터에 넣은 값 |                                      |  |
| {cps} | 구매 정보 JSON 포맷 | 아래 참조                                | ○ |


- ###### {cps} 포맷 설명

| parameter         | type   | 설명           |
|-------------------|--------|--------------|
| txid              | String | 트랜잭션ID       |
| clickid           | String | 광고주 트래킹ID    |
| track_id          | String | ADBC 트래킹ID   |
| orderId           | String | 주문번호         |
| status            | String | S: 구매, F: 취소 |
| items.productId   | String | 상품ID         |
| items.productName | String | 상품명          |
| items.gmv         | String | 매출 금액        |
| items.quantity    | String | 수량           |

- ###### {cps} 형식 예시
```
{
  "txid": "8265950777479210",
  "clickid": "6XY00911622794378167770480_CPR",
  "track_id": "e0ShI78ztvUxa-3Kbw8-DzXZEwclQ8z3T8lLhI3QTOM",
  "orderId": "8265950777479210",
  "status": "S",
  "items": [
    {
      "productId": "1729776574",
      "productName": "6다이닝 미국산 살치살 바로구이 (냉동)",
      "gmv": "33600",
      "quantity": "2"
    },
   {
      "productId": "12680336557",
      "productName": "푸드랩플러스 미국산 초이스 살치살 구이용 (냉장)",
      "gmv": "21400",
      "quantity": "1"
    }
  ]
}

```



# 2.Script 

쿠팡 다이나믹배너를 노출하기 위해 아래와 같이 스크립트를 삽입해주세요

```
<script src="https://adbc.io/1147427604/915069528/dynamic_banner.js?sub1={sub1}&w={w}&h={h}&aff_id={aff_id}"></script>
```

| Parameter | 설명                   | 예시            | 필수 |
|-----------|----------------------|---------------| ------ |
| sub1      | 포스트백으로 전달받을 값 (클릭ID) | aaaaaaaa-bbbb | ○ | 
| w         | 배너 가로 사이즈            | 100%          | ○ |
| h         | 배너 세로 사이즈            | 200           | ○ |
| aff_id    | 귀사의 하위 매체 ID         | app1          |  |


- ###### 예시
```
<html>
    <head></head>
    <body>
    
        .. contents .. 
    
        <script src="https://adbc.io/1147427604/915069528?sub1=aaaaaaaa-bbbb&w=100%&h=200&aff_id=app1"></script>    
    </body>
</html>
```


## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





