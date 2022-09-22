# 프런트엔드 개발 도구

프런트엔드에서는

- 의존성 관리
- 구형 브라우저의 호환을 위한 트랜스 파일링
- 소스 번들링

등을 위한 다양한 도구가 존재한다.

## JavaScript의 의존성 관리

⬅️  **과거에 자바스크립 파일들을 관리한 방법**

- JavaScript 파일을 CDN으로 가져온다.
- 웹 서버 또는 로컬 환경에서 별도로 파일을 관리한다.

❗ **단점**

- 전역 스코프를 공유하기 때문에 다른 파일에 영향을 준다.
- 각 파일이 의존성 맞게 순서대로 로딩되어야 하기 때문에, 파일 간 의존성 확인이 필요하다.
- CDN을 통해 의존성을 가져오면 네트워크 요청이 증가하여 성능 측면의 단점이 발생했다.

→ 최근 기준, 대부분의 프런트엔드 프로젝트는 **Node.js와 npm**을 사용하여 의존성을 관리하고 있다.

## Node.js와 프런트엔드 개발

Node.js는 크롬 V8 자바스크립트 엔진을 사용하여 만들어진 이벤트 기반의 자바스크립트 런타임 플랫폼

**브라우저가 아닌 환경에서 자바스크립트를 실행**할 수 있게 되며, 서버 개발 플랫폼으로 사용된다.

## npm(Node Package Manager)

Node.js의 패키지를 관리하는 도구이다.

모든 패키지는 `npm 레지스트리`에 저장된다.

npm 레지스트리에는 수많은 라이브러리와 프레임워크가 저장되어 있다.

npm은 **npm CLI(Command Line Interface)**를 사용하여 간편하게 설치할 수 있다

> **npm의 패키지**는 **레지스트리에 배포된 모듈**을 의미한다.
> 패키지를 실제 프로덕션에서 사용할 수 있고, 이러한 패키지를 의존성 패키지라고 한다.
> **npm의 패키지**를 **의존성** 또는 **의존성 패키지**와 같은 의미로 사용하기도 한다.

### npm의 의존성 관리

npm은 `package.json` 파일을 통해 프로젝트 정보와 패키지 정보를 관리한다.

`$ npm init` 명령어 실행 시 프로젝트에 대한 이름, 버전, 라이선스 등 여러가지 정보를 입력하고,

이 정보를 기준으로 pakage.json이 생성된다.

### 의존성 설치와 제거

`$ npm install` 명령어 뒤에 설치할 패키지를 입력하면 패키지의 가장 최신 버전이 설치된다.

설치 명령어는 지역(local) 설치와 전역(global) 설치 옵션이 존재한다.

### **지역 설치**

- 옵션을 별도로 지정하지 않으면 지역 설치가 된다.
- 프로젝트 루트에 node_modules 폴더가 자동으로 생성되고, 패키지가 설치된다.
- 지역 설치된 패키지는 해당 프로젝트 내에서만 사용할 수 있다.

```jsx
-- 패키지 모듈 설치
$ npm install <package>

-- 버전 명시하여 특정 모듈 설치
$ npm install <package>@1.0.0

-- pakage.json 파일에 정의된 의존성 패키지 모두 설치
$ npm install
```

### 전역 설치

- `-g` 옵션을 추가한다.
- OS의 특정 경로에 설치되며, OS마다 설치 경로가 다르다.

```jsx
$ npm install -g <package>
$ npm install -g <package>@1.0.0
```

- 기본적으로 `C:\Users\사용자\AppData\Roaming\npm\node_modules` 에 설치된다.
- 설치 위치는 `npm config` 명령어를 사용해서 변경할 수 있다.
  - [설치 위치를 변경하고 싶다면?](https://www.davidyardy.com/blog/change-default-global-installation-directory-for-nodejs-on-windows/)

### 제거

`npm uninstall` 명령어로 제거한다.

```jsx
$ npm uninstall <package>
$ npm uninstall -g <package>
```
