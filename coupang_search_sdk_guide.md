# 검색 광고 SDK 가이드

---

## 빠른 시작

### 1. 스크립트 삽입

```html
<script src="https://adbc.io/static/js/cpsearch.js"></script>
```

### 2. 광고 영역 생성

```html
<div id="coupang-ad"></div>
```

### 3. 초기화

```html
<script>
AdbcCoupangAd.init({
    token: 'YOUR_TOKEN',
    keyword: '아이폰',
    elementId: 'coupang-ad'
});
</script>
```

---

## 전체 예제

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>쿠팡 광고 예제</title>
</head>
<body>
    <h1>추천 상품</h1>

    <!-- 광고 영역 -->
    <div id="coupang-ad"></div>

    <!-- SDK 스크립트 -->
    <script src="https://adbc.io/static/js/cpsearch.js"></script>
    <script>
        AdbcCoupangAd.init({
            token: 'YOUR_TOKEN',      // 발급받은 토큰
            keyword: '무선이어폰',      // 검색 키워드
            elementId: 'coupang-ad',  // 광고 영역 ID
            limit: 6,                 // 상품 개수
            layout: 'grid',           // 레이아웃
            cols: 3                   // 컬럼 수
        });
    </script>
</body>
</html>
```

---

## 옵션

| 옵션 | 타입 | 필수 | 기본값 | 설명 |
|------|------|:----:|--------|------|
| `token` | string | ✅ | - | 매체 인증 토큰 |
| `keyword` | string | ✅ | - | 검색 키워드 |
| `elementId` | string | ✅ | - | 광고를 표시할 요소 ID |
| `limit` | number | | 6 | 상품 개수 (최대 10) |
| `imageSize` | string | | '512x512' | 이미지 크기 |
| `layout` | string | | 'grid' | 레이아웃 타입 ('grid' 또는 'scroll') |
| `cols` | number | | 3 | 그리드 컬럼 수 (layout이 'grid'일 때) |
| `itemWidth` | number | | 150 | 아이템 너비 (layout이 'scroll'일 때) |

---

## 레이아웃

### Grid 레이아웃 (기본)

상품을 그리드 형태로 배치합니다.

```javascript
AdbcCoupangAd.init({
    token: 'YOUR_TOKEN',
    keyword: '노트북',
    elementId: 'coupang-ad',
    layout: 'grid',
    cols: 4,        // 4열로 배치
    limit: 8        // 8개 상품
});
```

**결과:**
```
┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
│ 상품1 │ │ 상품2 │ │ 상품3 │ │ 상품4 │
└──────┘ └──────┘ └──────┘ └──────┘
┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
│ 상품5 │ │ 상품6 │ │ 상품7 │ │ 상품8 │
└──────┘ └──────┘ └──────┘ └──────┘
```

### Scroll 레이아웃

상품을 가로 스크롤 형태로 배치합니다. 모바일에 적합합니다.

```javascript
AdbcCoupangAd.init({
    token: 'YOUR_TOKEN',
    keyword: '화장품',
    elementId: 'coupang-ad',
    layout: 'scroll',
    itemWidth: 160,  // 아이템 너비 160px
    limit: 10
});
```

**결과:**
```
┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ...
│ 상품1 │ │ 상품2 │ │ 상품3 │ │ 상품4 │ │ 상품5 │ ←→ 스크롤
└──────┘ └──────┘ └──────┘ └──────┘ └──────┘
```

---

## 사용 예시

### 모바일 최적화 (2열 그리드)

```javascript
AdbcCoupangAd.init({
    token: 'YOUR_TOKEN',
    keyword: '헤드폰',
    elementId: 'mobile-ad',
    layout: 'grid',
    cols: 2,
    limit: 4
});
```

### 사이드바 광고 (1열)

```javascript
AdbcCoupangAd.init({
    token: 'YOUR_TOKEN',
    keyword: '키보드',
    elementId: 'sidebar-ad',
    layout: 'grid',
    cols: 1,
    limit: 3
});
```

### 가로 배너형 (스크롤)

```javascript
AdbcCoupangAd.init({
    token: 'YOUR_TOKEN',
    keyword: '간식',
    elementId: 'banner-ad',
    layout: 'scroll',
    itemWidth: 140,
    limit: 10
});
```

---

## 스타일 커스터마이징

광고 영역의 크기는 부모 요소의 CSS로 조절할 수 있습니다.

```html
<style>
    #coupang-ad {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
    }
</style>

<div id="coupang-ad"></div>
```

---

## 주의사항

1. **토큰 관리**: token은 외부에 노출되지 않도록 주의하세요.
2. **호출 제한**: API는 1분당 50회까지 호출 가능합니다.
3. **광고 문구**: SDK가 자동으로 쿠팡 파트너스 문구를 표시합니다.
4. **HTTPS**: 프로덕션 환경에서는 반드시 HTTPS를 사용하세요.

---

## 테스트 페이지

SDK 동작을 미리 확인하고 코드를 생성할 수 있는 테스트 페이지를 제공합니다.

👉 **[SDK 테스트 페이지](https://static.adbc.io/cp_db/cpsearch_test.html)**

테스트 페이지에서 다음 기능을 사용할 수 있습니다:
- Token과 Keyword를 입력하여 실제 광고 미리보기
- Grid / Scroll 레이아웃 비교
- 컬럼 수, 상품 개수 등 옵션 조정
- 삽입용 HTML 코드 자동 생성

---
