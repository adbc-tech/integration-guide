# Reward Campaign API guide for publisher

### Getting Started

- 본 가이드는 ADBC와 매체사간의 S2S API 연동에 대한 내용입니다 
---
# 1.캠페인 리스트 조회 API

- 5분 주기로 업데이트 받는 것을 추천드립니다
- method : GET
- production API url : https://api.adbc.io/api/v3/reward/campaigns
- test API url : https://dev.qtbit.co.kr:8431/api/v3/reward/campaigns
- parameter : token (필수), p (필수)


#### 참고 : 방화벽 등 보안 설정이 되어 있는 경우 아래 주소로 호출해주세요
- <code>URL : https://api.v3.adbc.io/api/v3/reward/campaigns</code>
- <code>허용할 IP : 3.37.102.179, 43.202.31.186</code>
- <code>허용할 Port : 443</code>

| 항목    | 설명      | 필수 | 비고                                              |
|-------|---------|----|-------------------------------------------------|
| token | 매체사 식별키 | O  |                                                 |
| p     | 플랫폼     |    | 기본 값은 전체이며, 구분하여 받을 경우 Android : 1, iOS : 2로 요청 |
- 요청 예시 : https://api.adbc.io/api/v3/reward/campaigns?token={token}
- token은 발급 후 전달드립니다



| 항목                      | 형태     | 설명                                     | 비고                                                                 |
|-------------------------|--------|----------------------------------------|--------------------------------------------------------------------|
| result                  | int    | 응답 결과 값                                | 200인 경우에만 정상, '결과 코드' 참고                                           |
| cnt                     | int    | 캠페인 수                                  |                                                                    |
| camp                    | array  | 캠페인 리스트                                |                                                                    |
| camp.campid             | int    | 캠페인 식별값                                |                                                                    |
| camp.os                 | string | 모바일 OS                                 | android, ios, WEB (WEB은 모든 OS 호환)                                  |
| camp.name               | string | 캠페인명                                   |                                                                    |
| camp.bm                 | int    | 캠페인 유형                                 | 1: CPA, 2:CPE, 3:CPI, 4:CPS, 0: 기타                                 |
| camp.sub_type           | int    | 캠페인 하위 유형(bm 값이 1일 경우)                 | 0: 기본, 1: 플레이스 저장하기, 2:퀴즈맞추기, 3:스토어상품찜, 4:스토어 알림받기, 5: 유튜브, 6: SNS |
| camp.detail_type        | string | 캠페인 하위 유형(bm 값이 1일 경우)                 | *아래 표 참고                                                           |
| camp.package            | string | 패키지네임 또는 url scheme                    | android: package name, iOS: custom url                             |
| camp.price              | int    | 캠페인 집행 단가 (=매체비)                       | 100                                                                |
| camp.price_dollar       | float  | 캠페인 집행 단가 (=매체비)                       | 0.01                                                               |
| camp.rewarddesc         | string | 캠페인 적립 조건 안내                           | ex) 앱 설치 후 회원가입 완료                                                 |
| camp.joindesc           | string | 캠페인 참여 방법 안내                           |                                                                    |
| camp.totalquantity      | int    | 해당 캠페인의 참여 가능한 총 수량                    | 0: 무제한                                                             |
| camp.quantity           | int    | 해당 캠페인의 일별 참여 가능 수량(=매체사에 할당된 데일리 캡)   | 0: 무제한                                                             |
| camp.enddate            | string | 캠페인 종료일                                | 1) 값이 있는 경우: YYYYMMDD 2) 값이 없는 경우: 별도 종료일 없음                       |
| ~~camp.ad_event_limit~~ | int    | ~~사용자당 참여 가능 횟수~~                      | deprecated                                                         |
| camp.day_event_limit    | int    | 일일 사용자당 참여 가능 횟수                       | 0: 단 한번만 참여 가능, 1: 하루 한번 참여 가능, N: 하루 N번 참여 가능                     |
| camp.targetcarrier      | string | 통신사 타겟팅                                | 공백: 타겟 없음, 1: KT, 2: LGU+, 3: SKT                                  |
| camp.targetgender       | int    | 성별 타겟팅                                 | 0: 타겟 없음, 1: 남자, 2: 여자                                             |
| camp.targetagemin       | int    | 최소 연령 타겟팅                              | 0: 타겟 없음                                                           |
| camp.targetagemax       | int    | 최대 연령 타겟팅                              | 0: 타겟 없음                                                           |
| camp.targetpkg          | string | 앱 패키지네임 타겟팅. AOS 캠페인일 경우에만 내려옵니다       |                                                                    |
| camp.detargetpkg        | string | 앱 패키지네임 디타겟팅. AOS 캠페인일 경우에만 내려옵니다      |                                                                    |
| camp.iconurl            | string | 캠페인의 아이콘 이미지 url                       |                                                                    |
| camp.ctv                | array  | 아이콘 이미지를 제외한 가로형, 세로형 소재 정보를 포함하고 있습니다 | 세부 항목 참고                                                           |
| camp.cpsprice(*)        | int    | 상품의 할인 전 가격                            | bm=4인, cps 캠페인 전용                                                  |
| camp.cpsdcprice(*)      | int    | 상품의 실제 구매 가격                           | bm=4인, cps 캠페인 전용                                                  |

(*) : 해당 사항이 없을 경우, 응답 항목이 전달되지 않습니다.

camp -> ctv 세부 항목

| 항목   | 형태     | 설명                            |
|------|--------|-------------------------------|
| type | int    | 1:가로형 소재, 1200*600, 2:1 ratio |
| type | int    | 2:세로형 소재, 720*780             |
| url  | string | 소재 url                        |

camp -> detail_type 세부 항목

| detail_type                  | 설명                |
|------------------------------|-------------------|
| place_save_default           | 기본 플레이스 저장하기      |
| place_save_share             | 플레이스 URL 공유하기     |
| place_save_click             | 업체 클릭 후 저장        |
| place_save_home              | 플레이스 홈 저장         |
| place_save_keep              | keep 공유           |
| place_save_noti              | 알림받기              |
| place_save_tab               | 검색 후 정답 입력        |
| cpc_detail_ceo_name          | 대표자명 맞추기          |
| cpc_detail_click_tag         | 상품 클릭 후 태그 단어 맞추기 |
| cpc_detail_click_ceo_name    | 상품 클릭 후 대표자명 맞추기  |
| cpc_detail_place             | 플레이스 퀴즈           |
| cpc_detail_place_quiz        | 서치 플레이스 퀴즈        |
| pick_shop_default            | 기본 상품찜            |
| pick_shop_click              | 상품 클릭 후 상품찜       |
| receive_notification_default | 기본 알림받기           |
| receive_notification_click   | 상품 클릭 후 알림받기      |
| video_length_default         | 시청하기              |
| youtube_subs_default         | 구독하기              |
| short_like_default           | 쇼츠 좋아요            |
| youtube_like_default         | 영상 좋아요            |
| youtube_like_subs_default    | 영상 좋아요 & 채널 구독    |
| sns_instagram_follow         | 인스타그램 팔로우         |


#### 응답 예시
```json
{
    "result": 200,
    "cnt": 2,
    "camp": [
        {
          "campid": 415405,
          "name": "A 강남역 맛집 우대포 강남역점 육회 명소 퀴즈맞추기",
          "bm": 1,
          "sub_type": 2,
          "detail_type": "cpc_detail_click_tag",
          "price": 20,
          "price_dollar": 0.014,
          "rewarddesc": "퀴즈 정답 입력하기",
          "joindesc": "[참여방법]\r\n1. 미션 페이지 클릭하기\r\n2. 강남역 맛집 우대포 강남역점 육회 <- 복사하기, \r\n3. 붙여넣기 후 해당 플레이스 클릭 -> 주변 -> 명소 클릭\r\n4. {명소} 1번째 장소는? 단어를 입력 하면 완료!\r\n\r\n\r\n[주의사항] \r\n이미 참여한 이력이 있다면 리워드가 지급되지 않을 수 있습니다. \r\nWIFI가 아닌 환경에서는 데이터 이용료가 발생할 수 있습니다.",
          "totalquantity": 14000,
          "quantity": 2000,
          "enddate": "20240724",
          "targetcarrier": "",
          "targetgender": 0,
          "targetagemin": 0,
          "targetagemax": 0,
          "targetpkg": "",
          "detargetpkg": "",
          "ad_event_limit": 9999,
          "iconurl": "https://webapp.superap.io/res/quiz_icon.png",
          "ctv": [
            {
              "type": 1,
              "url": "https://webapp.superap.io/res/quiz_1200-600_1.png"
            },
            {
              "type": 2,
              "url": "https://webapp.superap.io/res/quiz_720-780_1.png"
            }
          ]
        },
        {
            "campid": 405080,
            "name": "목포 맛집 목포관광오리탕 오리탕 저장하기",
            "bm": 1,
            "sub_type": 1,
            "detail_type": "place_save_home",
            "price": 19,
            "price_dollar": 0.013,
            "rewarddesc": "플레이스 저장하기",
            "joindesc": "[참여방법]\r\n1. 미션 페이지 클릭 -> 플레이스 더보기 클릭 후\r\n2. 홈 메뉴에서 플레이스 저장 하고 저장된 화면 캡쳐하기 \r\n3. 돌아와서 캡쳐화면 업로드 하면 완료! \r\n\r\n\r\n\r\n\r\n[주의사항]\r\n이미 참여한 이력이 있다면 리워드가 지급되지 않을 수 있습니다.\r\nWIFI가 아닌 환경에서는 데이터 이용료가 발생할 수 있습니다.",
            "totalquantity": 6000,
            "quantity": 200,
            "enddate": "20240728",
            "targetcarrier": "",
            "targetgender": 0,
            "targetagemin": 0,
            "targetagemax": 0,
            "targetpkg": "",
            "detargetpkg": "",
            "ad_event_limit": 1,
            "iconurl": "https://webapp.superap.io/res/place_icon.png",
            "ctv": [
              {
                "type": 1,
                "url": "https://webapp.superap.io/res/place_1200-600_2.png"
              },
              {
                "type": 2,
                "url": "https://webapp.superap.io/res/place_720-780_2.png"
              }
            ]
        }
    ]
}
```

---
# 2.캠페인 참여 요청 API


- 광고에 참여하기 위해서는 캠페인 참여 요청 API를 호출해 참여 가능 여부를 확인해야 합니다.
- 참여 유저의 데이터 정보(성별, 연령 등)를 많이 전달 할 수록 더 많은 캠페인에 참여할 수 있습니다.

### 요청 
- method : GET
- production API url : https://adbc.io/reward/v3/join
- test API url : https://dev.qtbit.co.kr:8432/reward/v3/join

#### 참고 : 방화벽 등 보안 설정이 되어 있는 경우 아래 주소로 호출해주세요
- <code>URL : https://click.adbc.io/reward/v3/join</code>
- <code>허용할 IP : 3.37.102.179, 43.202.31.186</code>
- <code>허용할 Port : 443</code> 


| 항목          | 형태     | 내용                                | 필수 | 비고                                              |
|-------------|--------|-----------------------------------|----|-------------------------------------------------|
| token       | string | 매체사 식별키                           | O  |
| cbparam     | string | 매체사에서 정의한 파라미터                    |    | 해당 파라미터에 값을 넣어 보내주시면 포스트백을 통해 다시 전달받을 수 있는 값입니다 |
| aff_id      | string | 매체사의 하위 매체 아이디 (최대 길이: 100)       |    |                                                 |
| userid      | string | 참여 유저 식별값 (최대 길이: 100)            | O  |
| campid      | int    | 캠페인 식별값                           | O  |
| osver       | string | 참여 단말기 OS 버전 정보                   |    |
| ip          | string | 참여 단말기 IP 정보                      | O  |
| adid        | string | AOS = adid, iOS = idfa            | O  |
| devmodel    | string | 참여 단말기 모델 정보                      |    | 권장                                              |
| devbrand    | string | 참여 단말기 제조자 정보                     |    | 권장                                              |
| mnetwork    | int    | 참여 단말기 네트워크 정보                    |    | 1: 3G, 2: 4G, 3: 5G, 4: WIFI                    |
| carrier     | int    | 참여 단말기 통신사 정보(통신사 타게팅 캠페인일 경우 필수) |    | 권장, 1: KT, 2: LGU+, 3: SKT                      |
| ga          | string | 참여 단말기의 구글 계정을 sha1 암호화           |    | 권장                                              |
| age         | int    | 유저 연령 정보(연령 타겟팅 캠페인일 경우 필수)       |    |                                                 |
| gender      | int    | 참여 유저 성별 정보(성별 타겟팅 캠페인일 경우 필수)    |    | 0: 모름, 1: 남자, 2: 여자                             |
| self_bridge | int    | 브릿지 페이지 구성 여부                     |    | 0: ADBC 브릿지 페이지 사용(기본), 1: 자체 브릿지 페이지 사용        |

### 응답
| 항목       | 형태     | 내용                                   |
|----------|--------|--------------------------------------|
| result   | int    | 처리 결과 코드                             |
| lurl     | string | 캠페인 랜딩 URL                           |
| click_id | string | (자체 브릿지 페이지 사용시) '정답 확인 API' 요청시 필수값 |

---
# 3.CPI 설치 확인 API
- 본 항목은 CPI 캠페인만 해당하는 API입니다
- CPI 캠페인은 매체사가 직접 사용자가 해당 앱의 설치 여부를 확인한 후, 이 API를 통해 설치 완료 요청을 해야 합니다
- 설치 완료 확인이 성공시, ADBC로부터 실적을 수신받게 됩니다

### 요청

- method : GET
- production API url : https://adbc.io/reward/v3/inst
- test API url : https://dev.qtbit.co.kr:8432/reward/v3/inst
- parameter :

| 항목     | 형태     | 내용                                            | 필수 | 비고                   |
|--------|--------|-----------------------------------------------|----|----------------------|
| token  | string | 매체사 식별키                                       | O  |                      |
| campid | int    | 캠페인 식별값                                       | O  |                      |
| userid | string | 참여 유저 식별값 (최대 길이: 100)                        | O  |                      |
| adid   | string | AOS = adid, iOS = idfa                        | O  |
| wvid   | string | Widevine DRM용 ID값 - AOS인 경우                   |    | 권장                   |
| wvslv  | string | Widevine security level(L1/l3) - AOS인 경우      |    | 권장                   |
| ip     | string | 참여 단말기 IP 정보(공인 IP만 인정되고 서버IP 또는 사설IP는 사용 불가) |    | 권장                   |

### 응답

|항목|형태|내용|
|------|------|------|
|result|int|처리 결과 코드|
|tid|string|설치 확인 성공일 경우 전송되는 유니크한 구분 데이터|

---
# 4.실적 전송

- 캠페인을 정상적으로 참여 완료했을 때, 매체사 서버로 리워드 적립 요청을 보냅니다
  (폐사에서 직접 적립 처리하지 않습니다)
- 리워드 적립 요청을 받을 콜백URL을 문서 최하단에 메일로 안내되어 있는 연동 담당자에게 전달합니다

#### 참고 : 방화벽 등 보안 설정이 되어 있는 경우 아래 정보를 허용해주세요
- <code>도메인 : proxy.adbc.io</code>
- <code>허용할 IP : 15.165.252.33</code>


### 매크로

| 항목           | 형태     | 설명                           | 비고       |
|--------------|--------|------------------------------|----------|
| cbparam      | string | 캠페인 참여시 보냈던 cbparam값         |          |
| tid          | string | 실적에 대한 유니크값 (transaction ID) |          |
| subpid       | string | 하위 매체 구분값                    | optional |
| userid       | string | 참여 유저 식별값                    |          |
| campid       | int    | 캠페인 식별값                      |          |
| price        | int    | 수익금                          |          |
| price_dollar | float  | 수익금                          |          |


### 예시

https://postback.com?clk_id={cbparam}&tid={tid}&aff_id={subpid}&uid={userid}&cid={campid}&price={price}


---
# 5.정답 확인 API

- 해당 API는 자체 브릿지 페이지를 사용하는 경우에만 사용합니다
- 해당 API의 응답 중 isMatch가 true인 경우에만 리워드 적립을 진행해주시면 됩니다

### 요청

- method : GET
- production API url : https://sns.adbc.io/check-answer
- test API url : https://dev.qtbit.co.kr:8430/check-answer
- parameter :

| 항목       | 형태     | 내용                        | 필수 | 비고 |
|----------|--------|---------------------------|----|----|
| token    | string | 매체사 식별키                   | O  |    |
| campid   | int    | 캠페인 식별값                   | O  |    |
| text     | string | 사용자 정답 입력값                | O  |    |
| adid     | string | AOS = adid, iOS = idfa    | O  |
| click_id | string | '참여API'의 응답으로 받은 click_id | O  |    |

### 응답

| 항목     | 형태     | 내용                            |
|--------|--------|-------------------------------|
| result | int    | 처리 결과 코드                      |
| tid    | string | 설치 확인 성공일 경우 전송되는 유니크한 구분 데이터 |

---
## 공통
- result로 반환되는 코드 목록입니다

### result code

| 항목  | 내용           |
|-----|--------------|
| 200 | 성공           |
| 400 | 잘못된 요청       |
| 401 | 등록되지 않은 매체   |
| 402 | 유저 정보 오류     |
| 403 | 캠페인 정보 오류    |
| 405 | 참여 이력 오류     |
| 406 | 매체 오류        |
| 407 | 타겟팅 매칭 실패    |
| 500 | 종료된 캠페인      |
| 501 | 중복 참여        |
| 502 | 참여할 수 없는 캠페인 |
| 900 | 시스템 오류       |

## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





