# 검색어광고 S2S(Server-to-Server) 연동 가이드

매체사 서버에서 검색어광고 API를 호출하고, 응답 데이터로 직접 UI를 구성하는 방식입니다.

---

## API 엔드포인트

```
GET https://cp.adbc.io/searchad-api
```

---

## 요청 파라미터

### 필수 파라미터

| 파라미터             | 타입     | 설명                              | 예시                                  |
|------------------|--------|---------------------------------|-------------------------------------|
| `keyword`        | string | 검색어 (URL 인코딩 필요)                | `%ED%85%80%EB%B8%94%EB%9F%AC` (텀블러) |
| `pub_id`         | string | 퍼블리셔 ID (**ADBC에서 발급**)         | `45539157`                          |
| `tsource`        | string | 매체 도메인 또는 앱 번들명 (**ADBC에서 발급**) | `com.example.app`                   |

### 선택 파라미터

| 파라미터 | 타입 | 기본값 | 설명 |
|---------|------|--------|------|
| `prod_cnt` | integer | 5 | 요청 상품 개수 (1~20) |
| `width` | integer | 300 | 상품 이미지 가로 (50~2000px) |
| `height` | integer | 300 | 상품 이미지 세로 (50~2000px) |
| `creative_yn` | string | `y` | 상품 이미지 노출 여부 (`y`/`n`) |
| `sub_media` | string | - | 서브매체명 |
| `gaid` | string | - | Android 광고 ID (GAID) |
| `idfa` | string | - | iOS 광고 ID (IDFA) |

---

## 요청 예시

```bash
curl "https://cp.adbc.io/searchad-api?keyword=%ED%85%80%EB%B8%94%EB%9F%AC&pub_id=45539157&tsource=com.example.app&prod_cnt=5"
```

---

## 응답 데이터

### 성공 응답 (HTTP 200)

```json
{
  "req_id": "ADS_20231016114604744210048517",
  "creative_id": "10",
  "width": "300",
  "height": "300",
  "prod_cnt": "5",
  "tracking_id": "6579a1b2c3d4e5f6a7b8c9d0",
  "search_click_url": "https://ad.n-bridge.io/...",
  "prod_list": [
    {
      "productId": "1947443462",
      "productImage": "https://ads-partners.coupang.com/image1/...",
      "productName": "에이비티 온도표시 보온보냉 텀블러, 블랙, 500ml",
      "productPrice": "21700.000000",
      "cateName": "주방용품",
      "impUrl": "https://ad.n-bridge.io/imp/...",
      "clickUrl": "https://ad.n-bridge.io/...",
      "isRocket": "True",
      "isFreeShipping": "True",
      "custom_tracking_url": "https://adbc.io/1418392354/my_publisher?tid=6579a1b2c3d4e5f6a7b8c9d0&prod_id=1947443462"
    },
    {
      "productId": "7293847562",
      "productImage": "https://ads-partners.coupang.com/image2/...",
      "productName": "스탠리 클래식 텀블러, 그린, 473ml",
      "productPrice": "35000.000000",
      "cateName": "주방용품",
      "impUrl": "https://ad.n-bridge.io/imp/...",
      "clickUrl": "https://ad.n-bridge.io/...",
      "isRocket": "True",
      "isFreeShipping": "False",
      "custom_tracking_url": "https://adbc.io/1418392354/my_publisher?tid=6579a1b2c3d4e5f6a7b8c9d0&prod_id=7293847562"
    }
  ],
  "camp_dsp_id": "283",
  "dsp_id": "28"
}
```

### 에러 응답 (HTTP 400)

필수 파라미터가 누락된 경우 에러를 반환합니다.

```json
{
  "error": true,
  "message": "필수 파라미터 누락: tsource"
}
```

> **참고**: `keyword`, `pub_id`, `tsource` 중 하나라도 누락되면 에러가 발생합니다. 누락된 파라미터를 확인하고 다시 요청해주세요.

---

## 응답 필드 설명

### 최상위 필드

| 필드 | 타입 | 설명 |
|------|------|------|
| `req_id` | string | NAP CPS 광고요청 ID |
| `tracking_id` | string | 트래킹 ID (24자리 16진수, MongoDB ObjectID 호환) |
| `prod_cnt` | string | 반환된 상품 개수 |
| `search_click_url` | string | 검색 결과 페이지 URL |
| `prod_list` | array | 상품 목록 |

### 상품 필드 (prod_list[])

| 필드 | 타입 | 설명 |
|------|------|------|
| `productId` | string | 상품 ID |
| `productImage` | string | 상품 이미지 URL |
| `productName` | string | 상품명 |
| `productPrice` | string | 상품 가격 (소수점 포함) |
| `cateName` | string | 카테고리명 |
| `impUrl` | string | **노출 트래킹 URL (필수 호출)** |
| `clickUrl` | string | 상품 클릭 URL (상품 페이지 랜딩) |
| `isRocket` | string | 로켓배송 여부 (`True`/`False`) |
| `isFreeShipping` | string | 무료배송 여부 (`True`/`False`) |
| `custom_tracking_url` | string | **클릭 트래킹 URL (필수 호출)** |

---

## 필수 연동 사항

### 1. 노출 트래킹 (impUrl)

상품이 화면에 노출될 때 반드시 `impUrl`을 호출해야 합니다.

```javascript
// 상품 노출 시 호출
fetch(product.impUrl, { method: 'GET', mode: 'no-cors' });
```

### 2. 클릭 트래킹 (custom_tracking_url) ⚠️ 필수

**사용자가 상품을 클릭할 때 반드시 `custom_tracking_url`을 호출해야 합니다.**

이 URL을 호출하지 않으면 정산 데이터에 집계되지 않습니다.

```javascript
// 상품 클릭 시 구현 예시
function onProductClick(product) {
  // 1. 클릭 트래킹 URL 호출 (필수)
  fetch(product.custom_tracking_url, { method: 'GET', mode: 'no-cors' });

  // 2. 상품 페이지로 이동
  window.open(product.clickUrl, '_blank');
}
```

> **중요**: `clickUrl`로 이동하기 전에 `custom_tracking_url`을 먼저 호출해주세요.

---

## 주의사항

### tsource 파라미터

`tsource`는 반드시 ADBC에서 발급받은 값을 사용해야 합니다.
잘못된 값으로 요청 시 광고가 정상적으로 노출되지 않을 수 있습니다.