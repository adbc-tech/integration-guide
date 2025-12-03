# 쿠팡 검색어 API 가이드

## 개요

쿠팡 파트너스 상품 검색 API를 통해 키워드 기반으로 쿠팡 상품을 검색할 수 있습니다.

- **호출 제한**: 1분당 최대 50회
- **최대 상품 수**: 10개

---

## API 명세

### 엔드포인트

```
GET https://adbc.io/cpsearch
```

### 요청 파라미터

| 파라미터 | 타입 | 필수 | 기본값 | 설명 |
|----------|------|:----:|--------|------|
| `token` | string | ✅ | - | 매체 인증 토큰 |
| `keyword` | string | ✅ | - | 검색 키워드 |
| `limit` | integer | | 10 | 최대 상품 수 (1~10) |
| `imageSize` | string | | 512x512 | 이미지 사이즈 (예: 230x230, 300x300, 512x512) |

* token은 adbc에서 발급하여 전달드립니다
---

## 요청 예시

### 기본 요청

```
GET https://adbc.io/cpsearch?token=YOUR_TOKEN&keyword=아이폰
```

### 전체 파라미터 요청

```
GET https://adbc.io/cpsearch?token=YOUR_TOKEN&keyword=아이폰&limit=5&imageSize=300x300
```

### cURL 예시

```bash
curl -X GET "https://adbc.io/cpsearch?token=YOUR_TOKEN&keyword=아이폰&limit=5"
```

---

## 응답

### 성공 응답 (HTTP 200)

```json
{
  "rCode": "0",
  "rMessage": "",
  "data": {
    "landingUrl": "https://link.coupang.com/re/AFFSRP?lptag=AF1234567&pageKey=fashion&traceid=V0-153-5243b8cd44933da5",
    "productData": [
      {
        "keyword": "아이폰",
        "rank": 1,
        "isRocket": true,
        "isFreeShipping": false,
        "productId": 27664441,
        "productImage": "https://ads-partners.coupang.com/image1/...",
        "productName": "Apple 아이폰 15 Pro 256GB",
        "productPrice": 1550000,
        "productUrl": "https://link.coupang.com/re/AFFSDP?lptag=AF1234567&pageKey=319834306&itemId=1023216541&vendorItemId=70064597513&traceid=V0-163-5fddb21eaffbb2ef"
      },
      {
        "keyword": "아이폰",
        "rank": 2,
        "isRocket": true,
        "isFreeShipping": true,
        "productId": 12345678,
        "productImage": "https://ads-partners.coupang.com/image1/...",
        "productName": "Apple 아이폰 15 128GB 자급제",
        "productPrice": 1250000,
        "productUrl": "https://link.coupang.com/re/AFFSDP?..."
      }
    ]
  }
}
```

### 응답 필드 설명

#### 최상위 필드

| 필드 | 타입 | 설명 |
|------|------|------|
| `rCode` | string | 응답 코드 ("0": 성공) |
| `rMessage` | string | 응답 메시지 |
| `data` | object | 검색 결과 데이터 |

#### data 필드

| 필드 | 타입 | 설명 |
|------|------|------|
| `landingUrl` | string | 검색 결과 페이지 URL |
| `productData` | array | 상품 목록 |

#### productData 배열 항목

| 필드 | 타입 | 설명 |
|------|------|------|
| `keyword` | string | 검색 키워드 |
| `rank` | integer | 검색 순위 |
| `isRocket` | boolean | 로켓배송 여부 |
| `isFreeShipping` | boolean | 무료배송 여부 |
| `productId` | long | 상품 ID |
| `productImage` | string | 상품 이미지 URL |
| `productName` | string | 상품명 |
| `productPrice` | integer | 상품 가격 (원) |
| `productUrl` | string | 상품 상세 페이지 URL (제휴 링크) |

---

## 에러 응답

### 인증 오류 (HTTP 401)

#### token 누락

```json
{
  "code": "401",
  "message": "token is required"
}
```

#### 유효하지 않은 token

```json
{
  "code": "401",
  "message": "Invalid token"
}
```

### 요청 오류 (HTTP 400)

#### keyword 누락

```json
{
  "code": "400",
  "message": "keyword is required"
}
```

#### 쿠팡 API 오류

```json
{
  "code": "에러코드",
  "message": "에러메시지"
}
```

### 검색 결과 없음 (HTTP 404)

```json
{
  "code": "404",
  "message": "No results found"
}
```

### 서버 오류 (HTTP 500)

```json
{
  "code": "500",
  "message": "Internal server error"
}
```

---

## 사용 시 주의사항

1. **토큰 관리**: 발급받은 token은 외부에 노출되지 않도록 안전하게 관리해주세요.
2. **호출 제한**: 1분당 50회 이상 호출 시 API 사용이 제한될 수 있습니다.
3. **상품 수 제한**: limit 값은 최대 10까지만 지원됩니다.
4. **제휴 링크**: 응답의 `productUrl`은 쿠팡 파트너스 제휴 링크입니다. 사용자가 해당 링크를 통해 구매 시 수익이 발생합니다.

---

