# Reward Campaign API guide for publisher

### Getting Started

- 본 가이드는 ADBC와 매체사간의 S2S API 연동에 대한 내용입니다 
---
# 1.캠페인 리스트 조회 API

- 5분 주기로 업데이트 받는 것을 추천드립니다
- method : GET
- url : http://api.adbc.io/api/v3/reward/campaigns
- parameter : token (필수), p (필수)

|항목|설명|비고|
|------|------|------|
|token|매체사 식별키| |
|p|플랫폼|Android : 1, iOS : 2|
- 요청 예시 : http://api.adbc.io/api/v3/reward/campaigns?token={token}
- token은 발급 후 전달드립니다



| 항목 | 형태 | 설명 | 비고                                      |
| ------ | ------ | ------ |-----------------------------------------|
| result | int | 응답 결과 값 | 200인 경우에만 정상, '결과 코드' 참고                |
| cnt | int | 캠페인 수 |                                         |
| camp | array | 캠페인 리스트 |                                         |
| camp.campid | int  | 캠페인 식별값 |                                         |
| camp.name | string | 캠페인명 |                                         |
| camp.bm | int | 캠페인 유형 | 1: CPA, 2:CPE, 3:CPI, 4:CPS, 0: 기타 |
| camp.package | string | 패키지네임 또는 url scheme | android: package name, iOS: custom url  |
| camp.price | int | 캠페인 집행 단가 (=매체비) |                                         |
| camp.rewarddesc | string | 캠페인 적립 조건 안내 | ex) 앱 설치 후 회원가입 완료                      |
| camp.joindesc | string | 캠페인 참여 방법 안내 |                                         |
| camp.totalquantity | int | 해당 캠페인의 참여 가능한 총 수량 | 0: 무제한                                  |
| camp.quantity | int | 해당 캠페인의 일별 참여 가능 수량(=매체사에 할당된 데일리 캡) | 0: 무제한                                  |
| camp.enddate | string | 캠페인 종료일 | 1) 값이 있는 경우: YYYYMMDD 2) 값이 없는 경우: 별도 종료일 없음 |
| camp.targetcarrier | string | 통신사 타겟팅 | 공백: 타겟 없음, 1: KT, 2: LGU+, 3: SKT       |
| camp.targetgender | int | 성별 타겟팅 | 0: 타겟 없음, 1: 남자, 2: 여자                  |
| camp.targetagemin | int | 최소 연령 타겟팅 | 0: 타겟 없음                                |
| camp.targetagemax | int | 최대 연령 타겟팅 | 0: 타겟 없음                                |
| camp.targetpkg | string | 앱 패키지네임 타겟팅. AOS 캠페인일 경우에만 내려옵니다 |                                         |
| camp.detargetpkg | string | 앱 패키지네임 디타겟팅. AOS 캠페인일 경우에만 내려옵니다 |                                         |
| camp.iconurl | string | 캠페인의 아이콘 이미지 url |                                         |
| camp.ctv | array | 아이콘 이미지를 제외한 가로형, 세로형 소재 정보를 포함하고 있습니다 | 세부 항목 참고                                |
| camp.cpsprice(*) | int | 상품의 할인 전 가격 | bm=4인, cps 캠페인 전용                       |
| camp.cpsdcprice(*) | int | 상품의 실제 구매 가격 | bm=4인, cps 캠페인 전용                       |

(*) : 해당 사항이 없을 경우, 응답 항목이 전달되지 않습니다.

camp -> ctv 세부 항목

|항목|형태|설명|
|------|------|------|
|type|int|1:가로형 소재(native), 1200*600, 2:1 ratio|
|url|string|소재 url|




---
# 2.캠페인 참여 요청 API


- 광고에 참여하기 위해서는 캠페인 참여 요청 API를 호출해 참여 가능 여부를 확인해야 합니다.
- 참여 유저의 데이터 정보(성별, 연령 등)를 많이 전달 할 수록 더 많은 캠페인에 참여할 수 있습니다.

### 요청 
- method : GET
- url : https://adbc.io/reward/v3/join
- parameter : 

|항목|형태|내용|비고|
|------|------|------|------|
|token|string|매체사 식별키||
|p|int|플랫폼|Android : 1, iOS : 2|
|cbparam|string|매체사에서 정의한 파라미터||
|aff_id|string|매체사의 하위 매체 아이디 (최대 길이: 100)||
|userid|string|참여 유저 식별값 (최대 길이: 100)||
|campid|int|캠페인 식별값||
|osver|string|참여 단말기 OS 버전 정보||
|ip|string|참여 단말기 IP 정보||
|adid|string|AOS = adid, iOS = idfa||
|accid|string|AOS = android id, iOS = idfv|권장|
|devid|string|AOS = imei, iOS = uuid|권장|
|wvid|string|Widevine DRM용 ID값 - AOS인 경우|권장|
|wvslv|string|Widevine security level(L1/l3) - AOS인 경우|권장|
|devmodel|string|참여 단말기 모델 정보|권장|
|devbrand|string|참여 단말기 제조자 정보|권장|
|mnetwork|int|참여 단말기 네트워크 정보|1: 3G, 2: 4G, 3: 5G, 4: WIFI|
|carrier|int|참여 단말기 통신사 정보(통신사 타게팅 캠페인일 경우 필수)|권장, 1: KT, 2: LGU+, 3: SKT|
|ga|string|참여 단말기의 구글 계정을 sha1 암호화|권장|
|age|int|유저 연령 정보(연령 타겟팅 캠페인일 경우 필수)||
|gender|int|참여 유저 성별 정보(성별 타겟팅 캠페인일 경우 필수)|0: 모름, 1: 남자, 2: 여자|

### 응답
|항목|형태|내용|
|------|------|------|
|result|int|처리 결과 코드|
|lurl|string|캠페인 랜딩 URL|

---
# 3.CPI 설치 확인 API
- 본 항목은 CPI 캠페인만 해당하는 API입니다
- CPI 캠페인은 매체사가 직접 사용자가 해당 앱의 설치 여부를 확인한 후, 이 API를 통해 설치 완료 요청을 해야 합니다
- 설치 완료 확인이 성공시, ADBC로부터 실적을 수신받게 됩니다

### 요청

- method : GET
- url : https://adbc.io/reward/v3/inst
- parameter :

|항목|형태|내용|비고|
|------|------|------|------|
|token|string|매체사 식별키||
|p|int|플랫폼|Android : 1, iOS : 2|
|campid|int|캠페인 식별값||
|userid|string|참여 유저 식별값 (최대 길이: 100)||
|adid|string|AOS = adid, iOS = idfa||
|wvid|string|Widevine DRM용 ID값 - AOS인 경우|권장|
|wvslv|string|Widevine security level(L1/l3) - AOS인 경우|권장|
|ip|string|참여 단말기 IP 정보(공인 IP만 인정되고 서버IP 또는 사설IP는 사용 불가)|권장|

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

### 매크로

|항목|형태|설명|비고|
|------|------|------|------|
|cbparam| string | 캠페인 참여시 보냈던 cbparam값 |  |
|tid| string | 실적에 대한 유니크값 (transaction ID) |  |
|subpid| string | 하위 매체 구분값 | optional |
|userid| string | 참여 유저 식별값 |  |
|campid| int | 캠페인 식별값 |  |
|price| int | 매체비 |  |


### 예시

https://postback.com?clk_id={cbparam}&tid={tid}&aff_id={subpid}&uid={userid}&cid={campid}&price={price}



---
## 공통
- result로 반환되는 코드 목록입니다

### result code

|항목|내용|
|------|------|
|200|성공|
|400|잘못된 요청|
|401|등록되지 않은 매체|
|402|유저 정보 오류|
|403|캠페인 정보 오류|
|405|참여 이력 오류|
|406|매체 오류|
|407|타겟팅 매칭 실패|
|500|종료된 캠페인|
|501|중복 참여|
|502|참여할 수 없는 캠페인|
|900|시스템 오류|

## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





