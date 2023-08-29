
# iOS App Life Cycle
## 앱의 실행 과정
1. main 함수 실행
2. main 함수는 UIApplicationMain 함수 호출
3. UIApplication 함수는 UIApplication 객체 생성
4. nib 파일을 사용하는 경우나, Info.plist 파일을 읽어들여 파일의 기록된 정보를 참고하여 그 외에 필요한 데이터를 로드
5. App Delegate 객체를 만들어 App 객체와 연결하고 RunLoop 를 만드는 등 실행에 필요한 준비
6. 실행 완료를 앞두고 App 객체가 App Delegate 에게 application:didFinishLaunchingWithOptions 메시지 전송


## The Main Function
