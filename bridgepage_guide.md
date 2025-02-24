# Bridge Page guide for advertiser

### Getting Started

- 본 가이드는 귀사의 캠페인을 여러 매체사들에게 노출시키기 위한 가이드입니다
- 해당 브릿지 페이지 연동을 통해 더 많은 트래픽을 확보할 수 있습니다
- ADBC 페이지 내에 귀사의 브릿지 페이지가 iframe으로 삽입되고 postMessage를 통해 데이터를 전달받습니다
- ADBC 페이지에서 해당 데이터를 전달받아 각 매체사에게 필요한 정보를 제공합니다

---
# 1.미션 참여 이동

- 일반적으로 브릿지 페이지에서 '미션 참여하러가기' 버튼을 클릭하면 해당 미션 참여 페이지로 이동합니다
- 그 대신 아래 코드를 추가하여 미션 참여 페이지로 이동시켜야합니다

```html
// 미션 페이지로 이동하기 코드

try {
    // iframe 내부인지 확인
    const isIframe = window !== window.parent;

    if (isIframe) {
        // 전달할 데이터
        const message = {
            functionName: "api_landing_url", 
            args: ["https://test.com/landing_page.html?test=true"]        // 랜딩 URL로 넣어주시면 됩니다
        };
        // 부모 페이지로 postMessage 전송
        window.parent.postMessage(message, "https://sns.adbc.io");
        console.log("메시지가 부모 페이지로 전송되었습니다:", message);
    } else {
        console.log("이 페이지는 iframe 내부에 포함되어 있지 않습니다.");
    }
} catch (e) {
    console.error("iframe 여부 확인 또는 postMessage 전송 중 오류 발생:", e);
}
```
- 사용자가 '미션 참여하러 가기' 버튼을 누를 경우 위 코드가 실행되어야 합니다
- message 내용 중 args에 랜딩 URL을 넣어주시면 됩니다

# 2.미션 참여 완료

- 사용자가 미션을 완료하고 정상적으로 실적이 발생했을 때 아래의 코드를 실행합니다

```html
try {
    // iframe 내부인지 확인
    const isIframe = window !== window.parent;

    if (isIframe) {
        // 전달할 데이터
        const message = {
            functionName: "api_callback", 
            args: ["success"]        // 정답 여부 전달. success, fail
        };
        // 부모 페이지로 postMessage 전송
        window.parent.postMessage(message, "https://sns.adbc.io");
        console.log("메시지가 부모 페이지로 전송되었습니다:", message);
    } else {
        console.log("이 페이지는 iframe 내부에 포함되어 있지 않습니다.");
    }
} catch (e) {
    console.error("iframe 여부 확인 또는 postMessage 전송 중 오류 발생:", e);
}
```


## Authors

* **CHOI BAWOO** - *Integration technical support* - bw@adbc.co.kr





