# Non-Reward Postback integration guide for publisher

### Getting Started

ADBC의 NCPI 캠페인을 진행하기 위해 포스트백을 연동합니다  
추가로 귀사에서 사용될 링크 템플릿을 작성하여 ADBC 캠페인의 링크 발급시 사용되는 링크 형태를 미리 정의합니다


### Prerequisites


# 1. Campaign List

- 5분마다 실데이터로 반영되므로 5분 주기로 업데이트 받는 것을 추천드립니다
- method : GET
- url : https://api.adbc.io/api/v1/campaigns
- parameter : token (mandatory), page (optional)
- 요청 예시 : https://api.adbc.io/api/v1/campaigns?token={token}&page=1
- token은 발급 후 전달드립니다

| 파라미터명 | 설명 | 예시 | 비고  |
| ------ | ------ | ------ | ------ |
| result | 응답 결과 코드 | 200 | 에러 코드:501, 502, 503, 504 |
| message | 에러 사유 | token is wrong. Require valid token. | 에러 코드일 경우에만 기재 |
| total_pages | 총 페이지 수 | 1 |  |
| current_page | 현재 페이지 | 1 |  |
| total_campaigns | 총 캠페인 수 | 13 |  |
| campaigns_current_page | 현재 페이지의 캠페인 수 | 10 |  |
| campaigns_per_page | 페이지당 보여줄 수 있는 캠페인 수 | 10 |  |
| campaigns.campaign_id | 캠페인 고유번호 | 11950 | 같은 캠페인이어도 플랫폼별로 구분 |
| campaigns.common_campaign_id | 캠페인 고유번호 | 55071022 | 같은 캠페인이면 동일 |
| campaigns.campaign_name_en | 영문 캠페인명 | UsemapDefenseOnline_NCPI |  |
| campaigns.campaign_name_kr | 한글 캠페인명 | 유즈맵디펜스온라인_NCPI |  |
| campaigns.description | 앱설명 |  |  |
| campaigns.status | 캠페인의 집행 상태 | Active | 진행중:Active, 당일소진:DayOff |
| campaigns.platform | 타겟 플랫폼 | Android | Android, iOS |
| campaigns.tracking_url | 트래킹 URL | https://adbc.io/159750794/{매체고유번호} |  |
| campaigns.target_country | 타겟 국가 | kr |  |
| campaigns.price_krw | 단가 | 1200 | 원 단위 |
| campaigns.price_usd | 단가 | 1 | USD 단위 |
| campaigns.price_type | 수익형태 | CPI | CPI, CPA |
| campaigns.start_date | 시작일 | 2019-06-17T17:07:00+0900 |  |
| campaigns.end_date | 종료일 | 2019-06-30T23:45:00+0900 |  |
| campaigns.daily_cap | 일일 캡 | 500 |  |
| campaigns.daily_remain_cap | 당일 남은 캡 | 150 |  |
| campaigns.icon_link | 캠페인의 아이콘 이미지 링크 |  |  |
| campaigns.guideline | 가이드라인 |  |  |
| campaigns.currency | 재화단위 | KRW |  |



# 2.Postback

- 앱 설치 그리고 이벤트에 대한 액션이 발생할때 호출될 포스트백 주소를 정의합니다
- method : GET
- timeout : 3000ms


| Macro | 설명 | 예시 | 필수 | 인스톨 포스트백 지원 | 이벤트 포스트백 지원 |
| ------ | ------ | ------ | ------ | ------ | ------ |
| {sub1} | 트래킹 링크의 sub1 파라미터에 넣은 값. 일반적으로 Click ID를 전달받기 위해 사용 | aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee | ○ | ○ |  ○|
| {sub2} | 트래킹 링크의 sub2 파라미터에 넣은 값 |  |  | ○ |  ○|
| {sub3} | 트래킹 링크의 sub3 파라미터에 넣은 값 |  |  | ○ |  ○|
| {sub4} | 트래킹 링크의 sub4 파라미터에 넣은 값 |  |  | ○ |  ○|
| {sub5} | 트래킹 링크의 sub5 파라미터에 넣은 값 |  |  | ○ |  ○|
| {install_time} | 앱 설치가 된 시간 | 2019-05-20T19:00:00 |  | ○ |  |
| {event_time} | 이벤트가 발생한 시간 | 2019-05-20T19:30:00 |  |  | ○ |
| {event_name} | 이벤트명 | 2019-05-20T19:30:00 |  |  | ○ |
| {gaid} | Android 광고 식별 ID | 022db29c-d0e2-11e5-bb4c-60f81dca7676 |  | ○ |  ○|
| {idfa} | iOS idfa | 022db29c-d0e2-11e5-bb4c-60f81dca7676 |  | ○ |  ○|
| {cid} | 캠페인ID | 11929 |  | ○ |  ○|
| {price} | 캠페인 단가 | 1200.0 |  | ○ |  ○|
| {evt_price} | 구매 가격 | 35000 |  |  |  ○|
| {evt_quantity} | 구매 수량 | 3 |  |  |  ○|
| {evt_product_id} | 상품번호 | pid1111 |  |  |  ○|
| {evt_currency} | 구매 통화 | krw |  |  |  ○|
| {cps_json} | 구매 정보 JSON 포맷 | {"order_id":"orderid123", "product_id":["p123"], "product_name":["goods_name"], "price":[11000], "quantity":[2], "currency":"KRW"} |  |  |  ○|


- ###### 인스톨 포스트백 예시
```
https://install.publisher.com?click_id={sub1}&inst_date={install_time}&aid={gaid}&idfa={idfa}
```
- ###### 이벤트 포스트백 예시
```
https://event.publisher.com?click_id={sub1}&evt_date={event_time}&aid={gaid}&idfa={idfa}&evt_name={event_name}
```


# 3.Link template

adbc에서 귀사에게 링크를 발급할 때 어떤 형태로 파라미터를 추가할지 정의합니다

| Parameter | 설명 | 예시 | 필수 |
| ------ | ------ | ------ | ------ |
| sub1 | 포스트백으로 전달받을 값 1 (클릭ID) | aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee | ○ | 
| sub2 | 포스트백으로 전달받을 값 2 |  |  |
| sub3 | 포스트백으로 전달받을 값 3 |  |  |
| sub4 | 포스트백으로 전달받을 값 4 |  |  |
| sub5 | 포스트백으로 전달받을 값 5 |  |  |
| aff_id | 귀사의 매체 식별 값 | app12345_1234 | ○ |
| gaid | Google Advertising ID | 5195b3c1-0a6b-4c60-a8f2-1cc997e3dfc9 |  |
| idfa | Identifier for Advertising | 97CC2CBC-544A-4A08-99E0-C450A5D2D8DF |  | 
| ua | user agent | Mozilla/5.0 (Linux; Android 10; SM-G887N Build/QP1A.190711.020; wv) AppleWebKit/537.36 --- (생략) | ○ | 

- ###### 링크 템플릿 예시
```
https://adbc.io?sub1={click_id}&aff_id={affiliate_id}&gaid={gaid}
```

# 4.Sign up

[https://adbc.co.kr](https://adbc.co.kr)로 접속하여 회원가입을 하고 
메뉴의 Integration > Postback Configuration에서 위 내용을 적용합니다


# 5.Test

메뉴의 Campaigns > List 페이지에서 실 집행중인 캠페인 중 하나를 테스트하여 포스트백이 잘 들어오는지 확인합니다
포스트백 연동이 성공적으로 이루어졌거나 문의사항이 있다면 아래 메일로 연락 부탁드립니다

## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





