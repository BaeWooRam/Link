## Deep Link

## App Link
App Link는 사용자가 앱을 선택할 필요 없이, 웹사이트의 URL을 클릭하면 Android App에서 해당 콘텐츠가 바로 열리도록 하는 특별한 유형의 Deep Link입니다.
    
- 작동순서
1. manifest에서 자동 앱 링크 확인을 요청합니다. 이렇게 하면 앱이 인텐트 필터에 사용된 URL 도메인에 포함되는지 여부를 확인하도록 Android 시스템에 신호를 보냅니다.    
    
2. 다음 위치에서 디지털 애셋 링크 JSON 파일을 호스팅하여 웹사이트와 인텐트 필터 사이의 관계를 선언합니다.    
```
https://domain.name/.well-known/assetlinks.json
```

## Deep vs App

- Deep Link
Deep Link는 사용자가 Android App의 특정 활동으로 바로 이동할 수 있게 하는 인텐트 필터입니다. 이 링크 중 하나를 클릭하면 특정 URL을 처리할 수 있는 여러 앱(내가 개발한 앱 포함) 중 하나를 사용자에게 선택하게 하는 명확성 대화상자가 열릴 수 있습니다.     

![image](https://user-images.githubusercontent.com/41356481/78000310-0a530b00-736f-11ea-8fe3-d8da5220e4be.png)

- App Link
App Link는 개발자의 웹사이트에 속한다는 것이 확인된, 웹사이트 URL 기반의 딥 링크입니다. 이 링크 중 하나를 클릭하면 설치되어 있는 앱이 즉시 열리며, 명확성 대화상자는 표시되지 않습니다.     

![image](https://user-images.githubusercontent.com/41356481/78000413-31a9d800-736f-11ea-9bf5-ed8adea57c85.png)

## Firebase Dynamic Link
