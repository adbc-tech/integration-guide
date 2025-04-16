# Order Report integration guide

### Getting Started

구매/취소 실적에 대한 리포트를 제공하는 API 가이드입니다. 

# 기본 정보

- API 조회시 필요한 token은 발급 후 전달드립니다


# 1.리포트 API 연동

- url : https://api.adbc.io/api/report/order?token={token}&from={from}&to={to}
- method : GET
- content-type : application/json;utf-8
- token값은 ADBC support팀에게 요청하여 발급 가능합니다  
- parameters :

| 파라미터명 | 설명 | 예시                    | 비고  |
|-------| ------ |-----------------------| ------ |
| token | 인증 토큰값 | f9b79e97f58.......... | Encoded market URL |
| from  | 조회 시작 날짜 | 20250101              |  |
| to    | 조회 끝 날짜 | 20250110              |  |

- response :

| 변수명                   | 설명       | 예시                                   | 비고  |
|-----------------------|----------|--------------------------------------| ------ |
| result                | 응답 결과 코드 | 200                                  | 에러 코드:501, 502, 503, 504 |
| message               | 에러 사유    | token is wrong. Require valid token. | 에러 코드일 경우에만 기재 |
| total_items           | 총 실적 수   | 10                                   |  |
| items.report_date     | 날짜       | 20250101                             |  |
| items.purchase_count  | 총 구매 횟수  | 6                                    |  |
| items.purchase_amount | 총 구매 금액  | 456700                               |  |
| items.cancel_count    | 총 취소 횟수  | 4                                    |  |
| items.cancel_amount   | 총 취소 금액  | 651210                               |  |
| items.click_count     | 총 클릭 횟수  | 13                                   |  |


## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





