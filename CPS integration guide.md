# CPS integration guide for publisher

### Getting Started

쿠팡, 티몬 등 CPS 캠페인을 진행하기 위한 연동 가이드입니다 
1. 링크 연동
2. 리포트 API 연동

# 기본 정보

- 당일 구매이력은 익일 16시에 일괄 반영됩니다
- 최종 확정 실적은 다음 달 1일 16시에 반영됩니다
- API 조회시 필요한 token은 발급 후 전달드립니다


# 1.링크 연동

- url : https://adbc.io/{campaign_code}/{publisher_code}
- method : GET
- {campaign_code} 와 {publisher_code}는 ADBC에서 링크 발급시 자동으로 생성되어 전달됩니다
- parameters : 

| 파라미터명 | 설명 | 예시 | 비고  |
| ------ | ------ | ------ | ------ |
| tp | 마켓코드 | coupang | coupang, tmon |
| url | 상품페이지 주소 | https%3A%2F%2Fwww.coupang.com%2F  vp%2Fproducts%2F207040%3F  itemId%3D2698315%26  vendorItemId%3D3000303300 | Encoded market URL |
| sub1 | click id | (various) | 리포트 API의 구매 이력에서 확인 가능 |
| aff_id | 귀사의 매체 식별값 | channel1 |  |

- response :  
### 302 redirect to market url






# 2.리포트 API 연동

- url : https://api.adbc.io/api/cps/report
- method : GET
- content-type : application/json;utf-8
- token값은 ADBC support팀에게 요청하여 발급 가능합니다  
- 페이지 당 500건씩 조회됩니다
- parameters :

| 파라미터명 | 설명 | 예시 | 비고  |
| ------ | ------ | ------ | ------ |
| tp | 마켓코드 | coupang | coupang, tmon |
| token | 인증 토큰값 | f9b79e97f58.......... | Encoded market URL |
| page | 페이징 넘버 | 1 | default 1 |
| start_date | 조회 시작 날짜 | 20210324 |  |
| end_date | 조회 끝 날짜 | 20210325 |  |

- response :

| 변수명 | 설명 | 예시 | 비고  |
| ------ | ------ | ------ | ------ |
| result | 응답 결과 코드 | 200 | 에러 코드:501, 502, 503, 504 |
| message | 에러 사유 | token is wrong. Require valid token. | 에러 코드일 경우에만 기재 |
| total_pages | 총 페이지 수 | 5 |  |
| total_items | 총 실적 수 | 409 |  |
| item_count | 현재 페이지의 실적 수 | 500 |  |
| items_per_page | 페이지당 실적 수 | 500 |  |
| items.day | 구매 날짜 | 2021-03-24 |  |
| items.result | 구매/취소 | S - 구매, F - 취소 |  |
| items.order_id | 주문ID | 8885951911765425 |  |
| items.prd_id | 제품ID | 1494745371 |  |
| items.prd_name | 제품명 | 샤오미 공기청정기 필터 |  |
| items.option_id | 옵션ID | (various) |  |
| items.quantity | 구매 수량 | 2 |  |
| items.purchase | 구매 가격 | 17950 |  |
| items.tp | 마켓이름 | coupang |  |
| items.aff_id | 귀사의 매체 식별값 | channel1 |  |
| items.sub_param1 | click id | (various) |  |
| items.recent_date | 최종 상태 변경 시각 | 2021-03-25 16:00:00 | |


## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





