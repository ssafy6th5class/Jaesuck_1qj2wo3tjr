# Javascript - day 1. (DOM)

### 1. DOM 조작

- DOM(Document Object Model) : 그대로 번역해보면 ''문서 객체 모델'' 이다.

  - 문서 객체 : <html> 이나 <body> 같은 html 문서의 **태그** 들을 JavaScript가 이용할 수 있는 객체로 만든 것.

  - Model : 인식하는 방법
  - DOM (문서 객체 모델) 은 문서 객체를 인식하는 방법(방식) 이라고 할 수 있다. 
  - 넓게는 웹 브라우저가 HTML 페이지를 인식하는 방식, 좁은 의미로는 document 객체와 관련된 객체의 집합을 의미.



![1](Javascript - day 1. (DOM).assets/1.png)

​																	< DOM은 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델이다. >



- DOM 조작 순서

  - 선택 (Select)

    1.  HTML 태그 이름(tag name)을 이용한 선택 -> document.getElementsByTagName(<tag name>)

       ![2](Javascript - day 1. (DOM).assets/2.JPG)

       

    2.  아이디(id)를 이용한 선택 -> getElementById(<id name>)

       ![3](Javascript - day 1. (DOM).assets/3.JPG)

       

       ##### -->> id 를 이용한 선택은 해당 id 를 가지고 있는 요소 중 첫 번째 요소 하나만 선택한다. 따라서 네 번째 아이템은 변경되지 않는다.

       

    3.  클래스(class)를 이용한 선택 -> document.getElementsByClassName(<class name>)

       ![4](Javascript - day 1. (DOM).assets/4.JPG)

       

    4.  name 속성(attribute)을 이용한 선택 -> document.getElementsByName(<name>)

       ![5](Javascript - day 1. (DOM).assets/5.JPG)

       

    5.   CSS 선택자(selector)를 이용한 선택 -> document.querySelectorAll(<selector>) : 선택자와 일치하는 여러 요소 선택, NodeList를 반환

       ![6](Javascript - day 1. (DOM).assets/6.JPG)

       (2) querySelector(<selector>) : 선택자와 일치하는 요소 1개 선택. 없다면 null을 반환

       ![7](Javascript - day 1. (DOM).assets/7.JPG)

       

  - 변경 (Manipulation)
  
    1. Document.createElement() : 작성한 태그명을 사용해 HTML 요소를 만들어 반환
    2. Element.append() : 특정 부모 노드의 자식 노드 리스트 중 마지막 자식 다음에 Node 객체나 DOMString을 삽입
    3. Node.appendChild() : 한 노드를 특정 부모 노드의 자식 노드 리스트 중 마지막 자식으로 삽입. 한 번에 하나의 노드만 추가 가능. 노드만 추가 가능.
    4. Node.innerText : Node 안의 text 값들만 가져온다
    5. Node.innerHTML : Node 안의 HTML 이나 XML 을 가져온다. 즉 최종 스타일링이 적용된 HTML 마크업을 반환
    6. ChildNode.remove() : 노드가 속한 트리에서 해당 Node를 제거
    7. Node.removeChild() : DOM 에서 자식 노드를 제거하고 제거된 노드를 반환
    8. Element.setAttribute(name, value) : 지정된 요소의 값을 변경, 속성이 존재하면 값을 업데이트 그렇지 않으면 지정된 이름과 값으로 새 속성 추가
    9. Element.getAttribute(attributeName) : 해당 요소의 지정된 값을 반환, 인자는 값을 얻고자 하는 속성의 이름
  
- DOM 선택 메서드별 반환 타입

  - 단일 element 
    - `getElementById()`
    - `querySelector()`

  - HTMLCollection (live collection)  :  name, id, 인덱스 속성으로 각 항목들에 접근 가능
    - `getElementByTagName()`
    - `getElementByClassName()`
    
  - NodeList (static collection)  :  인덱스 번호로만 각 항목들에 접근 가능
    - `querySelectorAll()`

  - HTMLCollection 과 NodeList 모두 live collection 이지만, querySelectorAll()에 의한 NodeList는 static collection 이다.

  - 배열과 같은 형태를 하고 있지만 배열이 아닌 객체를 유사배열 이라고 부르는데, NodeList 와 HTMLCollection이 이에 해당된다.

    

- Live Collection

  - 문서가 바뀔 때 실시간으로 업데이트 됨
  - DOM의 변경사항을 실시간으로  collection에 반영
  - ex) HTMLCollction, NodeList

  ![8](Javascript - day 1. (DOM).assets/8.JPG)

  - 모든 글자가 빨간색으로 바뀔 것 같지만 실제로는 1번, 3번, 5번 만 바뀌었다. 

    ClassName으로 가져와서 HTMLCollection을 반환하고, Live Collection 에 해당하기 때문에 실시간으로 변경사항이 반영된다.

    'txt-blue' 라는 클래스를 가진 요소들을 가져왔을 때 HTMLCollection의 길이는 5 이다. 그 다음 i = 0 부터 5 까지 for문을 도는데,

    첫 번째 아이템이 'txt-red'로 변경되는 순간 'txt-blue' 라는 클래스를 가지고 있지 않기 때문에 HTMLCollection 에서 삭제된다.

    삭제됨과 동시에 리스트의 length 가 5에서 4로 변경이 되고, i = 1 의 인덱스를 가지고있는 세 번째 아이템이 변경된다. 마찬가지로

    3번째 아이템이 변경되면, 다시 길이는 3으로 변경되고, i =2 의 인덱스를 가지고있는 다섯 번째 아이템이 변경이 된다.

    

- Static Collection

  - DOM이 변경되어도 collection 내용에는 영향을 주지 않는다.

  - querySelectorAll() 의 반환 NodeList만 static collection에 해당

    

- Event : 네트워크 활동이나 사용자와의 상호작용 같은 사건의 발생을 알리기 위한 객체

  - EventTarget.addEventListener() : 지정한 이벤트가 대상에 전달될 때마다 호출할 함수를 설정

  - target.addEventListner(type, listener[, options]) : type = 반응 할 이벤트 유형 , listener = 지정된 타입의 이벤트가 발생했을 때 알림받는 객체

    

- Event 취소

  - Event.preventDefault() : 현재 이벤트의 기본 동작을 중단, 태그의 기본 동작을 작동하지 않게 막음
  - 예외적으로 취소 할 수 없는 이벤트도 존재한다. ex) scroll 



