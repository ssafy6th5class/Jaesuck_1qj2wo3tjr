# Vue CLI & Vue router

- SFC (Single File Component)

  - Vue의 컴포넌트 기반 개발의 핵심 특징
  - 하나의 컴포넌트는 .vue 확장자를 가진 하나의 파일 안에서 작성되는 코드의 결과물
  - Vue 컴포넌트 === Vue 인스턴스 === .vue 파일

- Component

  - 기본 HTML element를 확장하여 재사용 가능한 코드를 캡슐화 하는데 도움을 준다
  - 유지보수를 쉽게 만들어 주고, 재사용성의 측면에서 매우 강력한 기능 제공
  - Vue 컴포넌트 === Vue 인스턴스

  ```js
  # Vue 컴포넌트 
  const app = new Vue({
      ...
  })
  ```

  

- Vue CLI : Vue.js 개발을 위한 표준 도구
- Node.js
  - JS를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 JS 런타임 환경
  - 단순 브라우저만 조작하던 JS를 SSR  아키텍처에서도 사용가능하게함

- NPM (Node Package Manage)

  - JS 언어를 위한 패키지 관리자
    - ex) python 에 pip 이라면 Node.js 에 NPM
  - Node.js 의 기본 패키지 관리자
  - Node.js  설치 시 함께 설치 됨

  ```bash
  # bash
  npm install -g @vue/cli
  vue --version
  
  # vscode
  vue create 'project이름'     // 프로젝트 생성
  
  npm run serve               // 서버 실행
  ```

- Vue 프로젝트 구조

  - bable.config.js

    - JS compiler

    - JS 의 ECMAScript 코드를 이전 버전으로 번역 / 변환해 주는 도구

      ```js
      // 원시 코드(ES6+)
      [1,2,3].map((n) => n + 1);
      
      // 목적 코드(ES5)
      [1,2,3].map(function(n) {
          return n + 1;
      })
      ```

  - Webpack

    - static module bundler
      - module : 파일 하나를 의미 (스크립트 1개 === 모듈 1개)
      - module 의존성 문제 : 모듈의 수가 많아지고 모듈 간의 의존성이 깊어지면서 특정 문제가 어떤 모듈 간의 문제인지 파악하기 어려워짐
      - Bundler : 모듈 의존성 문제를 해결해주는 작업을 bundling 이라고 한다
      - 이러한 일을 해주는 도구가 Bundler 이고, webpack은 다양한 bundler 중 하나
    - 모듈 간의 의존성 문제를 해결하기 위한 도구
    - 프로젝트에 필요한 모든 모듈을 매핑하고 내부적으로 종속성 그래프를 빌드한다

  - Node.js : JS를 브라우저 밖에서 실행할 수 있는 새로운 환경
  - Bable : ES6+ 코드를 그 이전의 코드로 바꿔주는 compiler
  - webpack : 모듈 간의 의존성 문제를 해결하기 위한 도구

- Pass props & Emit event

  - 부모는 자식에게 데이터를 전달 (Pass props), 자식은 자신에게 일어난 일을 부모에게 알림 (Emit event)
  - 부모는 props를 통해 자식에게 '데이터' 전달
  - 자식은 events를 통해 부모에게 '메시지' 전달

- 컴포넌트 구조

  - 템플릿(HTML)

    - HTML의 body 부분
    - 각 컴포넌트를 작성

    ```js
    <template>
      <div>
    
      </div>
    </template>
    ```

  - 스크립트(JavaScript)

    - JS 가 작성되는 곳
    - 컴포넌트 정보, 데이터, 메서드 등 vue 인스턴스를 구성하는 대부분이 작성 됨

    ```js
    <script>
    export default {
        
    }
    </script>
    ```

  - 스타일(CSS)

    - 컴포넌트의 스타일을 담당

    ```js
    <style>
    
    </style>
    ```

- 컴포넌트 등록 3단계

  - 불러오기

    ```js
    <script>
    // 불러오기 //
    import HelloWorld from '@/components/HelloWorld.vue'
    </script>
    ```

  - 등록하기

    ```js
    export default {
      name: 'Home',
      components: {
        // 등록하기 //
        HelloWorld
      }
    }
    ```

  - 보여주기

    ```vue
    <template>
      <div class="home">
        <img alt="Vue logo" src="../assets/logo.png">
        // 보여주기 //
        <HelloWorld msg="Welcome to Your Vue.js App"/>
      </div>
    </template>

- Static Props 작성

  ```vue
  // App.vue
  <template>
    <div class="app">
      <img alt="Vue logo" src="../assets/logo.png">
        <about my-message="This is prop data"></about>   ==> 자식 컴포넌트(About.vue)에 보낼 prop 데이터 선언
    </div>
  </template>
  ```

  ```vue
  <template>
    <div>
  	<h1>About</h1>
      <h2>{{ myMessage }}</h2>
    </div>
  </template>
  
  <script>
  export default {
    name: 'About',
    props: {               
   	myMessage: String,            
    }
  }
  </script>
  ```

- Props 이름 컨벤션

  - during declaration (선언시) : camelCase
  - in template (HTML) : kebab-case

- 단방향 데이터 흐름
  - 모든 props는 하위 속성과 상위 속성 사이의 단방향 바인딩을 형성
  - 부모의 속성이 변경되면 자식 속성에게 전달되지만, 반대 방향으로는 안된다

- Emit event
  - $emit(eventName) -> 현재 인스턴스에서 이벤트를 트리거
  - 추가 인자는 리스터의 콜백 함수로 전달
  - 부모 컴포넌트는 자식 컴포넌트가 사용되는 템플릿에서 v-on 을 사용해서 자식 컴포넌트가 보낸 이벤트를 청취 (v-on 외의 방법도 가능)
- evnet 이름 컨벤션
  - 컴포넌트 / props 와 달리 이벤트는 자동 대소문자 변환 x
  - v-on:myEvent ==> v-on:myevent 로 자동 변환. 따라서 kebab-case 사용을 권장한다



- Veu Router

  - Vue.js 공식 라우터
  - 라우트에서 컴포넌트를 매핑한 후 어떤 주소에서 렌더링 할 지 알려준다

  ```js
  $vue create "프로젝트 이름"
  $vue add router
  ```

  - <router-link>

    - 사용자에게 네비게이션 가능하게 하는 컴포넌트
    - 목표 경로는 'to' prop으로 지정된다

    ```js
    <router-link to="/">Home</router-link>
    ```

- History mode

  - HTML History API를 사용해서 router 구현한 것
  - 브라우저의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능
  - 페이지를 다시 로드하지 않고 URL을 탐색할 수 있다  ==> SPA의 단점 "URL이 변경되지 않는다" 해결

- 프로그래밍 방식 네비게이션

  - router의 인스턴스 메서드를 사용하여 프로그래밍 방식으로 <router-link> 기능을 수행할 수 있다

    |      선언적 방식      | 프로그래밍 방식 |
    | :-------------------: | :-------------: |
    | <router-link to =" "> | $router.push( ) |

  

- Vue Router 필요한 이유

  |       SPA 등장 이전        |            SPA 등장 이후            |                라우팅 처리 차이                |
  | :------------------------: | :---------------------------------: | :--------------------------------------------: |
  |  서버가 모든 라우팅 통제   |    서버는 index.html 하나만 제공    |    SSR : 라우팅에 대한 결정권을 서버가 가짐    |
  | 요청 경로에 맞는 HTML 제공 | 요청에 대한 처리를 서버가 하지 않음 | CSR : 라우팅에 대한 결정권을 클라이언트가 가짐 |

  
