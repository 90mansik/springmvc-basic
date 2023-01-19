# 스프링 MVC - 기본 기능
### 출처 : 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술(김영한님 강의)

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
