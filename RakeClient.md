# RakeClient

**Rake** 는 단말에서 서버로 로그를 *쉽고*, *안전하게* 전송할 수 있도록 도와주는 *가벼운* 라이브러리입니다. 

Android / iOS / Javascript 지원


## Rake

<br/>

> **Rake:** 

> [타동사]
> : 갈퀴질하다; <불을> 헤치다; (갈퀴 등으로) 긁어모으다 ((together))

<br/>

Rake는 단말 앱(Android, iOS) 이나 웹(WebApp, Hybrid)에서 로그를 전송하는 것을 도와주는 클라이언트 라이브러리입니다. 한 블럭의 코드를 넣는것만으로도 쉽게 로그를 전송할 수 있습니다. 오픈소스인 [Mixpanel](https://github.com/mixpanel) 을 필요한만큼 수정하여 제작하였습니다.

혹시 **Sentinel Shuttle** 을 찾아오셨다면, [Shuttle API Document](https://github.com/skpdi/sentinel-document/wiki/Sentinel-Shuttle) 를 확인해주세요.

<br/>

### 왜 Rake 를 써야 하나요?

- **light weight:** 가볍습니다. 메인스레드에 영향을 주지 않도록 별도의 스레드로 동작합니다. 또한 경량성을 위해 *mixpanel* 에서 필요 없는 부분을 제거했습니다.

- **fault-tolerant:** 견고합니다. *Network Error* 을 비롯해서 각종 *Exception* 을 고려하여 설계되었습니다. 모든 로그는 로컬 데이터베이스(sqlite) 에 저장되는데, 꽉차거나, *time-out* 이 되면 로그를 서버로 전송합니다. 만약 이 과정에서 실패했다면, 다시 로컬 데이터베이스에 저장하기 때문에 로그 유실을 막아줍니다.

- **easy to use:** 개발이 쉽습니다. `os`, `browser` 등의 모든 단말에서 동일한 정보들은 개발자가 지정하지 않아도 자동으로 전송됩니다. 또한 해당 애플리케이션 내에서 매번 전송되는 필드를 *SuperProperty* 를 이용해서 자동으로 전송되도록 지정할 수 있어 편리합니다.  마지막으로, `setBodyOf~` 메소드를 이용하면 로그 키값을 신경쓰지 않으면서 남겨야 할 정보만 파라미터로 넘기면 됩니다. 이 과정에서 파라미터가 부족하면 컴파일 에러가 나기 때문에, 로그 전송 오류를 최소화 할 수 있습니다.

<br/>  
\* 웹의 경우엔 즉시 *flush* 됩니다. 이는 페이지가 전환되면 가지고 있는 정보(Javascript Object)가 사라지기 때문입니다. 물론 설정을 통해서 즉시 *flush* 되지 않도록 변경할 수 있습니다.  
\* 웹은 *Local Database* 를 지원하지 않습니다. 이는 모든 브라우저가 *local storage API* 를 지원하지 않기 때문입니다.

<br/>

### RakeClient API 사용 가이드

(1) Android

[Rake Android API Usage](https://github.com/skpdi/rake-document/wiki/1.-Rake-Android)  
[Rake Android Example Project](https://github.com/skpdi/rake-android-example)

(2) iPhone

[Rake iPhone API Usage](https://github.com/skpdi/rake-document/wiki/2.-Rake-iPhone)  
[Rake iPhone Example Project](https://github.com/skpdi/rake-iphone-example)

(3) Web (Javascript)

[Rake Web API Usage](https://github.com/skpdi/rake-document/wiki/3.-Rake-Web)  
[Rake Web Example Project](https://github.com/skpdi/rake-web/tree/master/example)

<br/>

### License

**Apache V2** 를 따릅니다. 이는 개발에 사용된 [Mixpanel](https://github.com/mixpanel) 이 Apache V2 를 따르기 때문입니다. 라이센스에 따라, 이를 사용하는 앱에서는 라이브러리 사용 여부를 표시해야 하기 때문에 라이센스 전문을 링크합니다.

- [Apache V2](http://www.apache.org/licenses/LICENSE-2.0.html)
- [Mixpanel: iPhone](https://github.com/mixpanel/mixpanel-iphone/blob/master/LICENSE)
- [Mixpanel: Android](https://github.com/mixpanel/mixpanel-android/blob/master/LICENSE)
- [Mixpanel: JS](https://github.com/mixpanel/mixpanel-js/blob/master/LICENSE)
- [JSnappy (for prior to Rake 0.3)](https://code.google.com/p/jsnappy/source/browse/trunk/LICENCE.txt)

<br/>

### FAQ

#### 1. Sentinel Dev Log Tailor 에서 로그가 보이지 않아요.

- `track`, `flush` 호출시 Server Response 를 확인해주세요. `-1` 이면 토큰값이 잘못된 경우입니다. 

<br/>

#### 2. 구글 시트에서, 어떤 필드들이 자동수집 컬럼인가요?

아래 나열된 필드들은 플랫폼에 따라서 Rake 에서 자동수집 합니다.

|Platform|Field Name|Hive Table Type|Description|Example|
|---|---|---|---|---|---|
|App / Web|base_time|string|단말 기준 서울 시각|20150816231055760|
|App / Web|local_time|string|단말 기준 지역 시각|20150916231055760|
|App / Web|recv_time|string|서버 기준 입수 시각|20150716231055760|
|App / Web|os_name|string|OS 이름||
|App / Web|os_verion|string|OS 버전||
|App / Web|resolution|string|화면 해상도 (width * height)||
|App / Web|screen_width|string|화면 가로 크기||
|App / Web|screen_height|string|화면 세로 크기||
|App / Web|rake_lib|string|Rake Platform|web, android, iphone 중 1|
|App / Web|rake_lib_version|string|Rake Library 버전||
|App / Web|ip|string|IP||
|App / Web|recv_host|string|로그 입수 서버 이름||
|App / Web|token|string|Rake Token|20150716231055760|
|App / Web|log_version|string|로그 스키마 버전(구글 시트)|15.06.23:1.5.29:9|
|App|app_version|string|Application Version||
|App|carrier_name|string|통신사||
|App|network_type|string|와이파이 사용 여부|WIFI, NOT WIFI, UNKNOWN 중 1|
|App|language_code|string|언어 코드||
|App|device_id|string|기기 고유 ID||
|App|devic_model|string|기기 모델 명|SNM-4105|
|Web|browser_name|string|단말 기준 서울 시각|20150716231055760|
|Web|browser_version|string|단말 기준 서울 시각|20150716231055760|
|Web|referrer|string|단말 기준 서울 시각|20150716231055760|
|Web|url|string|단말 기준 서울 시각|20150716231055760|
|Web|document_title|string|단말 기준 서울 시각|20150716231055760| 

<br/>

#### 3. Device ID 는 어떤 값을 수집하나요?

1. Android 

`Settings.Secure.ANDROID_ID`

> A 64-bit number (as a hex string) that is randomly generated when the user first sets up the device and should remain constant for the lifetime of the user's device. The value may change if a factory reset is performed on the device.

[참조 - Settings.Secure](http://developer.android.com/reference/android/provider/Settings.Secure.html#ANDROID_ID)

디바이스 공장 초기화를 하면 값이 변경됩니다.

2. iPhone

`UIDevice.identifierForVendor` (IDFV)

> An alphanumeric string that uniquely identifies a device to the app's vendor. (read-only)Edit

[참조 - UIDevice.identiferForVender](https://developer.apple.com/library/ios/documentation/uikit/reference/UIDevice_Class/Reference/UIDevice.html#//apple_ref/occ/instp/UIDevice/identifierForVendor)

Vendor에 대해 기기별로 유니크한 값을 돌려줍니다. (e.g package `com.skplanet..` 의 앱에 대해서만 같은 값을 리턴함)

3. Web

수집하지 않습니다.


<br/>
#### 4. 개발환경과 라이브환경은 어떻게 다른가요?

짧게 요약하자면

- 개발환경에서는 로그가 즉시 *flush* 됩니다.
- 개발환경과 라이브환경용 토큰이 따로 있습니다. 따라서 배포시에는 라이브 토큰으로 세팅하셔야 합니다.
- 개발환경 세팅과, 라이브환경 세팅이 다릅니다. 개발환경으로 세팅하면, 개발용 *end-point* 로 로그가 전송됩니다. 이 로그는 개발환경에서 발생한 로그이기 때문에 분석하지 않습니다. 반대로, 라이브환경으로 세팅하면, 라이브용 *end-point* 로 로그가 전송됩니다.

**따라서 개발 환경으로 세팅하고, 앱을 마켓에 릴리즈하면 로그가 유실됩니다.**  
**반대로 라이브환경으로 세팅하고, 개발을 진행하면 잘못된 로그가 실서버로 전송되어, 로그 분석 과정에서 매우 큰 오차가 발생할 수 있습니다.**
  
**반드시 토큰과 세팅을 릴리즈/개발용으로 맞추어 주세요.**

<br/>
