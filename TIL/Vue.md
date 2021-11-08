# Vue

- Front-end Development
  - HTML, CSS, JS 를 활용해서 데이터를 볼 수 있게 만들어 준다.
  - 대표적인 프론트엔드 프레임워크로 Vue.js / React / Angular 가 있다

- Vue.js
  - 사용자 인터페이스를 만들기 위한 JS 프레임워크
  - SPA 를 지원
- SPA (Single-Page-Application)
  - 현재 페이지를 동적으로 렌더링함으로써 사용자와 소통하는 웹 어플리케이션
  - 서버로부터 최초에만 페이지를 다운로드하고, 이후에는 동적으로 DOM 구성
  - 동작 원리의 일부가 CSR(Client Slide Rendering)의 구조를 따른다
- CSR (Client Slide Rendering)
  - 서버에서 화면을 구성하는 SSR 방식과 달리 클라이언트에서 화면을 구성
  - 최초 요청 시 HTML / CSS / JS 등 데이터를 제외한 리소스를 응답받고, 클라이언트에서는 필요한 데이터만 요청해 렌더링하는 방식
  - 장점 : 서버와 클라이언트 간 트래픽 감소, 사용자 경험 향상
  - 단점 : SSR 에 비해 전체 페이지 렌더링 시점이 느림, SEO(검색 엔진 최적화)에 어려움이 있음 -> 최초 문서에 데이터가 없기 때문

- SSR (Server Slide Rendering)

  - 서버에서 사용자에게 보여줄 페이지를 모두 구성하여 사용자에게 페이지를 보여주는 방식

  - SSR을 사용하면 모든 데이터가 매핑된 서비스 페이지를 클라이언트(브라우저)에게 바로 보여줄 수 있다

  - 장점 : 초기 구동 속도가 빠름, SEO에 적합

  - 단점 : 모든 요청마다 새로운 페이지를 구성하여 전달 -> 반복되는 전체 새로고침으로 사용자 경험이 떨어짐, 상대적으로 트래픽이 많아 서버의 부담이 큼

    |               |                    SSR                    |               CSR                |
    | :-----------: | :---------------------------------------: | :------------------------------: |
    | 렌더링의 주최 |                   서버                    |            클라이언트            |
    |     장점      |          초기 구동 속도가 빠르다          | 서버와 클라이언트 간 트래픽 감소 |
    |     단점      |        요청마다 새로운 페이지 구성        | 전체 페이지 렌더링 시점이 느리다 |
    |               | SEO에 적합(처음부터 데이터가 모두 있어서) |           SEO에 부적합           |

  

- Vue.js MVVM Pattern : 애플리케이션 로직을 UI로부터 분리하기 위해 설계된 디자인 패턴
  - Model
    - JS 의 Object 자료 구조
    - Object는 Vue instance 내부에서 data로 사용되는데, 이 값이 바뀌면 View(Dom)가 반응
    - 서버에서 가져온 데이터를 JS 객체 형태로 저장
  - View
    - DOM(HTML)
    - Data의 변화에 따라 바뀌는 대상
    - 사용자에게 보여지는 화면
  - View Model
    - View 와 Model 사이에서 Data와 DOM 에 관련된 모든 일을 처리



- Vue instance

  - 모든 Vue 앱은 Vue 함수로 새 인스턴스를 만드는 것부터 시작

  - Vue 인스턴스를 생성할 때 Options 객체를 전달해야 함

    - el : Vue 인스턴스에 연결할 기존 DOM element가 필요

    - data : Vue 인스턴스의 데이터 객체, Vue 인스턴스의 상태 데이터를 정의하는 곳

      ​            Vue 객체 내 다른 함수에서 this 키우더르르 통해 접근 가능

      ​            화살표 함수를 'data'에서 사용하면 안됨

      ​            화살표 함수가 부모 컨텍스트를 바인딩하기 때문에 'this'는 Vue 인스턴스를 가리키지 않는다

    - methods : Vue 인스턴스에 추가할 메서드

      ​				    화살표 함수를 메서드를 정의하는데 사용하면 안 됨 

      ​					활살표 함수가 부모 컨텍스트를 바인딩하기 때문에 'this'는 Vue 인스턴스가 아니며, 'this.a'는 정의되지 않는다

    - this keyword : Vue 함수 객체 내에서 vue 인스턴스를 가리킴

      - JS 함수는 호출될 때 this 를 암묵적으로 전달 받는다

      - JS 는 함수 호출 방식에 따라 this에 바인딩 되는 객체가 달라진다

        ```js
        함수 호출 방식 3가지
        # 함수 호출
        const app = function () {
            console.log(this)
        }
        app()
        
        # 메서드 호출
        const myObj = {
            app: app,
        }
        myObj.app()
        
        # 생성자 함수 호출
        const myapp = new app()
        ```

        

  ```js
  const app = new Vue({
  	el: '#app',        // 'el' options : Vue 인스턴스에 연결할 기존 DOM element가 필요. app 이라는 id를 가지고있는 el 연결
      data: {
          message: '',
      },
      methods: {
          greeting: function () {
              console.log('')
          },
          newFunc : () => {
              console.log(this)
          }
      }
  })
  ```



- Directive : v- 접두사가 있는 특수 속성

  - 전달인자 :   ':' 를 통해 전달인자를 받을 수 있다

    ```js
    <a v-bind:href="url">...</a>
    ```

  - 수식어 : '.' 으로 표시되는 특수 접미사

    ```js
    <form v-on:submit.prevent="onSubmit">...</form>
    ```

  - v-text

    - element 의 textContent를 업데이트
    - 내부적으로 interpolation 문법이 v-text 로 컴파일 됨

    ```html
    <div id="app">
        <p v-text="message"></p>    또는   <p>{{ message }}</p>
    </div>
    
    <script>
      const app = new Vue({
          el: '#app',
          data: {
            message: 'Hello',
          }
      })
    </script>
    ```

  - v-html

    - element 의 innerHTML을 업데이트 (XSS 공격에 취약)
    - 임의로 사용자로부터 입력 받은 내용은 v-html에 절대 사용 금지

    ```html
    <div id="app">
      <div v-html="myHtml"></div>
    </div>
    
    <script>
      const app = new Vue({
          el: '#app',
          data: {
            myHtml: <b>Hello</b>,
          }
      })
    </script>
    ```

  - v-show

    - 조건부 렌더링 중 하나
    - element는 항상 렌더링 되고 DOM에 남아있다
    - 단순 element에 display CSS 속성을 토글

    ```html
    <div id="app">
      <p v-show="isTrue">
      	true  
      </p>
      <p v-show="isFalse">
      	false
      </p>
    </div>
    
    <script>
      const app = new Vue({
          el: '#app',
          data: {
            isTrue: true,
            isFalse: false,
          }
      })
    </script>
    ```

  - v-if / v-else-if / v-else

    - 조건부 렌더링 중 하나
    - 조건에 따라 블록을 렌더링
    - 표현식이 true 일 때만 렌더링
    - element 및 포함된 directive는 토글하는 동안 삭제되고 다시 작성된다

    ```html
    <div id="app">
        <!-- 1 -->
        <div v-if="seen">
            seen이 true 일때만 렌더링
        </div>
        <!-- 2 -->
        <div v-if="myType === 'A'">
            A
        </div>
        <!-- 3 -->
        <div v-else-if="myType === 'B'">
            B
        </div v-else-if="myType === 'C'">
        <!-- 4 -->
        <div v-else>
            Not A/B/C
        </div>
    </div>
    
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            seen: false,
            myType: 'A',
          }
        })
      </script>
    ```

    |                    v-show                    |                v-if                 |
    | :------------------------------------------: | :---------------------------------: |
    |  CSS display속성을 hidden 으로 만들어 토글   |  전달인자가 false 인 경우 렌더링 x  |
    | 실제로 렌더링은 되지만 눈에서 보이지 않는 것 |   화면에서도 안보이고 렌더링도 x    |
    |    자주 변경되는 요소라면 토글 비용 적다     | 자주 변경되는 요소의 경우 비용 증가 |

  - v-for

    - 원본 데이터를 기반으로 element / 템플릿 블록을 여러 번 렌더링
    - item in items 구문 사용
    - item 위치의 변수를 각 요소에서 사용할 수 있다 -> 객체의 경우 key
    - v-for 사용 시 반드시 key 속성을 각 요소에 작성
    - v-if 와 함께 사용하면 v-for 가 우선순위가 더 높다 (권장 x)

    ```html
    <h2>Array</h2>
        <div v-for="fruit in fruits">
            {{ fruit }}
        </div>
    
        <div v-for="(fruit, index) in fruits" :key="`fruit-${index}`">
            {{ fruit }}
        </div>
    
        <div v-for="todo in todos" :key="todo.id">
            {{ todo.title }} - {{ todo.completed }}
        </div>
    <script>
        const app = new Vue({
            el: '#app',
            data : {
                myStr : 'Hello world!',
                fruits: ['apple', 'banana', 'coconut'],
                todos: [
                    { id : 1, title: 'todo1', completed: true },
                    { id : 1, title: 'todo2', completed: false },
                    { id : 1, title: 'todo3', completed: true },
                ],
            }
        })
    </script>
    
    ```

  - v-on

    - element 에 eventListener 를 연결
    - 이벤트 유형은 전달인자로 표시
    - 약어 : @    /   v-on :click -> @click

    ```html
    <div id="app">
        <!-- 메서드 핸들러 -->
        <button v-on:click="doAlert">Button</button>
        <button @click="doAlert">Button</button> <!--축약형태-->
        <!-- 기본 동작 방지 -->
        <form action="/articles/" @submit.prevent>
            <button>Submit</button>
        </form>
        <!-- 키 별칭을 이용한 키 입력 수식어 -->   
        <input @keyup.enter="onEnter">
    
        <button @click="changeMessage">button</button>
    </div>
    
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue.js',
            },
            methods: {
                doAlert: function () {
                    alert('hello!!!')
                },
                onEnter: function(event) {
                    console.log(event)
                    console.log(event.target.value)
                },
                changeMessage: function() {
                    this.message = 'bye bye'
                    // this.$data.message -> method 와 data 모두 message 를 갖고있을때 정확히 명시해줘야함
                }
            }
        })
    </script>
    ```

  - v-bind

    - HTML 요소의 속성에 Vue의 상태 데이터를 값으로 할당
    - Object 형태로 사용하면 value가 true인 key가 class 바인딩 값으로 할당
    - 약어 : ' : '  /  v-bind:href --> :href

    ```html
    <div id="app">
        <!-- 속성 바인딩 -->
        <img v-bind:src="imageSrc" alt="sample img">
        <!-- <img :src="imageSrc" alt="sample img"> v-bind 생략해도 똑같음-->
        <hr>
    
        <!-- 클래스 바인딩 -->
        <div :class="{ active: isRed }">클래스 바인딩</div>
        <h2 :class="[activeRed, myBackground]">Hello Vue !!!</h2>
        <hr>
    
        <!-- 스타일 바인딩 -->
        <ul>
            <li 
                v-for="todo in todos" 
                :class="{ active: todo.isActive }"
                :style="{ fontSize: fontSize + 'px' }"
                >
                {{ todo }}
            </li>
        </ul>
    </div>
    
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                imageSrc: 'https://picsum.photos/200/300/',
                isRed: true,
                activeRed: 'active',
                myBackground: 'my-background-color',
                todos: [
                    { id: 1, title: 'todo 1', isActive: true },
                    { id: 2, title: 'todo 2', isActive: false },
                ],
                fontSize: 30,
            }
        })
    </script>
    ```

  - v-model
  
    - HTML form 요소의 값과 data를 양방향 바인딩

- Options / Data

  - computed

    - 데이터를 기반으로 하는 계산된 속성
    - 함수의 형태로 정의하지만 함수가 아닌 함수의 반환 값이 바인딩 된다
    - 종속된 데이터가 변경될 때만 함수를 실행

    ```js
    computed: {
    	doubleNum: function () {
    		return this.num * 2
    }
    ```

  - watch

    - 데이터를 감시
    - 데이터에 변화가 일어났을 때 실행되는 함수

    ```js
    watch: {
        num: function () {
            console.log(`${this.num}이 변경되었습니다.`)
        }
    }
    ```

    |                      computed                       |                        watch                         |
    | :-------------------------------------------------: | :--------------------------------------------------: |
    | 데이터를 직접 사용/가공하여 다른 값으로 만들때 사용 | 데이터의 변화에 맞춰 다른 data가 바뀌어야 할 때 사용 |
    |               선언형 프로그래밍 방식                |                명령형 프로그래밍 방식                |
    |   특정 값이 바뀌면 해당 값을 다시 계산해서 보여줌   |           특정 값이 바뀌면 다른 작업을 함            |



- Lifecycle Hooks
  - 각 Vue 인스턴스는 생성될 대 일련의 초기화 단계를 거침
  - 이 과정에서 사용자 정의 로직을 실행할 수 있는 Lifecycle Hooks 호출
