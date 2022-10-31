# CPS integration guide for publisher

### Getting Started

This guide is for CPS integration.


1. Tracking link integration
2. Report API integration

### overview

- The purchase history on the day will be collectively reflected at 16:00 in KST the next day
- The final confirmed results will be reflected at 16:00(KST) on the 1st of the following month
- Tokens required for API inquiry will be delivered by adbc support team


# 1.Tracking link integration

- url : https://adbc.io/{campaign_code}/{publisher_code}
- method : GET
- {campaign_code} and {publisher_code} is automatically created and delivered when the link is issued by ADBC
- parameters : 

| parameter name | description | sample | remark  |
| ------ | ------ | ------ | ------ |
| tp | market code | coupang | coupang, tmon |
| url | product page url | https%3A%2F%2Fwww.coupang.com%2F  vp%2Fproducts%2F207040%3F  itemId%3D2698315%26  vendorItemId%3D3000303300 | URL Encoding mandatory |
| sub1 | click id | (various) | Can be checked in the report API |
| aff_id | sub affiliate code | channel1 | insert the affiliates you have  |

- response :  
### 302 redirect to market url






# 2.Report API integration

- url : https://api.adbc.io/api/cps/report
- method : GET
- content-type : application/json;utf-8
- Token value can be issued by requesting ADBC support team  
- 500 items per page
- parameters :

| parameter name | description | sample | remark  |
| ------ | ------ | ------ | ------ |
| tp | market code | coupang | coupang, tmon |
| token | auth token code | f9b79e97f58.......... |  |
| page | page number | 1 | default 1 |
| start_date | start date | 20210324 |  |
| end_date | end date | 20210325 |  |

- response :

| name | description | sample | remark  |
| ------ | ------ | ------ | ------ |
| result | response code | 200 | error code:501, 502, 503, 504 |
| message | error reason | token is wrong. Require valid token. | error only |
| total_pages | total number of pages | 5 |  |
| total_items | total number of items | 409 |  |
| item_count | items of current page | 500 |  |
| items_per_page | items per page | 500 |  |
| items.day | purchase date | 2021-03-24 |  |
| items.result | purchase result | S - purchase, F - cancel |  |
| items.order_id | order ID | 8885951911765425 |  |
| items.prd_id | product ID | 1494745371 |  |
| items.prd_name | product name | 샤오미 공기청정기 필터 |  |
| items.option_id | option ID | (various) |  |
| items.quantity | quantity | 2 |  |
| items.purchase | purchase price | 17950 |  |
| items.tp | market name | coupang |  |
| items.aff_id | sub affiliate code | channel1 |  |
| items.sub_param1 | click id | (various) |  |
| items.recent_date | recent update date | 2021-03-25 16:00:00 | |


## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





