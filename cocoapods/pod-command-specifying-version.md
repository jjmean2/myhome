> [Home](https://github.com/jjmean2/til) - [Cocoapods](https://github.com/jjmean2/til/tree/master/cocoapods) - [pod 명령어마다 Cocoapods 버전을 지정하여 실행하는 법](https://github.com/jjmean2/til/blob/master/cocoapods/pod-command-specifying-version.md)

### pod 명령어마다 Cocoapods 버전을 지정하여 실행하는 법
pod 명령어를 실행시키는데 버전에 따라 에러가 나는 경우가 있었다. 
예를 들어 Cocoapods을 1.1.0 버전으로 업데이트했는데, `pod init` 명령어가 에러를 내서 0.39 버전으로 다시 깔았는데 이번에는 `pod install` 명령어가 에러를 내는 일이 있었다. 그래서 이를 해결하기 위해 명령어마다 다른 버전의 Cocoapods을 이용하기로 했는데 이럴 때 사용할 수 있는 방법이다.
```
pod _0.39_ init
pod install
```
위처럼 `pod` 다음에 밑줄(`_`) 사이에 버전명을 지정해 주면 해당 버전의 Cocoapods으로 명령어를 실행한다. 버전명 지정이 없으면 기본값(`pod --version` 하면 나오는 버전)으로 실행시킨다.

`vim $(which pod)`을 해보면 `pod`의 스크립트 코드를 볼 수 있는데 여기에서 `pod` 명령어의 버전을 지정하는 방식을 확인할 수 있다.
