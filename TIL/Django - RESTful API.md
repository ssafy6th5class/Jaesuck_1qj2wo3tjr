## Django - RESTful API

- HTTP ( Hyper Text Transfer Protocol) : 웹 상에서 컨텐츠를 전송하기 위한 약속

  - 웹에서 이루어지는 모든 데이터 교환의 기초
    - 요청(response) : 클라이언트에 의해 전송되는 메시지
    - 응답(request) : 서버에서 응답으로 전송되는 메시지

  - HTTP 요청의 대상을 리소서(resource) 라고 한다. 각 리소스는 리소스 식별을 위해  HTTP 전체에서 사용되는 URI 로 실별됨

- URI

  - 통합 자원 식별자
  - 인터넷의 자원을 식별하는 유일한 주소
  - 하위 개념으로 URL, URN 이 있다 하지만 URN을  사용하는 비중이 매우 적어 URL을 URI 와같은 의미로 사용한다
    - URL 
      - 통합 자원 위치
      - 네트워크 상에 자원이 어디 있는지 알려주기 위한 약속
      - '웹 주소', '링크' 라고 불림
    - URN
      - 통합 자원 이름
      - URL 과 달리 자원의 위치에 영향을 받지않는 유일한 이름 역할
  - URI의 구조
    - Scheme(protocol) : 브라우저가 사용해야 하는 프로토콜  ex) https / data/ file / ftp /malito
    - Host(Domain name) : 요청을 받는 웹 서버의 이름 ex) www.example.com
    - Port : 웹 서버 상의 리소스에 접근하는데 사용되는 기술적인 '문' ex) HTTP 80 / HTTPS 443
    - Path : 웹 서버 상의 리소스 경로 ex) path/to/myfile.html
    - Query(Identifier) : 웹 서버에 제공되는 추가적인 매개 변수 ex) ?key=value
    - Fragment : 브라우저에게 해당 문서의 특정 부분을 보여주기 위한 방법 ex) #quick-start
    - https:www.example.com:80/path/to/myfile.html/?key=value#quick-start

- API (Application Programming Interface)

  - 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
  - Web API : 웹 애플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세
  - 응답 데이터 타입 : HTML / XML / JSON

- REST : 자원을 정의하는 방법에 대한 고민, REST 원리를 타르는 시스템을 RESTful 용어로 지칭한다

  - REST의 자원과 주소의 지정 방법
    - 자원 - URI
    - 행위 - HTTP Method
    - 표현 - 자원과 행위를 통해 궁극적으로 표현된느 결과물, JSON으로 표현된 데이터를 제공
    - JSON : JavaScript 표기법을 따른 단순 문자열
  - REST의 핵심 규칙
    - 정보는 URI 로 표현
    - 자원에 대한 행위는 HTTP Method 로 표현 (GET, POST, PUT, DELETE)
    - 지키지 않더라도 동작에 영향을 미치지는 않음

- RESTful API 

  - REST 원리를 따라 설계한 API
  - 프로그래밍을 통해 클라이언트의 요청에 JSON을 응답하는 서버를 구성

  

- JsonResponse

  - Json response를 만드는 HttpResponse의 서브 클래스
  - "safe" parameter
    - True (기본값)
    - dict 이외의 객체를 직렬화 하려면 False로 설정해야함

  ```python
  response = JsonResponse({'foo': 'bar'}) // dict 이므로 safe 생략
  response = JsonResponse([1, 2, 3], safe=False) // list 이므로 safe=False
  ```

- Serialization (직렬화)

  - 데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고, 나중에 재구성할 수 있는 포맷으로 변환하는 과정
  - serializers in Django : Queryset / Model Instance와 같은 복잡한 데이터를 JSON / XML 유형으로 쉽게 변환 할 수 있는 python 데이터 타입으로 만든다

- DRF (Django REST Framework)

  ```js
  from rest_fram_work import serializers
  from .models import Article
  
  class ArticleSerializer(serializers.ModelSerializer):  // Article 모델에 맞춰 자동으로 필드를 생성해 serializer 해주는 ModelSerializer
  
  	class Meta:
      	model = Article
  		fields = '__all__'
  ```

  |          |  Django   |     DRF     |
  | :------: | :-------: | :---------: |
  | Response |   HTML    |    JSON     |
  |  Model   | ModelForm | serializers |

  - ModelSerializer : 모델 정보에 맞춰 자동으로 필드 생성, serializer에 대한 유효성 검사기를 자동으로 생성

- api_view decorator

  - 기본적으로 GET 메서드만 허용되며 다른 요청에 대해서는 405 Method Not Allowed 로 응답
  - DRF에서는 선택이 아닌 필수적으로 작성해야 한다

- raise_exception

  - is_valid()는 유효성 검사 오류가 있는 경우 serializers.ValidationError 예외를 발생시키는 선택적 인자를 사용 할 수 있다

  ```python
  @api_view(['GET', 'POST'])
  def article_list(request):
      if request.method == 'GET':
          articles = get_list_or_404(Article)
          serializer = ArticleListSerializer(articles, many=True)
          return Response(serializer.data)
      elif request.method == 'POST':
          serializer = AritlceSerializer(data=request.data)
          if serializer.is_valid(raise_exception=True):
              serializer.save()
              return Response(serializer.data, status=status.HTTP_201_CREATED)
          // return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)  -> raise_exception 으로 자동 에러 처리를해서 생략가능
  ```

  

- Read Only Field

  - 어떤 게시글에 작성하는 댓글인지에 대한 정보를 form-data 로 넘겨주지 않아 직렬화 하는 과정에서 article 필드가

    유효성 검사를 통과하지 못한다

  - 이때 읽기 전용 필드 설정을 통해 직렬화하지 않고, 반환 값에만 해당 필드가 포함되도록 설정 할 수 있다

  ```python
  from rest_fram_work import serializers
  from .models import Comment
  
  class CommentSerializer(serializers.ModelSerializer):
  
  	class Meta:
      	model = Comment
  		fields = '__all__'
          read_only_fields = ('article', )
  ```

- 특정 게시글에 작성된 댓글 목록 출력 -> 역참조

  ```python
  // PrimaryKeyRelatedField
  class ArticleSerializer(serializers.ModelSerializer):
  	comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
      	
  	class Meta:
      	model = Article
  		fields = '__all__'
          
  ///
  pk 를 사용하여 관계된 대상을 나타내는데 사용 가능
  필드가 1:N / M:N 관계일 때 many=True 파라미터 필요
  역참조하는 comment_set 을 form-data로 받지 않으므로 read_only=True 파라미터 필요
  
  
  // Nested relationshops
  class CommentSerializer(serializers.ModelSerializer):
      	
  	class Meta:
      	model = Comment
  		fields = '__all__'
          read_only_fields = ('article', )
          
  class ArticleSerializer(serializers.ModelSerializer):
  	comment_set = CommentSerializer(many=True, read_only=True)
      	
  	class Meta:
      	model = Article
  		fields = '__all__'
          
  ///
  모델 관계상으로 참조된 대상(comment) 는 참조하는 대상(Article)의 응답에 포함되거나 중첩될 수 있음
  ```

  - 특정 필드를 override 혹은 추가한 경우 read_only_fields shortcut 사용 불가능!