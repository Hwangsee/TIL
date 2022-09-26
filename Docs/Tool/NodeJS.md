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

## pakage.json의 중요 속성

npm은 pakage.json을 통해 Node.js의 패키지를 관리한다.

- name : 프로젝트(패키지)의 이름.
- version : 프로젝트 버전 (semantic versioning 규칙을 따름)
- dependencies : 의존성 패키지 정의. **실제 프로덕션 배포에도 필요한 패키지**이다.
- devDependencies : dependencies와 마찬가지로 의존성 패키지이지만, **실제 프로덕션 배포에는 필요하지 않고 개발에 필요한 패키지**만 정의.
- repository : 소스 코드가 저장된 리포지터리 주소.
- author : 개발자 정보.
- license : 라이선스.
  - [여기](https://olis.or.kr/)에 라이선스의 특징이 잘 정리되어 있다.
- main : 패키지 설치하는 곳에서 진입점으로 사용할 파일
  ```jsx
  // pakage.json
  {
  	"name" : "my-project",
  	"version" : "1.0.0",
  	"main" : "index.js"
  }

  // pakage.json의 main 속성에 정의된 index.js 파일 기준으로 모듈을 가져옴.
  import main from 'my-project';
  ```
- files : npm 레지스트리 배포 시, 포함해야 할 폴더나 파일을 정의
  - files 속성이 있다면, 해당 속성에 정의된 파일들만 배포한다.
- types : 타입스크립트 사용 시, type 속성에 타입 정의 파일을 명시. (typing 속성 역시 동일)

> 🍳 **devDependency와 dependencies**
> npm install 명령어 뒤 -D 옵션 혹은 --save-dev를 추가하면 개발 환경에 사용될 패키지를 설치한다.
> 외부에서 특정 패키지 설치 시 dependencies는 함께 설치되지만 devDependencies는 설치되지 않는다.

### 의존성 버전과 semantic versioning

각기 다른 버전 방식을 따르는 소프트웨어, 패키지는 동일한 규칙으로 통합 관리하기 위해 등장

X.Y.Z의 양의 정수로 정의한다.

- X : Major 버전
  - 프로젝트 전반적인 큰 구조 변경, 기존 API, 옵션과 호환되지 않는 변경 사항이 있을 경우.
  - 사용자 측에 많은 변경을 요구하기 때문에 마이그레이션 가이드, 스크립트를 제공해주는 경우가 많음.
- Y : minor 버전
  - 하위 버전과 호환되는 기능을 추가할 때
- Z : patch 버전
  - 이전 버전과 호환되는 버그를 수정할 때

### npm의 의존성 버전 관리

npm install 명령어를 사용하여 패키지 설치 시 pakage.json에 ^(캐럿) 표시와 함께 패키지 버전이 명시됨.

```jsx
"dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^5.3.0",
    "react-scripts": "5.0.1",
  },
```

### 캐럿

**0이 아닌 버전 요소 중 가장 왼쪽에 있는 요소를 수정하지 않는 범위에서 패키지를 설치**

ex ) pakage.json에 ^1.2.3 버전으로 정의된 패키지의 최신 버전이 1.3.0이면, 1.3.0 버전 설치.
^0.2.3으로 정의되어 있고 최신 버전이 0.3.0이면, 0.2.9 설치

> 💡  **~(틸드)를 사용하면 특정 버전으로 고정하거나 마이너 버전만 패치 버전만 업데이트 할 수 있다.**

## pakage-lock.json 파일의 중요성

pakage.json은 버전 범위를 사용하여 패키지를 설치하기 때문에 설치 시점마다 패키지 버전이 달라질 수 있음.

→ 버전 충돌이나, 프로젝트 개발자마다 다른 버전 패키지가 설치되어 동작 오류 발생할 가능성이 있음.

**pakage-lock.json은 설치 시점 당시의 의존성 트리 정보를 저장하고 있으며, pakage-lock.json이 있다면 npm은 이 파일의 의존성 트리를 기준으로 패키지**를 설치한다.
