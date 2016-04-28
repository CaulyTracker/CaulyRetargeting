Cauly 리타겟팅 연동 가이드
=========================
광고주 Web
--------------------------
### 개요
이 문서는 광고주가 Cauly 와 static 형 리타겟팅 연동을 할 때 tracker 를 삽입하는 방법에 대해서 설명하는 문서입니다.

### 문서 버전 
| 문서 버전 | 작성 날짜 | 작성자 및 내용|
 --- | --- | --- 
| 1.0.0 | 2016. 04. 28. | 권순국(nezy@fsn.co.kr) 초안 작성|



### 목차
- [연동 절차](#연동-절차)
- [연동 상세](#연동-상세)
	- [메인 페이지](#메인-페이지)
		- 스크립트 삽입
	- [상품 상세 페이지](#상품-상세-페이지)
		- 스크립트 삽입
	- [구매 완료 페이지](#구매-완료-페이지)
		- 스크립트 삽입


### 연동 절차
1. Cauly 담당자 혹은 cauly@fsn.co.kr로 연락하여 Cauly 리타겟팅 연동에 대해서 협의합니다.
1. 협의 완료 후, track_code 를 발급받습니다.
1. track_code 를 포함한 tracker 코드를 사이트의 각 페이지에 삽입합니다.
1. 페이지 삽입 이후 연동이 되었는지 Cauly 에서 테스트를 진행합니다.
1. 테스트가 완료되면 광고를 집행합니다.


### 연동 상세
tracker 스크립트의 삽입 위치는 아래 3군데이며 차례대로 기술하겠습니다.
- 메인 페이지
- 상품 상세 페이지
- 구매완료 페이지

Cauly 에서 발급한 track_code 를 aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee 라고 가정하겠습니다.
예제 코드에서 aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee 부분은 따로 발급 받은 track_code 로 대체하여야 합니다.

### 하이브리드 앱인 경우
하이브리드 앱은 앱 안에서 Webview 를 이용하여 컨텐츠를 보여주는 형식의 앱입니다.
이 경우 Cauly Tracker Native SDK 의 일부 기능을 사용하여 Webview 에 WebSDK 와 통신할 수 있는 기능을 추가해야 합니다.
<br>각 SDK의 설치및 활용은 아래 링크에서 확인가능합니다.
<br> 1.0.1 버전 iOS/Android SDK의 Purchase API는 작동하지 않습니다.
- [Android SDK](https://github.com/CaulyTracker/Android-Tracking-SDK)
- [iOS SDK](https://github.com/CaulyTracker/iOS-Tracking-SDK)

#### 메인 페이지
##### 스크립트 삽입
```javascript
<script type="text/javascript" src="//image.cauly.co.kr/script/caulytracker.js"></script>
<script type="text/javascript">
        var mTracker = new CaulyTracker();
        var initData = mTracker.InfoBuilder.setTrackCode("aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee").build();
         mTracker.init(initData);
         mTracker.trackEvent('OPEN');  
</script>
```

스크립트는 <head> 보다는 <body> 태그 안쪽의 마지막 부분에 삽입하는 것이 좋습니다.


#### 상품 상세 페이지
##### 스크립트 삽입
```javascript
<script type="text/javascript" src="//image.cauly.co.kr/script/caulytracker.js"></script>
<script type="text/javascript">
        var mTracker = new CaulyTracker();
        var initData = mTracker.InfoBuilder.setTrackCode("aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee").build();
         mTracker.init(initData);
         mTracker.trackEvent('PRODUCT');  
</script>
```

#### 구매 완료 페이지
##### 스크립트 삽입
```javascript
<script type="text/javascript" src="//image.cauly.co.kr/script/caulytracker.js"></script>
<script type="text/javascript">
         var mTracker = new CaulyTracker();
         var initData = mTracker.InfoBuilder.setTrackCode("aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee").build();
          mTracker.init(initData);

          /* STAR LOOP: 구매한 모든 상품에 대해 */
          mTracker.PurchaseEvent.addPurchase( "{$itemId}", "{$productPrice}", "{$productQuantity}");
          /* END LOOP */

          mTracker.PurchaseEvent.setOrder("{$orderId}", "{$orderPrice}");

          var purchaseEvent = mTracker.PurchaseEvent.build();
          mTracker.trackEvent(purchaseEvent);
</script>
```

##### 스크립트 삽입(재구매를 따로 표시하려면)
```javascript
<script type="text/javascript" src="//image.cauly.co.kr/script/caulytracker.js"></script>
<script type="text/javascript">
         var mTracker = new CaulyTracker();
         var initData = mTracker.InfoBuilder.setTrackCode("aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee").build();
          mTracker.init(initData);

          /* STAR LOOP: 구매한 모든 상품에 대해 */
          mTracker.PurchaseEvent.addPurchase( "{$itemId}", "{$productPrice}", "{$productQuantity}");
          /* END LOOP */

          mTracker.PurchaseEvent.setOrder("{$orderId}", "{$orderPrice}");

          var purchaseEvent = mTracker.PurchaseEvent.build();
          /* 재구매 표시 시작, 재구매 표시를 위해 이 부분이 추가되었다. */
          purchaseEvent['purchase_type']='RE-PURCHASE';
          /* 재구매 표시 끝 */
          mTracker.trackEvent(purchaseEvent);
</script>
```
