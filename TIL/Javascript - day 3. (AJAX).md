# Javascript - day 3. (AJAX)

- AJAX (Asynchronous JavaScript And XML) : 비동기식 JavaScript 와 XML

  - 자바스크립트를 이용해 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신기능
  - 서버와 통신하기 위해 XMLHttpRequest 객체를 활용
  -  XMLHttpRequest 객체를 이용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 로드할 수 있다

  

-  XMLHttpRequest 객체

  - 서버와 상호작용하기 위해 사용되며 전체 페이지의 새로고침 없이 데이터를 받아올 수 있음
  - 이름과 달리 XML뿐만 아니라 모든 종류의 데이터를 받아올 수 있음
  - 생성자 - XMLHttpRequest()

  ```js
    const request = new XMLHttpRequest()                           
    const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
  
    request.open('GET', URL)
    request.send()
  
    const todo = request.response
    console.log(`data: ${todo}`)
  ```

  -> todo 데이터가 출력이 될까??   출력 X

  -> 실행해보면 서버로부터 xhr 객체로 응답이 온다. 그런데 출력이 왜 안될까?

  -> request.send() 로 요청을 보내고, todo 에 응답을 저장한 뒤 출력되는 것이 python 식이다.

  -> JS 측면에서 보면 응답을 todo에 저장 후 기다리지 않고 바로 console.log() 로 실행을 해서 데이터가 나오지 않는다



1. 동기식

   - 순차적, 직렬적 Task 수행
   - 요청을 보낸 후 응답을 받아야 다음 동작이 이루어진다(blocking)

2. 비동기식

   - 병렬적 Task 수행
   - 요청을 보낸 후 응답을 기다리지 않고 다음 동작이 이루어짐(non-blocking)
   - JS 는 single threaded 이기 때문에 응답을 기다리지 않고 다음 동작이 진행되는 방식이다

3. single thread 

   - 컴퓨터가 여러 개의 CPU를 가지고 있어도 단일 스레드에서만 작업 수행

   - single thread 인데 동시성을 가질 수 있는 이유??

     - JS 의 런타임은 "메모리 힙" 과 "콜 스택" 으로 구성되 어있다.
     - 메모리 힙 : 메모리 할당을 담당하는 곳
     - 콜 스택 : 코드가 호출되면서 스택으로 쌓이는 곳
     - 하나의 메인스레드 (싱글 스레드) 에서 호출되는 함수들이 콜 스택에 쌓이고, LIFO 방식으로 실행이 된다.
     - JS 가 싱글 스레드 기반의 언어 이다 = 하나의 메인스레드(싱글 스레드) 와 하나의 콜 스택을 갖고 있다

     ```js
     const foo = () => {
       bar()								    (2) foo 함수 내부의 bar 함수 실행
       console.log('foo')					(4) foo 함수로 돌아와 console.log('foo') 실행 후 콜 스택에서 제거
     }
     const bar = () => {
       console.log('bar')				    (3) console.log('bar') 실행 후 콜 스택에서 제거
     }
     foo();									(1) foo 함수 실행
     console.log('foo and bar')				(5) console.log('foo and bar') 콜 스택에 추가, 실행 후 제거
     
     // 출력 결과 //
     bar
     foo
     foo and bar
     ```

4. Concurrency model : Event loop 를 기반으로 하는 동시성 모델

   - Call Stack : 코드가 호출되면서 스택으로 쌓이는 곳

   - Web API : 브라우저에서 자체 지원하는 api. Web API는 Dom 이벤트, AJAX, setTimeout 등의 비동기 작업들을 수행할 수 있도록 api 지원한다

   - Task Queue : web api 에서 비동기 작업들이 실행된 후 호출되는 콜 백 함수들이 기다리는 공간. FIFO 방식. 여러 큐로 이어져있다

   - Event Loop : 이벤트 발생 시 호출되는  콜 백 함수들을 관리하여 태스크 큐에 전달, 태스크 큐에 담겨있는 콜백 함수들을 콜스택에 넘겨준다

     ​						콜 스택에 쌓여있는 함수가 없을때만 태스크 큐에서 콜 스택으로 콜 백 함수를 넘겨준다

   - JS는 런타임 자체적으로 비동기를 지원하는 것이 아닌 web api 를 지원하여 비동기식으로 처리하는 것이다

   ```js
   console.log('첫번째로 실행됩니다.')
   setTimeout(() => console.log('최소 1초 뒤에 실행!'), 1000)
   console.log('언제 실행될까요?')
   
   // 출력 결과 //
   첫번째로 실행됩니다.
   언제 실행될까요?
   최소 1초 뒤에 실행!
       
   // 실행 시간을 0 으로 해도 결과가 같을까?? //     -> 동일
   console.log('첫번째로 실행됩니다.')
   setTimeout(() => console.log('최소 1초 뒤에 실행!'), 0)
   console.log('언제 실행될까요?')
   
   // 출력 결과 //
   첫번째로 실행됩니다.
   언제 실행될까요?
   최소 1초 뒤에 실행!
       
   // 시간을 0으로 해도 결과가 같은 이유 //
   setTimeout 함수 자체가 web api가 지원하는 비동기 함수
   즉, setTimeout의 콜백함수가 바로 call stack에 싸힝는 것이 아니라 web api에서 비동기 처리된 후 콜 백 함수가 태스크 큐로 전달된다
   시간을 아무리 0으로 해도 call stack에 바로 쌓이는 다른 함수들보다 늦게 호출된다
   ```

   1. console.log('첫번째로 실행됩니다.') 가 call stack에 쌓이고, 바로 실행되어 제거 된다
   2. setTimeout() 이 call stack에 쌓이고 setTimeout이 실행되어 Web api 에서 timer가 생성된다
   3. console.log('언제 실행될까요?') 가 call stack 에 쌓이고, 바로 실행되어 제거 된다
   4. Web api 에서 생성된 timer는 생성된 시점을 기준으로 최소 1초 후에 태스크 큐로 call back 함수를 전달한다
   5. 태스크 큐에 있던 setTimeout의 콜 백 함수가 call stack에 스택이 없는것을 확인 후 call stack에 호출되어 실행된다



- 순차적인 비동기 처리 : web api로 들어오는 순서는 중요하지 않고, 어떤 이벤트가 먼저 처리되는지가 중요
  - Async callbacks
    - 백그라운드에서 실행을 시작할 함수를 호출할 때 인자로 지정된 함수
  - promise-style
    - XMLHttpRequest 객체를 사용하는 구조보다 조금 더 현대적 버전, 새로운 코드 스타일
- Callback function 
  - 다른 함수에 인자로 전달된 함수
  - 외부 함수 내에서 호출되어 일종의 작업을 완료한다
  - 동기 / 비동기식 모두 사용됨
  - Django 의 '요청이 들어오면', evnet 의 '특정 이벤트가 발생하면' 조건으로 함수를 호출할 수 있는 이유가 Callback function 개념 때문에 가능

- Promise object

  - 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
  - 성공에 대한 약속 --> .then()
    - 이전 작업(promise) 가 성공했을 때 수행할 작업을 나타내는 callback 함수
    - 이전 작업의 성공 결과를 인자로 전달받음
  - 실패에 대한 약속 --> .catch()
    - .then() 이 하나라도 실패하면 동작
  - 무조건 실행되어야 하는 절 --> .finally(callback)

  

- Axios 

  - 브라우저를 위한 promise 기반의 클라이언트
  - 기존에 xhr 이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리했다면, 더 편리하게 도움을 준다

  ```js
  // XMLHttpRequest
  const request = new XMLHttpRequest()
  const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
  
  request.open('GET', URL)
  request.send()
  
  const todo = request.response
  console.log(todo)
  
  
  // Axios
  const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
  
  axios.get(URL)
    .then(response => {
      console.log(response.data)
    })
  ```

  



