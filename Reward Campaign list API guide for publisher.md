# Reward-CPC Campaign API guide for publisher

***

## Campaign list API

### Getting Started
ADBC에서 제공하는 리워드 캠페인을 API를 통하여 확인합니다 \
Campaign list API 연동이 필요 없을 경우, \
아래 "링크 템플릿 정의" 항목으로 이동해주세요

# 1.기본 정보

- 1분마다 실데이터로 반영되므로 1분 주기로 업데이트 받는 것을 추천드립니다
  - **클라이언트에서 직접 호출 금지. 서버에서 요청 후 캐싱하여 사용하여야합니다**
- method : GET
- domain : http://api.adbc.io/api/v1/reward/campaigns
- parameter : token (mandatory), page (optional)
- 요청 예시 : http://api.adbc.io/api/v1/reward/campaigns?token={token}&page=1
- token은 발급 후 전달드립니다



# 2.응답 정보

| 파라미터명 | 설명 | 예시 | 비고  |
| ------ | ------ | ------ | ------ |
| result | 응답 결과 코드 | 200 | 에러 코드:501, 502, 503, 504 |
| message | 에러 사유 | token is wrong. Require valid token. | 에러 코드일 경우에만 기재 |
| total_pages | 총 페이지 수 | 1 |  |
| current_page | 현재 페이지 | 1 |  |
| total_campaigns | 총 캠페인 수 | 13 |  |
| campaigns_current_page | 현재 페이지의 캠페인 수 | 13 |  |
| campaigns_per_page | 페이지당 보여줄 수 있는 캠페인 수 | 1000 |  |
| campaigns.campaign_id | 캠페인 고유번호 | 11950 | 같은 캠페인이어도 플랫폼별로 구분 |
| campaigns.common_campaign_id | 캠페인 고유번호 | 55071022 | 같은 캠페인이면 동일 |
| campaigns.campaign_name_en | 영문 캠페인명 | UsemapDefenseOnline_NCPI |  |
| campaigns.campaign_name_kr | 한글 캠페인명 | 유즈맵디펜스온라인_NCPI |  |
| campaigns.description | 앱설명 |  |  |
| campaigns.status | 캠페인의 집행 상태 | Active | 진행중:Active, 당일소진:DayOff |
| campaigns.platform | 타겟 플랫폼 | Android | Android, iOS |
| campaigns.tracking_url | 트래킹 URL | https://adbc.io/159750794/{매체고유번호} |  |
| campaigns.dest_url | 목적지 URL | https://naver.com |  |
| campaigns.package_name | 패키지네임 | com.nhn.android.search |  |
| campaigns.target_country | 타겟 국가 | kr |  |
| campaigns.price_krw | 단가 | 1 | 원 단위 |
| campaigns.price_type | 수익형태 | RCPC | 현재 지원 형식 : RCPC |
| campaigns.start_date | 시작일 | 2019-06-17T17:07:00+0900 |  |
| campaigns.end_date | 종료일 | 2019-06-30T23:45:00+0900 |  |
| campaigns.click_cap | 일일 캡 | 500 |  |
| campaigns.click_remain_cap | 당일 남은 캡 | 150 |  |
| campaigns.frequency | 디바이스당 노출 제한 | 5 |  |
| campaigns.img_link | 소재 이미지 링크 |  |  |
| campaigns.guideline | 가이드라인 |  |  |
| campaigns.currency | 재화단위 | KRW |  |
| campaigns.game | 카테고리 게임 여부 | false | true, false |



***

## 링크 템플릿 정의

adbc에서 귀사에게 트래킹 링크를 발급할 때 어떤 형태로 파라미터를 추가할지 정의합니다

| Parameter | 설명 | 예시 | 필수 |
| ------ | ------ | ------ | ------ |
| sub1 | 포스트백으로 전달받을 값 1 (클릭ID) | aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee | ○ | 
| sub2 | 포스트백으로 전달받을 값 2 |  |  |
| sub3 | 포스트백으로 전달받을 값 3 |  |  |
| sub4 | 포스트백으로 전달받을 값 4 |  |  |
| sub5 | 포스트백으로 전달받을 값 5 |  |  |
| aff_id | 귀사의 매체 식별 값 | app12345_1234 |  |
| gaid | 구글 광고ID | 5195b3c1-0a6b-4c60-a8f2-1cc997e3dfc9 | ○ (only android) |
| idfa | 애플 IDFA | 97CC2CBC-544A-4A08-99E0-C450A5D2D8DF | ○ (only iOS) |  

- ###### 링크 템플릿 예시
```
https://adbc.io?sub1={click_id}&aff_id={affiliate_id}&gaid={gaid}&idfa={idfa}
```

***


## 포스트백 정의

포스트백에 아래의 값을 추가하여 캠페인번호, GAID, IDFA 등의 값을 전달받을 수 있습니다

| Macro | 설명 | 예시 | 필수 |
| ------ | ------ | ------ | ------ |
| {sub1} | 트래킹 링크의 sub1 파라미터에 넣은 값. 일반적으로 Click ID를 전달받기 위해 사용 | aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee | ○ |
| {sub2} | 트래킹 링크의 sub2 파라미터에 넣은 값 |  |
| {sub3} | 트래킹 링크의 sub3 파라미터에 넣은 값 |  |
| {sub4} | 트래킹 링크의 sub4 파라미터에 넣은 값 |  |
| {sub5} | 트래킹 링크의 sub5 파라미터에 넣은 값 |  |
| {cid} | 캠페인번호 | 13228 |  |
| {gaid} | 구글 광고 ID |  |
| {idfa} | 애플 광고 ID |  |

- ###### 포스트백 예시
```
http://postback.publisher.com?click_id={sub1}&aid={gaid}&idfa={idfa}
```

## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





