# 키워드 연동 가이드

최신 트렌딩 쇼핑키워드를 추천하여 매체에 제공합니다.


## API 명세

### 엔드포인트

```
GET https://cp.adbc.io/keywordad-api
```

### 요청 파라미터

#### 필수 파라미터

| 파라미터 | 유형 | 설명 | 예시 |
|---------|------|------|------|
| `pub_id` | string | 퍼블리셔 ID | `my_publisher` |
| `keyword_cnt` | integer | 요청 키워드 개수 (1~10) | `5` |
| `prod_info` | string | 상품정보 포함 여부 (`y`/`n`) | `y` |
| `tsource` | string | 앱 번들 또는 도메인 | `www.example.com` |

#### prod_info=y일 때 추가 필수

| 파라미터 | 유형 | 설명 | 예시 |
|---------|------|------|------|
| `prod_cnt` | integer | 키워드별 상품 개수 (1~10) | `3` |
| `width` | integer | 상품 이미지 가로 (50~2000px) | `300` |
| `height` | integer | 상품 이미지 세로 (50~2000px) | `300` |

#### 선택 파라미터

| 파라미터 | 유형 | 설명 | 예시 |
|---------|------|------|------|
| `gender` | string | 타겟 성별 (`m`: 남성, `f`: 여성) | `f` |
| `age_range` | integer | 타겟 연령대 (10/20/30/40/50/60) | `30` |
| `sub_media` | string | 서브매체명 | `main_banner` |
| `gaid` | string | Android 광고 ID | `4d56b456-...` |
| `idfa` | string | iOS 광고 ID | `F3DBC187-...` |
| `user_key` | string | 매체 고유 유저 구분값 | `user123` |
| `ssp_pub_bundle` | string | 노출매체 번들 ID/도메인 | `www.example.com` |
| `ssp_sub1` | string | 매체 커스텀 파라미터 1 | - |
| `ssp_sub2` | string | 매체 커스텀 파라미터 2 | - |
| `ssp_sub3` | string | 매체 커스텀 파라미터 3 | - |

### 요청 예시

#### prod_info=y (상품정보 포함)

```bash
curl -X GET "https://cp.adbc.io/keywordad-api?\
pub_id=my_publisher&\
keyword_cnt=3&\
prod_info=y&\
prod_cnt=5&\
width=300&\
height=300&\
tsource=www.example.com&\
gender=f&\
age_range=30"
```

#### prod_info=n (키워드만)

```bash
curl -X GET "https://cp.adbc.io/keywordad-api?\
pub_id=my_publisher&\
keyword_cnt=5&\
prod_info=n&\
tsource=www.example.com"
```

---

## 연동 시나리오

### 시나리오 1: prod_info=y (키워드 + 상품정보)

키워드와 함께 각 키워드에 해당하는 상품 정보를 받아 UI를 구성합니다.

**응답 예시:**

```json
{
  "camp_dsp_id": "292",
  "dsp_id": "28",
  "req_id": "KWD_20240529160315233668015173",
  "tracking_id": "6949f2a068af51c8b5b9442d",
  "channel_cd": "105528SA",
  "adv_banner_logo_url": "https://static.coupangcdn.com/image/coupang/common/logo_coupang_w350.png",
  "adv_banner_logo_click_url": "https://ad.n-bridge.io/...",
  "keyword_list": [
    {
      "keyword_cd": "1184235323a5f650050636f0f25f033d",
      "keyword": "자석젤네일",
      "prod_id": "158853778",
      "prod_name": "아모스프로페셔널 컬링에센스 2X 헤어에센스",
      "prod_img": "https://ads-partners.coupang.com/image1/...",
      "click_url": "https://ad.n-bridge.io/ads/...&ssp_click_id=6949f2a068af51c8b5b9442d_0",
      "imp_url": "https://ad.n-bridge.io/imp/...",
      "custom_tracking_url": "https://adbc.io/1418392354/my_publisher?tid=6949f2a068af51c8b5b9442d_0&keyword=자석젤네일&landing=0"
    },
    {
      "keyword_cd": "2b36799a0cdee1a3b02055ffb74bed43",
      "keyword": "주토피아가챠",
      "prod_id": "1333742606",
      "prod_name": "디즈니 주토피아 피규어...",
      "prod_img": "https://ads-partners.coupang.com/image1/...",
      "click_url": "https://ad.n-bridge.io/ads/...&ssp_click_id=6949f2a068af51c8b5b9442d_1",
      "imp_url": "https://ad.n-bridge.io/imp/...",
      "custom_tracking_url": "https://adbc.io/1418392354/my_publisher?tid=6949f2a068af51c8b5b9442d_1&keyword=주토피아가챠&landing=0"
    }
  ]
}
```

### 응답 필드 설명

| 필드 | 유형 | 설명 |
|------|------|------|
| `req_id` | string | CPS 광고요청 ID |
| `tracking_id` | string | 트래킹 ID (노출/클릭 매칭용) |
| `channel_cd` | string | CPS 채널 코드 |
| `adv_banner_logo_url` | string | 광고주 로고 이미지 URL |
| `adv_banner_logo_click_url` | string | 로고 클릭 시 랜딩 URL |
| `keyword_list` | array | 키워드 목록 |
| `keyword_list[].keyword_cd` | string | CPS 키워드 ID |
| `keyword_list[].keyword` | string | 쇼핑 키워드 |
| `keyword_list[].prod_id` | string | 대표 상품 ID |
| `keyword_list[].prod_name` | string | 대표 상품명 |
| `keyword_list[].prod_img` | string | 대표 상품 이미지 URL |
| `keyword_list[].click_url` | string | 클릭 URL (키워드 검색결과/상품페이지) |
| `keyword_list[].imp_url` | string | **노출 트래킹 URL (호출 필수)** |
| `keyword_list[].custom_tracking_url` | string | 커스텀 트래킹 URL |

---

### 필수 구현사항

#### 1. 노출 트래킹 (필수)

키워드가 화면에 노출될 때 반드시 `imp_url`을 호출해야 합니다.

```javascript
function trackImpression(impUrl) {
  new Image().src = impUrl;
}
```

#### 2. 클릭 처리 (필수)

키워드 클릭 시 `click_url`로 이동해야 합니다.

```javascript
function handleKeywordClick(keywordItem) {
  if (keywordItem.custom_tracking_url) {
    new Image().src = keywordItem.custom_tracking_url;
  }

  // 랜딩 페이지로 이동
  window.open(keywordItem.click_url, '_blank');
}
```


### 주요 에러 코드

| HTTP 상태 | 에러 메시지 | 원인 | 해결 방법 |
|----------|-----------|------|----------|
| 400 | 필수 파라미터 누락: pub_id, keyword_cnt | 필수 파라미터 미입력 | 필수 파라미터 확인 |
| 400 | prod_info=y일 경우 필수 파라미터 누락: prod_cnt, width, height | prod_info=y인데 상품 파라미터 없음 | prod_cnt, width, height 추가 |
| 400 | prod_info는 'y' 또는 'n' 값만 허용됩니다. | 잘못된 prod_info 값 | y 또는 n으로 수정 |
| 500 | CPS API 요청 타임아웃 | API 응답 지연 | 재시도 또는 운영팀 문의 |
| 500 | CPS API 요청 실패 | API 서버 오류 | 운영팀 문의 |


---

### Q1. tsource 파라미터 형식은?

프로토콜(`https://`, `http://`)과 경로(`/path`)를 제외한 도메인 또는 앱 번들명을 입력합니다.

- **올바른 예**: `www.example.com`, `com.example.app`
- **잘못된 예**: `https://www.example.com/path`

### Q2. keyword_cnt와 prod_cnt의 최대값은?

- `keyword_cnt`: 최대 **10개**
- `prod_cnt`: 최대 **10개**


---

