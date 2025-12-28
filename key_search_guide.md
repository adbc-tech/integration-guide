# Key-Search API 연동 가이드

CPS Key-Search API 연동 가이드입니다. 트렌딩 키워드와 연관 상품을 한 번에 조회할 수 있는 통합 API입니다.

## 목차

1. [개요](#개요)
2. [API 명세](#api-명세)
3. [응답 데이터](#응답-데이터)
4. [연동 구현](#연동-구현)
5. [트래킹 구현](#트래킹-구현)
6. [UI 구현 예시](#ui-구현-예시)
7. [FAQ](#faq)

---

## 개요

### Key-Search API란?

Key-Search API는 실시간 트렌딩 키워드와 각 키워드별 추천 상품을 한 번의 API 호출로 제공하는 통합 API입니다.

**특징**:
- 매일 자동 갱신되는 **5개 트렌딩 키워드**
- 키워드당 **10개 추천 상품** (총 50개)
- 단일 API 호출로 모든 데이터 제공
- 트래킹 ID 기반 노출/클릭 추적 지원

### 데이터 갱신 주기

- **갱신 시간**: 매일 00:00 KST (한국시간 자정)
- **데이터 유효 기간**: 25시간 (다음 갱신까지 유지)

### 활용 사례

| 사용처 | 설명 |
|--------|------|
| 메인 페이지 | 인기 검색어 + 추천 상품 섹션 |
| 검색 결과 없음 페이지 | 대체 추천 상품 노출 |
| 사이드바/위젯 | 트렌딩 키워드 배너 |
| 뉴스레터/앱 푸시 | 일일 인기 상품 추천 |

---

## API 명세

### 엔드포인트

```
GET https://cp.adbc.io/key-search-api
```

### 요청 파라미터

| 파라미터 | 유형 | 설명                     | 필수 | 예시                |
|---------|------|------------------------|------|-------------------|
| `pub_id` | string | 퍼블리셔 ID (adbc에서 발급)    | Y | `12345678`        |
| `tsource` | string | 앱 번들 또는 도메인(adbc에서 발급) | Y | `example.com` |
| `sub_media` | string | 서브매체명                  | N | `main_page`       |

### 요청 예시

```bash
curl -X GET "https://cp.adbc.io/key-search-api?pub_id=12345678&tsource=example.com"
```

### 성공 응답 (200)

```json
{
  "success": true,
  "updated_at": "2024-12-28T00:00:00+09:00",
  "tracking_id": "694a1054ee6a5e0b0ef5ef5b",
  "keyword_count": 5,
  "product_count": 50,
  "data": [
    {
      "keyword": "케이크토퍼",
      "products": [
        {
          "product_id": "8485769335",
          "product_name": "크리스마스 케이크 장식 데코 토퍼",
          "product_image": "https://ads-partners.coupang.com/image1/...",
          "product_price": 18000,
          "category": "문구/오피스",
          "is_rocket": false,
          "is_free_shipping": true,
          "click_url": "https://ad.n-bridge.io/...",
          "imp_url": "https://ad.n-bridge.io/imp/...",
          "custom_tracking_url": "https://adbc.io/1418392354/my_publisher?tid=694a1054ee6a5e0b0ef5ef5b_0_0&keyword=케이크토퍼&landing=0"
        }
        // ... 9개 더 (총 10개)
      ]
    }
    // ... 4개 더 (총 5개 키워드)
  ]
}
```

### 에러 응답

**데이터 없음 (404)**:
```json
{
  "success": false,
  "error": "데이터가 아직 생성되지 않았습니다. 잠시 후 다시 시도해주세요."
}
```

**파라미터 누락 (400)**:
```json
{
  "success": false,
  "error": "필수 파라미터 누락: pub_id"
}
```

**서버 오류 (500)**:
```json
{
  "success": false,
  "error": "서버 오류: [상세 메시지]"
}
```

---

## 응답 데이터

### 최상위 필드

| 필드 | 유형 | 설명 |
|------|------|------|
| `success` | boolean | 요청 성공 여부 |
| `updated_at` | string | 데이터 갱신 시간 (ISO 8601) |
| `tracking_id` | string | 트래킹 ID (24자리 hex) |
| `keyword_count` | integer | 키워드 수 (기본 5) |
| `product_count` | integer | 총 상품 수 (기본 50) |
| `data` | array | 키워드별 상품 목록 |

### 키워드 객체

| 필드 | 유형 | 설명 |
|------|------|------|
| `keyword` | string | 트렌딩 키워드 |
| `products` | array | 해당 키워드의 상품 목록 (10개) |

### 상품 객체

| 필드 | 유형 | 설명 |
|------|------|------|
| `product_id` | string | 상품 ID |
| `product_name` | string | 상품명 |
| `product_image` | string | 상품 이미지 URL |
| `product_price` | number | 상품 가격 (원) |
| `category` | string | 카테고리명 |
| `is_rocket` | boolean | 로켓배송 여부 |
| `is_free_shipping` | boolean | 무료배송 여부 |
| `click_url` | string | **클릭 URL (필수 사용)** |
| `imp_url` | string | **노출 트래킹 URL (필수 호출)** |
| `custom_tracking_url` | string | **클릭 트래킹 URL (필수 호출)** |

---

## 연동 구현

### JavaScript (프론트엔드)

```javascript
async function loadKeySearchData() {
  const params = new URLSearchParams({
    pub_id: 'my_publisher',
    tsource: 'www.example.com'
  });

  try {
    const response = await fetch(`https://cp.adbc.io/key-search-api?${params}`);
    const data = await response.json();

    if (data.success) {
      console.log(`${data.keyword_count}개 키워드, ${data.product_count}개 상품 로드 완료`);
      console.log(`갱신 시간: ${data.updated_at}`);
      return data;
    } else {
      console.error('API 오류:', data.error);
      return null;
    }
  } catch (error) {
    console.error('네트워크 오류:', error);
    return null;
  }
}
```

### Python (서버 사이드)

```python
import requests

def get_key_search_data(pub_id: str, tsource: str) -> dict:
    """Key-Search API 호출"""
    url = "https://cp.adbc.io/key-search-api"
    params = {
        "pub_id": pub_id,
        "tsource": tsource
    }

    response = requests.get(url, params=params, timeout=10)
    data = response.json()

    if data.get("success"):
        print(f"키워드: {data['keyword_count']}개, 상품: {data['product_count']}개")
        return data
    else:
        raise Exception(data.get("error", "Unknown error"))

# 사용 예시
data = get_key_search_data("my_publisher", "www.example.com")
for keyword_data in data["data"]:
    print(f"[{keyword_data['keyword']}] {len(keyword_data['products'])}개 상품")
```

### Java (Android)

```java
public class KeySearchApi {
    private static final String API_URL = "https://cp.adbc.io/key-search-api";

    public void fetchData(String pubId, String tsource, Callback callback) {
        String url = API_URL + "?pub_id=" + pubId + "&tsource=" + tsource;

        Request request = new Request.Builder()
            .url(url)
            .get()
            .build();

        client.newCall(request).enqueue(new okhttp3.Callback() {
            @Override
            public void onResponse(Call call, Response response) throws IOException {
                String json = response.body().string();
                KeySearchResponse data = gson.fromJson(json, KeySearchResponse.class);
                if (data.success) {
                    callback.onSuccess(data);
                } else {
                    callback.onError(data.error);
                }
            }

            @Override
            public void onFailure(Call call, IOException e) {
                callback.onError(e.getMessage());
            }
        });
    }
}
```

### Swift (iOS)

```swift
struct KeySearchAPI {
    static let baseURL = "https://cp.adbc.io/key-search-api"

    static func fetchData(pubId: String, tsource: String) async throws -> KeySearchResponse {
        var components = URLComponents(string: baseURL)!
        components.queryItems = [
            URLQueryItem(name: "pub_id", value: pubId),
            URLQueryItem(name: "tsource", value: tsource)
        ]

        let (data, _) = try await URLSession.shared.data(from: components.url!)
        let response = try JSONDecoder().decode(KeySearchResponse.self, from: data)

        guard response.success else {
            throw NSError(domain: "KeySearchAPI", code: -1, userInfo: [NSLocalizedDescriptionKey: response.error ?? "Unknown error"])
        }

        return response
    }
}
```

---

### 노출 트래킹 (필수)

상품이 화면에 노출될 때 반드시 `imp_url`을 호출해야 합니다.

```javascript
// 방법 1: Image 비콘
function trackImpression(impUrl) {
  new Image().src = impUrl;
}

// 방법 2: Intersection Observer (권장)
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const impUrl = entry.target.dataset.impUrl;
      if (impUrl && !entry.target.dataset.tracked) {
        new Image().src = impUrl;
        entry.target.dataset.tracked = 'true';
      }
    }
  });
}, { threshold: 0.5 }); // 50% 이상 노출 시

document.querySelectorAll('.product-item').forEach(el => observer.observe(el));
```

### 클릭 처리 (필수)

상품 클릭 시 `custom_tracking_url`을 호출하고 `click_url`로 이동해야 합니다.

```javascript
function handleProductClick(product) {
  // 1. 클릭 트래킹 (필수)
  new Image().src = product.custom_tracking_url;

  // 2. 상품 페이지로 이동 (필수)
  window.open(product.click_url, '_blank');
}
```

---


## FAQ

### Q1. pub_id는 어떻게 발급받나요?

adbc 운영팀에 문의하시면 매체별로 고유한 `pub_id`를 발급해 드립니다.

### Q2. tsource 파라미터 형식은?

프로토콜(`https://`, `http://`)과 경로(`/path`)를 제외한 도메인 또는 앱 번들명을 입력합니다.

- **올바른 예**: `www.example.com`, `com.example.app`
- **잘못된 예**: `https://www.example.com/path`

### Q3. 데이터가 없을 때 어떻게 처리하나요?

API 응답의 `success`가 `false`이거나 `data`가 비어있으면 대체 콘텐츠를 표시합니다.

```javascript
if (!data.success || data.data.length === 0) {
  // 패스백 URL 호출 또는 대체 콘텐츠 표시
  showAlternativeContent();
}
```

### Q3. 데이터는 언제 갱신되나요?

- **갱신 시간**: 매일 00:00 KST (한국시간 자정)
- **응답의 `updated_at` 필드**로 정확한 갱신 시간 확인 가능

---

## 문의

연동 관련 문의사항은 adbc 운영팀으로 연락 바랍니다.
