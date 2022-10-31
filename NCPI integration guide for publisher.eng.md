# Non-Reward Postback integration guide for publisher


# 1.Campaign List

- Recommend that you get updates every five minutes.
- method : GET
- url : https://api.adbc.io/api/v1/campaigns
- parameter : token (mandatory), page (optional)
- Example : https://api.adbc.io/api/v1/campaigns?token={token}&page=1
- Request {token} value from adbc team


| Parameter | Description | example | notes  |
| ------ | ------ | ------ | ------ |
| result | result code | 200 | error code:501, 502, 503, 504 |
| message | cause of error | token is wrong. Require valid token. | error only |
| total_pages | total pages | 1 |  |
| current_page | current page | 1 |  |
| total_campaigns | total campaign count | 13 |  |
| campaigns_current_page | Number of campaigns on the current page | 10 |  |
| campaigns_per_page | Number of campaigns per page | 10 |  |
| campaigns.campaign_id | campaign ID | 11950 |  |
| campaigns.common_campaign_id | campaign group ID | 55071022 |  |
| campaigns.campaign_name_en | campaign name for eng | UsemapDefenseOnline_NCPI |  |
| campaigns.campaign_name_kr | campaign name for kor | 유즈맵디펜스온라인_NCPI |  |
| campaigns.description | description |  |  |
| campaigns.status | status of campaign | Active | Active, DayOff |
| campaigns.platform | target platform | Android | Android, iOS |
| campaigns.tracking_url | tracking URL | https://adbc.io/159750794/{pub_id} |  |
| campaigns.target_country | target country | kr |  |
| campaigns.price_krw | price for krw | 1200 | won |
| campaigns.price_usd | price for usd | 1 | USD |
| campaigns.price_type | type | CPI | CPI, CPA |
| campaigns.start_date | begin date | 2019-06-17T17:07:00+0900 |  |
| campaigns.end_date | end date | 2019-06-30T23:45:00+0900 |  |
| campaigns.daily_cap | daily capacity | 500 |  |
| campaigns.daily_remain_cap | remain capacity | 150 |  |
| campaigns.icon_link | icon image url |  |  |
| campaigns.guideline | guideline |  |  |
| campaigns.currency | currency | KRW |  |

# 2.Postback

- Define Install, Event Postback URL
- method : GET
- timeout : 3000ms


| Macro | description | ex | mandatory | install | event |
| ------ | ------ | ------ | ------ | ------ | ------ |
| {sub1} | The first publisher passable parameter | aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee | ○ | ○ |  ○|
| {sub2} | The second publisher passable parameter |  |  | ○ |  ○|
| {sub3} | The third publisher passable parameter |  |  | ○ |  ○|
| {sub4} | The fourth publisher passable parameter |  |  | ○ |  ○|
| {sub5} | The fifth publisher passable parameter |  |  | ○ |  ○|
| {install_time} | installed time | 2019-05-20T19:00:00 |  | ○ |  |
| {event_time} | event time | 2019-05-20T19:30:00 |  |  | ○ |
| {event_name} | event name | 2019-05-20T19:30:00 |  |  | ○ |
| {gaid} | Google advertising identifier | 022db29c-d0e2-11e5-bb4c-60f81dca7676 |  | ○ |  ○|
| {idfa} | identifier for advertisers | 022db29c-d0e2-11e5-bb4c-60f81dca7676 |  | ○ |  ○|
| {cid} | adbc campaign ID | 11929 |  | ○ |  ○|
| {price} | payout | 1200.0 |  | ○ |  ○|
| {evt_price} | purchase price | 35000 |  |  |  ○|
| {evt_quantity} | purchase quantity | 3 |  |  |  ○|
| {evt_product_id} | product id | pid1111 |  |  |  ○|
| {evt_currency} | currency | krw |  |  |  ○|
| {cps_json} | purchase info JSON format | {"order_id":"orderid123", "product_id":["p123"], "product_name":["goods_name"], "price":[11000], "quantity":[2], "currency":"KRW"} |  |  |  ○|

- ###### install postback sample
```
https://install.publisher.com?click_id={sub1}&inst_date={install_time}&aid={gaid}&idfa={idfa}
```
- ###### event postback sample
```
https://event.publisher.com?click_id={sub1}&evt_date={event_time}&aid={gaid}&idfa={idfa}&evt_name={event_name}
```


# 3.Link template

define tracking url template

| Parameter | Description | Sample | Mandatory |
| ------ | ------ | ------ | ------ |
| sub1 | The first publisher passable macro (for click id) | aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee | ○ | 
| sub2 | The second publisher passable macro |  |  |
| sub3 | The third publisher passable macro |  |  |
| sub4 | The fourth publisher passable macro |  |  |
| sub5 | The fifth publisher passable macro |  |  |
| aff_id | sub-publisher's id | app12345_1234 | ○ |
| gaid | Google Advertising ID | 5195b3c1-0a6b-4c60-a8f2-1cc997e3dfc9 |  |
| idfa | Identifier for Advertising | 97CC2CBC-544A-4A08-99E0-C450A5D2D8DF |  | 
| ua | user agent | Mozilla/5.0 (Linux; Android 10; SM-G887N Build/QP1A.190711.020; wv) AppleWebKit/537.36 ... | ○ | 

- ###### sample for tracking url template
```
https://adbc.io?sub1={click_id}&aff_id={affiliate_id}&gaid={gaid}
```

# 4.Sign up

Go to [https://adbc.co.kr](https://adbc.co.kr) and signup  
Locate to Integration > Postback Configuration  
Fill the forms with your values 

\
\
\
Feel free to contact us at

## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





