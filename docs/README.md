# 스프링 MVC - 기본 기능
### 출처 : 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술(김영한님 강현)

## 1.로깅

### 로깅 라이브러리
- 스프링 부트 라이브러리를 사용 하면 스프링 부트 로깅 라이브러리(spring-boot-starter-logging)가 함께 포함된다.
- 스프링 부트 로깅 라이브러리는 기본으로 다음 로깅 라이브러리를 사용한다.
  - SLF4J - http://www.slf4j.org
  - Logback - http://logback.qos.ch
  - SLF4J는 인터페이스고, 구현체로 Logback 같은 로그 라이브러리를 선택하면 된다.

### 로그 레벨 설정
- LEVEL : TRACE > DEBUG > INFO > WARN > ERROR
- 개발 서버는 debug 출력, 운영 서버는 info 출력 처럼 환경에 맞게 선언 가능
  - 로그레벨 설정 방법
    - application.properties
    ```
    #전체 로그 레벨 설정(기본 info)
    logging.level.root=info
    
    #hello.springmvc 패키지와 그 하위 로그 레벨 설정
    logging.level.hello.springmvc.debug
    ```
### 올바른 로그 사용법
- log.debug("data="+data)
  - 로그 출력 레벨을 info로 설정해도 해당 코드에 있는 "data="+data가 실제 실행이 되어 버린다.
- log.debug("data={}", data)
  - 로그 출력 레벨은 info로 설정하면 아무일도 발생하지 않는다. 따라서 앞과 같은 의미없는 연산이 발생하지 않는다.

### 로그 사용시 장점
- 쓰레드 정보, 클래스 이름 같은 부가 정보를 함께 볼 수 있고, 출력 모양을 조정할 수 있다.
- 로그 레벨에 따라 개발 서버에서는 모든 로그를 출력하고, 운영서버에서는 출력하지 않는 등 로그를 상황에 맞게 조절 할 수 있다.
- 시스템 아웃 콘솔에만 출력하는 것이 아니라, 파일이나 네트워크 등, 로그를 변도의 위치에 남길 수 있다.
- 성능이 System.out보다 좋다.


## 2.요청매핑

### @RestController
- @Controller는 반환 값이 String 이면 뷰 이름으로 인식 하기 때문에 뷰를 찾고 뷰가 랜더링 된다.
  @RestController 는 반환 값으로 뷰를 찾는 것 아닌, HTTP 메시지 바디에 바로 입력한다.

### Url Mapping 변경 사항 (스프링 부트 3.0 이전/이후)
- 스프링 부트 3.0 이전
  - 다음 두가지 요청은 다른 URL이지만, 스프링은 다음 URL 요청들을 같은 요청으로 매핑한다. 
  - 매핑: /hello-basic
  - URL 요청: /hello-basic , /hello-basic/
- 스프링 부트 3.0 이후
  - /hello-basic , /hello-basic/ 는 서로 다른 URL 요청을 사용해야 한다.
  - 기존에는 마지막에 있는 / (slash)를 제거했지만, 스프링 부트 3.0 부터는 마지막의 / (slash)를 유지한다. > 따라서 다음과 같이 다르게 매핑해서 사용해야 한다.
  - 매핑: /hello-basic URL 요청: /hello-basic
  - 매핑: /hello-basic/ URL 요청: /hello-basic/

### HTTP 매핑

#### @RequestMapping
- method 형식을 지정하지 않으면 HTTP 메서드와 무관하게 호출된다.
- 모두 허용 GET, HEAD, POST, PUT, PATCH, DELETE

#### HTTP 메서드 축약 사용
- 축약된 http 메서드를 사용할 수 있다. 실제로 메서드를 타고 들어가면 @ReqeustMapping(method = {})로 지정되어 있다.
- @GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping


#### PathVariable(경로 변수) 사용
- Ex) @PathVariable("userId") String data 
- @RequestMapping 은 URL 경로를 템플릿화 할 수 있는데, @PathVariable 을 사용하면 매칭 되는 부분을 편리하게 조회할 수 있다.
- @PathVariable 의 이름과 파라미터 이름이 같으면 생략할 수 있다.

#### 미디어 타입 조건 매핑
- consumes 타입 설정
  - Ex) PostMapping(value = "/mapping-consume", consumes = "application/json")
  - HTTP 요청의 Content-Type 헤더를 기반으로 미디어 타입으로 매핑한다.
    만약 맞지 않으면 HTTP 415 상태코드(Unsupported Media Type)을 반환한다.
- produces 타입 설정
  - Ex)   @PostMapping(value = "/mapping-produce", produces = "text/html") 
  - HTTP 요청의 Accept 헤더를 기반으로 미디어 타입으로 매핑한다. 
   만약 맞지 않으면 HTTP 406 상태코드(Not Acceptable)을 반환한다.
