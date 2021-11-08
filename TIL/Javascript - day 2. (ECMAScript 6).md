# Javascript - day 2. (ECMAScript 6)

- ECMA : 정보 통신에 대한  표준을 제정하는 비영리 표준화 기구
- ECMAScript : ECMA 에서 ECMA-262 규격에 따라 정의한 언어, ECMAScript 6 는 ECMA에서 제안하는 6번째 표준 명세
- 자바스크립트는 세미콜론을 선택적으로 사용 가능

---

### 1. 식별자

- 변수를 구분할 수 있는 변수명, 반드시 문자 / 달러($) / 밑줄(_) 로 시작
- 대소문자를 구분하며, 클래스명 이외에는 모두 소문자로 시작
- 예약어(ex. for, if, case ..) 사용 불가능
- 카멜 케이스 (두 번째 단어의 첫 글자부터 대문자) : 변수, 객체, 함수에 사용
- 파스칼 케이스 (모든 단어의 첫 번째 글자 대문자) : 클래스, 생성자에 사용
- 대문자 스네이크 케이스 : 상수에 사용(변경될 가능성이 없는 값)

|        let         |       const        | var(ES6 이전에 사용) |
| :----------------: | :----------------: | :------------------: |
|    재할당 가능     |   재할당 불가능    |     재할당 가능      |
| 변수 재선언 불가능 | 변수 재선언 불가능 |     재선언 가능      |
|    블록 스코프     |    블록 스코프     |     함수 스코프      |

```javascript
let foo     // 선언
foo = 10    // 할당
let bar = 0 // 선언 + 할당

let x = 1
if (x == 1) {      // if, for, 함수 등의 중괄호 내부를 블록 스코프라고 한다. 블록 바깥에서 접근 불가능
    let x = 2
    console.log(x)
}
console.log(x)

# hoisting : 변수를 선언 이전에 참조할 수 있는 현상. undefined 반환.  var 의 특성
console.log(username)
var username = '아무개'
```



### 2. 데이터 타입

- Primitive type (원시타입) : 객체가 아닌 기본 타입, 변수에 해당 타입의 값이 담김, 다른 변수에 복사할 때 실제 값이 복사됨

  - Number

  - String

  - Boolean

  - undefined : 변수에 값이 없거나, 선언 이후 값을 할당하지 않을 때 반환

  - null : 변수에 값이 없음을 의도적으로 표현할 때

  - Symbol

    |                         undefined                          |                 null                 |
    | :--------------------------------------------------------: | :----------------------------------: |
    |                        빈 값을 표현                        |             빈 값을 표현             |
    | 변수 선언 시 값을 할당하지 않으면 자바스크립트가 자동 할당 | 개발자가 의도적으로 필요한 경우 할당 |
    |                   typeof 결과 undefined                    |          typeof 결과 object          |

  

- Reference type (참조타입) : 객체 타입의 자료형, 변수에 해당 값의 참조 값이 담김, 다른 변수에 복사할 때 참조 값이 복사됨

  - Objects
    - Array
    - Function
    - ...etc



### 3. 조건문과 반복문

- if : 조건 표현식의 결과값을 Boolean 타입으로 변환 후 참/거짓 판단

- switch : 조건 표현식의 결과값이 어느 값(case)에 해당하는지 판별

  ```js
  const nation = 'Korea'
  
  switch(nation) {
      case 'Korea': {
          console.log('안녕하세요')	// break 문이 없으면 계속해서 진행된다. Fall-through 현상
      }
      case 'Japan': {
          console.log('안녕')
          break
      }
      case 'China': {
          console.log('하세요)
  }
  ```

  

- while : 조건문이 참인 동안 반복 시행

- for : 세미콜론 으로 구분되는 세 부분으로 구성. (initialization, condition, expression)

  ```js
  for(initialization; condition; expression) {
      //
  }
  ```

- for...in : 객체의 속성들을 순회할 때 사용 (객체 순회 적합)

  ```js
  const capitals = {
      Korea: '서울',
      France: '파리',
      USA: '워싱턴 D.C'
  }
  for (let capital in capitals) {
      console.log(capital) // Korea, France, USA
  }
  
  //
  const fruits = ['사과', '바나나']
  for (let fruit in fruits) {
      console.log(fruit) // 0, 1
  }
  ```

- for...of : 반복 가능한 객체를 순회하며 값을 꺼낼 때 사용 (배열 원소 순회)

  ```js
  const fruits = ['사과', '바나나']
  for (let fruit of fruits) {
      console.log(fruit) // 사과, 바나나
  }
  ```

  

### 4. 함수

- 함수 선언식: 함수의 이름과 함께 정의하는 방식. name(이름), args(매개변수), 중괄호 내부(몸통)

  ```js
  function name(args) {
      //
  }
  ```

- 함수 표현식: 함수를 표현식(어떤 하나의 값으로 결정되는 코드의 단위)내에서 정의하는 방식. 함수의 이름 생략가능(표현식에서만 가능)

  ```js
  const myFunction = function (args) {
      //
  }
  ```

- Javascript는 일급 객체에 해당

  - 변수에 할당 가능
  - 함수의 매개변수로 전달 가능
  - 함수의 반환 값으로 사용 가능

|         함수 선언식          |        함수 표현식         |
| :--------------------------: | :------------------------: |
| 익명 함수 불가능, 호이스팅 O | 익명 함수 가능, 호이스팅 X |

```js
add(2, 7)                       // 호이스팅 발생

function add(num1, num2) {
    return num1 + num2
}
//
add(2,7)                           // 표현식에서는 에러 발생, 표현식에서의 함수는 변수로 평가되어 변수의 scope 규칙을 따름
const sub = function(num1, num2) {
    return num1 - num2
}
```

- 화살표 함수(Arrow Function)
  - fucntion 키워드 생략 가능
  - 함수의 매개변수가 단 하나 뿐이면, () 도 생략가능
  - 함수의 몸통이 표현식 하나라면 {} 와 return 도 생략가능

```js
const arrow = function (name) {
    return `hello! ${name}`
}

1) function 키워드 삭제
const arrow = (name) => {
	return `hello! ${name}`
}

2) 매개변수가 하나 뿐이므로 () 도 생략
const arrow = name => {
	return `hello! ${name}`
}

3) 몸통 표현식이 하나 뿐이므로 {} 와 return 생략
const arrow = name => `hello! ${name}`
```



### 5. 배열과 객체

- 배열 : 키와 속성들을 담고 있는 참조 타입의 객체, 순서 보장

- 배열 관련 주요 메서드

  - reverse : 원본 배열의 요소들의 순서를 반대로 정렬

    ```js
    const numbers = [1, 2, 3, 4, 5]
    numbers.reverse()
    console.log(numbers) // [5, 4, 3, 2, 1]
    ```

  - push , pop : 배열의 가장 뒤에 요소 추가 , 배열의 마지막 요소 제거

    ```js
    const numbers = [1, 2, 3, 4, 5]
    
    numbers.push(100)
    console.log(numbers) // [1, 2, 3, 4, 5, 100]
    
    numbers.pop()
    console.log(numbers)  // [1, 2, 3, 4, 5]
    ```

  - unshift , shift : 배열의 가장 앞에 요소 추가 , 배열의 첫 번째 요소 제거

    ```js
    const numbers = [1, 2, 3, 4, 5]
    
    numbers.unshift(100)
    console.log(numbers) // [100, 1, 2, 3, 4, 5]
    
    numbers.shift()
    console.log(numbers)  // [1, 2, 3, 4, 5]
    ```

  - includes : 배열에 특정 값이 존재하면 참 존재하지 않으면 거짓 반환

    ```js
    const numbers = [1, 2, 3, 4, 5]
    
    console.log(numbers.includes(1)))  // true
    
    console.log(numbers.includes(100))  // false
    ```

  - indexOf : 배열에 특정 값이 존재하는지 확인 후 가장 첫 번째로 찾은 요소의 인덱스 반환. 없으면 -1 반환

    ```js
    const numbers = [1, 2, 3, 4, 5]
    let result
    
    result = numbers.indexOf(3)
    console.log(result) // 2
    
    result = numbers.indexOf(100)
    console.log(result) // -1
    ```

  - join : 배열의 모든 요소를 연결하여 반환, separator 는 선택적으로 지정가능 생략 시 쉼표 기본 값

    ```js
    const numbers = [1, 2, 3, 4, 5]
    let result
    
    result = numbers.join()
    console.log(result) // 1,2,3,4,5
    
    result = numbers.join('')
    console.log(result) // 12345
    
    result = numbers.join(' ')
    console.log(result) // 1 2 3 4 5
    ```

- 배열 관련 주요 메서드 심화 - 배열을 순회하며 특정 로직을 수행하는 메서드

  ​												 - 메서드 호출 시 인자로 callback 함수를 받는다. callback 함수 : 어떤 함수의 내부에서 실행될 목적으로 인자로 넘겨받는 함수

  - forEach : 배열의 각 요소에 대해 콜백 함수를 한 번씩 시행

    ```js
    array.forEach((element, index, array) => {   
        										 // element : 배열의 요소
        		//								 // index : 배열 요소의 인덱스
        										 // array : 배열 자체
    })
    
    const fruits = ['사과', '바나나', '수박']
    fruits.forEach((fruit, index) => {
        console.log(fruit, index)    // 사과 0 바나나 1 수박 2
    })
    ```

  - map : 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행, 콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환

    ```js
    array.map((element, index, array) => {  
        //
    })
    
    const numbers = [1, 2, 3, 4, 5]
    const doubleNums = numbers.map((num) => {    // element 이름은 내가 정하기 나름!
        return num * 2
    })
    console.log(doubleNums) // [2, 4, 6, 8, 10]
    ```

  - filter : 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행, 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열 반환

    ```js
    array.filter((element, index, array) => {  
        //
    })
    
    const numbers = [1, 2, 3, 4, 5]
    const oddNums = numbers.filter((num, index) => {   // index 안넣어줘도 상관없다.
        return num % 2
    })
    console.log(oddNums) // 1, 3, 5
    ```

  - reduce : 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행, 콜백 함수의 반환 값들을 하나의 값에 누적 후 반환

    ```js
    array.reduce((acc, element, index, array) => {  // acc : 이전 callback함수의 반환 값이 누적되는 변수
        //
    }, initialValue) 		// initialValue : 최초 callback 함수 호출 시 acc에 할당되는 값, default 값은 배열의 첫 번째 값
    
    const numbers = [1, 2, 3]
    const result = numbers.reduc((acc, num) => {
        return acc + num
    }, 0)
    console.log(result) // 6
    ```

  - find : 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행, 콜백 함수의 반환 값이 참이면 해당 요소를 반환

    ```js
    array.find((element, index, array) => {  
        //
    })
    
    const students = [
        { name: '김철수', age: 15 },
        { name: '김영희', age: 15 },
        { name: '김민수', age: 15 },
    ]
    const result = students.find((student) =>{
        return student.name === '김철수'
    })
    console.log(result) // { name: '김철수', age: 15 }
    ```

  - some : 배열의 요소 중 하나라도 주어진 판별 함수를 통과하면 참 반환, 모든 요소가 통과하지 못하면 거짓 반환

    ```js
    array.some((element, index, array) => {  
        //
    })
    const numbers = [1, 2, 3]
    const hasEvenNumber = numbers.some((number) => {
        return number % 2
    })
    console.log(hasEvenNumber) // true
    ```

  - every : 배열의 모든 요소가 주어진 판별 함수를 통과하면 참 반환, 모든 요소가 통과하지 못하면 거짓 반환

    ```js
    array.every((element, index, array) => {  
        //
    })
    const numbers = [1, 2, 3]
    const isEveryEvenNumber = numbers.every((number) => {
        return number % 2 === 0
    })
    console.log(isEveryEvenNumber) /// false
    ```

- 객체 : 속성의 집합, 중괄호 내부에 key, value 의 쌍으로 표현

  ​			key는 문자열 타입만 가능, value 는 모든 타입 가능

- 객체 - 속성명 축약 : key와 할당하는 변수의 이름이 같으면 축약 가능

  ```js
  var bookshop = {                           var bookshop = {
      books : books,               =>			   books,
      magazines : magazines,					   magazines,
  }										   }
  ```

- 객체 - 메서드명 축약 : 메서드 선언시 function 키워드 생략 가능

  ```js
  var obj = {                           var obj = {
      greetings : function () {     	 	  greetings() {
        console.log('Hi!')     =>				console.log('Hi!')
      }                       			  }
  }									   }	   
  ```

- 객체 - 계산된 속성 : 객체를 정의할 때 key의 이름을 표현식을 이용하여 동적으로 생성 가능

  ```js
  const key = 'regions'
  const value = ['서울', '부산', '광주']
  
  const korea = {
      [key]: value,
  }
  console.log(korea) // {regions: Array(3)}
  console.log(korea.regions) // ['서울', '부산', '광주']
  ```

- 객체 - 구조 분해 할당 : 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

  ```js
  const userInformation = {
      name: 'gildong hong',
      userId: 'gildong123',
      phoneNumber: '010-1111-2222',
  }
  const name = userInformation.name
  const userId = userInformation.userId
  const phoneNumber = userInformation.phoneNumber
  
  /////
  const userInformation = {
      name: 'gildong hong',
      userId: 'gildong123',
      phoneNumber: '010-1111-2222',
  }
  const { name } = userInformation
  const { userId } = userInformation     => const {name, userId, phoneNumber } =userInformation
  const { phoneNumber} = userInformation
  ```

  

