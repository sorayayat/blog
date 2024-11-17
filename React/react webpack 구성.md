# CRA 없이 리액트 시작해보자

yarn을 사용할 계획! 
npm이 개선되었지만 무겁다는 평이 있기 때문에 
***yarn***이라는 패키지 매니저를 선택했다.

본인 환경에 맞게 node와 yarn을 설치해주도록 하자

여기서는 mac os 기준으로 설정 과정을 정리한다.

```
yarn init -y
```

터미널에 다음 명령어를 실행하면 package.json 파일이 생성된다.

![](img/Pasted%20image%2020240722215322.png)

필수 패키지인 
```
yarn add react react-dom
```

![](img/Pasted%20image%2020240722215506.png)


대부분 블로그 글에서 typescript도 함께 설치 하지만 일단은 건너뛰고 구성하도록 한다!


yarn add webpack —dev
yarn add webpack-cli --dev

각 웹팩 라이브러리와 cli 명령어로 이용할 수 있는 라이브러리를 설치한다.


yarn add html-webpack-plugin webpack-dev-server ts-loader --dev

html-webpack-plugin: 번들링 후 만들어둔 템플릿(HTML) 파일을 새로 만들어서 output 디렉토리에 생성

webpack-dev-server: 개발 할 때 사용하는 웹 서버 ( 개발용 )

여기서 webpack.common.js 파일을 만든다.
설정파일 common, develop, production으로 나눠서 관리하여 
개발, 배포 등의 환경에 따라 번들링하여 사용한다.

