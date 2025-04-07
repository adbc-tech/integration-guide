# Reward Campaign API guide for publisher

### Getting Started

- This guide is about S2S API integration between ADBC and Publisher 
---
# 1.Campaign List API

- Recommend updating every 5 minutes
- method : GET
- url : https://api.adbc.io/api/v3/reward/campaigns
- parameter : token (Mandatory), p (Mandatory)

| parameter | description | remark               |
|-----------|-------------|----------------------|
| token     | API_KEY     |                      |
| p         | platform    | Android : 1, iOS : 2 |
| category  | category    |    | Insurance : 01, Finance : 02, Car Insurance : 03, Card : 04, ETC : 99 |
- e.g) : https://api.adbc.io/api/v3/reward/campaigns?token={token}
  



| variable           | type   | description                            | remark                                                                                |
|--------------------|--------|----------------------------------------|---------------------------------------------------------------------------------------|
| result             | int    | result code                            | 200 is success. see below result code table                                           |
| cnt                | int    | number of campaigns                    |                                                                                       |
| camp               | array  | campaign list                          |                                                                                       |
| camp.campid        | int    | campaign ID                            |                                                                                       |
| camp.name          | string | campaign name                          |                                                                                       |
| camp.bm            | int    | campaign type                          | 1: CPA, 2:CPE, 3:CPI, 4:CPS, 0: etc                                                   |
| camp.sub_type      | int    | campaign sub type(bm=1 only)           | 0: default, 1: save place, 2:quiz, 3:favorite store, 4:store notification, 5: youtube |
| camp.package       | string | package name or url scheme             | android: package name, iOS: custom url                                                |
| camp.price         | int    | revenue                                | 100                                                                                   |
| camp.price_dollar  | float  | revenue                                | 0.01                                                                                  |
| camp.rewarddesc    | string | requirements for earning rewards       | ex) registration completed after installing the app                                   |
| camp.joindesc      | string | description for join campaign          |                                                                                       |
| camp.totalquantity | int    | available quantity                     | 0: infinity                                                                           |
| camp.quantity      | int    | daily capacity                         | 0: infinity                                                                           |
| camp.enddate       | string | end date                               | date format : YYYYMMDD. empty value is infinity                                       |
| camp.targetcarrier | string | carrier targeting                      | empty value: no target, 1: KT, 2: LGU+, 3: SKT                                        |
| camp.targetgender  | int    | gender targeting                       | 0: no target, 1: male, 2: female                                                      |
| camp.targetagemin  | int    | minimum age                            | 0: no target                                                                          |
| camp.targetagemax  | int    | maximum age                            | 0: no target                                                                          |
| camp.targetpkg     | string | app package name targeting. AOS only   |                                                                                       |
| camp.detargetpkg   | string | app package name detargeting. AOS only |                                                                                       |
| camp.iconurl       | string | icon image url                         |                                                                                       |
| camp.ctv           | array  | promotional image information          | see below ctv table                                                                   |
| camp.cpsprice(*)   | int    | price before discount                  | when bm is 4(CPS) only                                                                |
| camp.cpsdcprice(*) | int    | price after discount                   | when bm is 4(CPS) only                                                                |



* camp.ctv

| variable | type   | description                       |
|----------|--------|-----------------------------------|
| type     | int    | 1:horizontal, 1200*600, 2:1 ratio |
| type     | int    | 2:vertical, 720*780               |
| url      | string | image url                         |




---
# 2.Join API


- In order to participate in an ad, you need to call the campaign join request API to check if you can participate.
- The more data information (gender, age, etc.) of participating users is delivered, the more campaigns you can participate in.

### REQUEST

- method : GET
- url : https://adbc.io/reward/v3/join
- parameter : 

| parameter | type   | description                              | remark                         |
|-----------|--------|------------------------------------------|--------------------------------|
| token     | string | API_KEY                                  | mandatory                      |
| p         | int    | platform                                 | Android : 1, iOS : 2           |
| cbparam   | string | publisher passable parameter             | mandatory                      |
| aff_id    | string | sub-publisher ID (max length: 100)       ||
| userid    | string | user identification ID (max length: 100) | mandatory                      |
| campid    | int    | campaign ID                              | mandatory                      |
| osver     | string | OS version                               | mandatory                      |
| ip        | string | IP address                               | mandatory                      |
| adid      | string | AOS = adid, iOS = idfa                   | mandatory                      |
| devmodel  | string | device model name                        |                                |
| devbrand  | string | device brand name                        |                                |
| mnetwork  | int    | device network                           | 1: 3G, 2: 4G, 3: 5G, 4: WIFI   |
| carrier   | int    | device carrier                           | 1: KT, 2: LGU+, 3: SKT         |
| age       | int    | user's age                               ||
| gender    | int    | user's gender                            | 0: unknown, 1: male, 2: female |

### RESPONSE
| variable | type   | description  |
|----------|--------|--------------|
| result   | int    | result code  |
| lurl     | string | campaign URL |

---
# 3.CPI install verification API
- This API is for CPI campaigns only
- For CPI campaigns, the publisher must directly check whether the user has installed the app, and then request the installation completion through this API.
- Upon successful confirmation of installation completion, you will receive a postback from ADBC

### REQUEST

- method : GET
- url : https://adbc.io/reward/v3/inst
- parameter :

| parameter | type   | description                              | remark               |
|-----------|--------|------------------------------------------|----------------------|
| token     | string | API_KEY                                  ||
| p         | int    | platform                                 | Android : 1, iOS : 2 |
| campid    | int    | campaign ID                              ||
| userid    | string | user identification ID (max length: 100) ||
| adid      | string | AOS = adid, iOS = idfa                   ||
| ip        | string | IP address                               | recommend            |

### RESPONSE

| variavble | type   | description                                                             |
|-----------|--------|-------------------------------------------------------------------------|
| result    | int    | result code                                                             |
| tid       | string | Unique distinct value sent when installation verification is successful |

---
# 4.Callback(Postback)

- Upon successful completion of the campaign, a request for reward accumulation is sent to the publisher server.
  (We do not directly process the accumulation)
- The callback URL to receive the reward accumulation request is delivered to the integration manager in charge of the email at the bottom of the document. 

### Available Macro

| macro | type     | description                           | remark       |
|--|--------|------------------------------|----------|
| cbparam | string | cbparam value inserted in join api         |          |
| tid | string | Unique distinct value |          |
| subpid | string | sub-publisher ID                    | optional |
| userid | string | user identification ID                    |          |
| campid | int    | campaign ID                      |          |
| price | int    | revenue                          |          |
| price_dollar | float    | revenue                          |          |


### sample)

https://postback.com?clk_id={cbparam}&tid={tid}&aff_id={subpid}&uid={userid}&cid={campid}&price={price}



---

# result code

| code | message                           |
|------|-----------------------------------|
| 200  | success                           |
| 400  | bad request                       |
| 401  | unknown publisher                 |
| 402  | incorrect user information        |
| 403  | incorrect campaign information    |
| 405  | incorrect join history            |
| 406  | incorrect publisher information   |
| 407  | target is not matched             |
| 500  | closed campaign                   |
| 501  | already participated              |
| 502  | unable to participate in campaign |
| 900  | system error                      |

## Authors

* **Chris CHOI** - *Integration technical support* - bw@adbc.co.kr





