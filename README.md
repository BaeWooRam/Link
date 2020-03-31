## Deep Link
클릭된 링크나 프로그램의 요청이 웹 URI 인텐트를 호출하면 Android 시스템에서는 요청이 성공할 때까지 다음 각 작업을 순서대로 시도합니다.

1. URI를 처리할 수 있는, 사용자가 선호하는 앱(지정되어 있는 경우)을 엽니다.
2. URI를 처리할 수 있는, 사용 가능한 유일한 앱을 엽니다.
3. 사용자가 대화상자에서 앱을 선택하도록 합니다.

- action
Google 검색에서 인텐트 필터에 도달할 수 있도록 ACTION_VIEW 인텐트 작업을 지정합니다.

- data
data 태그를 하나 이상 추가합니다. 이 태그는 활동으로 확인되는 URI 형식을 나타냅니다. 최소한 <data> 태그는 android:scheme 속성을 포함해야 합니다. 
속성을 추가하여 활동이 수락하는 URI 유형을 더욱 세분화할 수 있습니다. 예를 들어, 유사한 URI를 수락하지만 경로 이름에 따라 달라지는 여러 활동이 있을 수 있습니다. 이 경우, android:path 속성이나 그 변형(pathPattern 또는 pathPrefix)을 사용하여 다양한 URI 경로별로 열리는 활동을 차별화해야 합니다.

- category     
BROWSABLE : 카테고리를 포함합니다. 웹브라우저에서 인텐트 필터에 액세스할 수 있으려면 필요합니다. 이 카테고리가 없는 경우 브라우저에서 링크를 클릭해도 앱으로 확인되지 않습니다.     
DEFAULT : 카테고리도 포함합니다. 그러면 앱이 암시적 인텐트에 응답할 수 있습니다. 이 카테고리가 없으면 인텐트에서 앱 구성요소 이름을 지정한 경우에만 활동을 시작할 수 있습니다.    

```
<activity
    android:name="com.example.android.GizmosActivity"
    android:label="@string/title_gizmos" >
    <intent-filter android:label="@string/filter_view_http_gizmos">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <!-- Accepts URIs that begin with "http://www.example.com/gizmos” -->
        <data android:scheme="http"
              android:host="www.example.com"
              android:pathPrefix="/gizmos" />
        <!-- note that the leading "/" is required for pathPrefix-->
    </intent-filter>
    <intent-filter android:label="@string/filter_view_example_gizmos">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <!-- Accepts URIs that begin with "example://gizmos” -->
        <data android:scheme="example"
              android:host="gizmos" />
    </intent-filter>
</activity>
```

두 인텐트 필터는 data 요소만 다릅니다. 동일한 필터에 여러 data 요소를 포함하는 것이 가능하지만 고유한 URL(예: scheme과 host의 특정 조합)을 선언할 의도라면 별도의 필터를 만드는 것이 중요합니다. 동일한 인텐트 필터의 여러 data 요소가 실제로 함께 병합되어 결합된 속성의 모든 변형을 나타내기 때문입니다.

```
<intent-filter>
  ...
  <data android:scheme="https" android:host="www.example.com" />
  <data android:scheme="app" android:host="open.my.app" />
</intent-filter>
```
    
원본 : https://developer.android.com/training/app-links/deep-linking?hl=ko
## App Link
App Link는 사용자가 앱을 선택할 필요 없이, 웹사이트의 URL을 클릭하면 Android App에서 해당 콘텐츠가 바로 열리도록 하는 특별한 유형의 Deep Link입니다.

- 작동순서
1. manifest에서 자동 앱 링크 확인을 요청합니다. 이렇게 하면 앱이 인텐트 필터에 사용된 URL 도메인에 포함되는지 여부를 확인하도록 Android 시스템에 신호를 보냅니다.    
    
2. 다음 위치에서 디지털 애셋 링크 JSON 파일을 호스팅하여 웹사이트와 인텐트 필터 사이의 관계를 선언합니다.    
```
https://domain.name/.well-known/assetlinks.json
```

- 인증 요청
App의 링크 처리 인증을 사용 설정하려면 다음 manifest 코드 스니펫과 같이 
__앱 manifest의 웹 URL 인텐트 필터 중 하나에서 android:autoVerify="true"__
를 설정합니다. (android.intent.action.VIEW 인텐트 작업 및 android.intent.category.BROWSABLE 인텐트 카테고리가 포함된 앱 manifest에서).    

```
<activity ...>

        <intent-filter android:autoVerify="true">
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="http" android:host="www.example.com" />
            <data android:scheme="https" />
        </intent-filter>

    </activity>
```
    
원본 : https://developer.android.com/training/app-links/verify-site-associations?hl=ko

- 웹사이트 연결 선언하기
디지털 애셋 링크 JSON 파일을 웹사이트에 게시하여 웹사이트와 연결된 Android 앱을 나타내고 앱의 URL 인텐트를 인증해야 합니다. JSON 파일은 다음 필드를 사용하여 연관된 앱을 식별합니다.    
    
1. package_name: 앱의 build.gradle 파일에서 선언한 애플리케이션 ID입니다.
2. sha256_cert_fingerprints: 앱 서명 인증서의 SHA256 지문 파일입니다. 다음 명령어를 사용하여 자바 keytool을 통해 지문 파일을 생성할 수 있습니다.
    
```
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target" : { "namespace": "android_app", "package_name": "com.example.app",
               "sha256_cert_fingerprints": ["hash_of_app_certificate"] }
}]
```
    
원본 : 
https://developer.android.com/training/app-links/verify-site-associations?hl=ko
https://developers.google.com/digital-asset-links/v1/getting-started?hl=ko

## Deep vs App

- Deep Link
Deep Link는 사용자가 Android App의 특정 활동으로 바로 이동할 수 있게 하는 인텐트 필터입니다. 이 링크 중 하나를 클릭하면 특정 URL을 처리할 수 있는 여러 앱(내가 개발한 앱 포함) 중 하나를 사용자에게 선택하게 하는 명확성 대화상자가 열릴 수 있습니다.     

![image](https://user-images.githubusercontent.com/41356481/78000310-0a530b00-736f-11ea-8fe3-d8da5220e4be.png)

- App Link
App Link는 개발자의 웹사이트에 속한다는 것이 확인된, 웹사이트 URL 기반의 딥 링크입니다. 이 링크 중 하나를 클릭하면 설치되어 있는 앱이 즉시 열리며, 명확성 대화상자는 표시되지 않습니다.     

![image](https://user-images.githubusercontent.com/41356481/78000413-31a9d800-736f-11ea-9bf5-ed8adea57c85.png)

## Firebase Dynamic Link
