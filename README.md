# Spring_Basic

1. 환경설정
<p>1-1. spring boot 2.7.6버전 사용 (3버전 이후는 java 17버전 사용해야함)</p>
<p>1-2. java 11 버전사용</p>
<p>1-3. gradle 사용 -> 현직에서 메이븐보다 더 많이 사용</p>
<p>1-3-1.intellij에서 build.gradle 실행하여 프로젝트 생성</p>
<p>1-3-2.setting에서 gradle설정을 gradle -> intellij로 변경해 줘야 실행속도가 더 빠름</p>
<p>1-4. 스프링 부트 라이브러리</p>
<p>1-4-1. 톰캣, 스프링MVC, 타임리프 템플릿 엔진, 스프링부트 + 스프링 코어 + 로깅 포함</p>
<p>1-5. 테스트 라이브러리</p>
<p>1-5-1. junit(test framework), mockito(목 라이브러리) assertj(테스트 코드를 편하게 작성 라이브러리), spring-test(스프링 통합 테스트 지원)</p>
<br>
2. 정적 콘텐츠
resources/static 폴더 안에 html 파일을 넣으면 정적 페이지 출력
웹 브라우저에서 http://localhost:9090/hello-static.html 를 넣으면 톰캣 서버에서 스프링으로 요청을 넘김, 컨트롤러안에서 hello-static이 있는지 찾고 없으면 resources의 static(정적)폴더 내부에 hello-static이 있는지 찾고 웹 브라우저로 응답
- controller에 hello-static.html mapping이 있으면 해당 요청을 우선적으로 처리하여 resources 내부 파일은 응답되지 않음
<br>
3. MVC와 템플릿 엔진
MVC : model, view, controller
과거에는 jsp에 모조리 때려박는 방식으로 controller와 view를 같이 쓰는 MVC1방식 사용
강사님은 수천줄의 jsp에 view controller를 몽땅 때려넣은 코드를 본적이 있다! 담당자만 유지보수 가능했다던!
thymeleaf 템플릿 장점 : vscode같이 html을 그냥 쓰고 서버없이 열어서 껍데기 볼 수 있음
html주소로 연결하면 p태그 문자가 출력되고 서버연결후 요청하면 p태그 안 th:text= 안의 값이 출력됨
웹 브라우저에서 http://localhost:9090/hello-static.html 를 넣으면 톰캣 서버에서 스프링으로 요청을 넘김, 컨트롤러안에서 hello-static이 있는지 찾고 mapping되어 있으면 return: hello-static / model(name:spring) 리턴값과 모델의 키와벨류값을 스프링으로 넘겨줌.
스프링은 viewResolver가 동작하여 return값과 같은 이름을 찾아서 thymeleaf 템플릿 엔진에 넣으면 렌더링을 해서 변환이 된 html을 브라우저에 반환함
<br>
4. API 방식
@ResponseBody ->응답의 바디에 데이터를 직접 넣어주겠다
default json방식으로 데이터 전달
xml로도 출력하도록 할 수도 있음
getter,setter방식은 java been 규약이라고도 하고 property 접근방식 이라고함
웹 브라우저에서 http://localhost:9090/hello-api.html 를 넣으면 톰캣 서버에서 스프링으로 요청을 넘김, 컨트롤러안에서 hello-api가 있는지 찾고 @responsbody 어노테이션이 붙어있으면 Http Body에 문자내용을 직접 반환
httpMessageConverter가 동작을함. 단순 문자면 StringConverter가 동작하고 객체라면 jasonConverter가 동작하여 json방식으로 변환하여 응답
스프링은 default jackson 라이브러리를 사용하여 변환함
gson도 사용 가능
Http Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합하여 HttpMessageConverter가 선택됨
<br>
5. 회원 관리 예제 - 비즈니스 요구사항 정리
컨트롤러 : 웹 MVC의 컨트롤러 역할
서비스 : 핵심 비즈니스 로직 구현
리포지토리 : 데이터 베이스에 접근, 도메인 객체를 DB에 저장하고 관리
도메인 : 비즈니스 도메인 객체/ DB에 저장되고 관리될 정보들

인터페이스에 구현 클래스 설계
저장소 사용 보류
구현체로 메모리 기반 저장소 사용
<br>
6.Junit 테스트 케이스 작성
main 메서드를 통해 실행하거나 웹 컨트롤러를 통해 실행하는건 실행하는데 오래걸리고 반복실행하기 어려운 문제가 있다. 자바는 Junit 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.
import static 에 클래스명.* 임포트 해주면 해당 클래스를 기입하지 않고도 메소드를 사용할 수 있다.
자바 static import는 무엇인가
Test 클래스는 패키지를 묶어서 한번에 실행 가능
모든 테스트는 순서가 보장 되지 않는다. 그러므로 서로 의존관계가 없도록 테스트가 끝날때마다 객체나 공용 데이터를 비워주는 코드를 작성해 줘야 함 @AfterEach 어노테이션으로 매 테스트 실행마다 실행될 코드를 작성 가능
TDD -> 테스트 주도 개발 : 테스트 클래스를 먼저 작성하고 기능을 구현하는것. 틀을 먼저 만들고 구현 클래스를 만들어 돌려 보는것
TDD(Test-Driven-Development) 방법론에 대하여…
<br>
7.회원서비스 개발
서비스클래스는 비즈니스 의존적으로 네이밍 설계 레포지토리는 좀더 개발관점으로 네이밍
optional로 감싸면 optional 안에 멤버객체가 있는 것 null이 있는 자료의 경우 많이 사용
null이 아니고 값이 있으면 동작하도록 ifPresent()를 작성하여 에러시 throw로 에러를 던지도록 작성
intellij 단축키 팁
컨트롤 알트 v 하면 자동으로 속성과 변수 작성
컨트롤 알트 쉬프트 t하면 refactor 자동완성 메뉴 나옴 extract method선택하면 메소드로 생성해줌 -> 기능이 있는 코드를 메소드로 분리해 낼때 사용
컨트롤 알트 t -> 테스트케이스 자동 완성
<br>
8.회원 서비스 테스트
테스트 클래스는 한글로 메소드를 적어 직관적으로 관리해도 좋음
테스트 설계 팁
given 무언가 주어졌을때
when 이걸 실행했을때
then 이런결과가 나와야해
try catch 대신 assertThrows()로 예외처리
<br>
9.스프링 빈을 등록하는 방법
컴포넌트 스캔과 자동 의존관계 설정
컴포넌트 스캔이 @Component라는 어노테이션을 찾아 해당 클래스의 인스턴스를 만들어 bean으로 등록
@controller, @service, @repository 모두 컴포넌트를 사용한 어노테이션으로 스프링 빈으로 자동 등록된다
아무데나 @Component가 있어도 되나? 기본적으로 @SpringBootAnnotation 클래스와 동일하거나 하위패키지를 스캔하여 찾아 들어감
생성자에 @Autowired를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입 생성자가 1개만 있으면 생략가능
스프링은 스프링 컨테이너에 스프링 빈을 등록할 때 기본적으로 싱글톤으로 등록함(유일하게 하나) 따라서 같은 스프링 빈이면 모두 같은 인스턴스임. 특별한 경우가 아니라면 싱글톤을 사용

자바 코드로 직접 스프링 빈 등록하기
클래스에 @Configuration을 사용하고 @Bean 어노테이션으로 작성하여 빈에 직접 등록할 수 있음.
xml방식도 있지만 최근에는 사용하지 않는다
DI 방식
생성자주입 -> 의존관계가 실행중에 동적으로 변하는 경우(runtime 이후 변경되는 경우)는 거의 없으므로 생성자 주입을 권장.
필드주입 -> 필드주입의 경우 생성된 객체를 바꿀수 있는방법이 없으므로 추천되지 않는 방법
setter주입 -> 객체를 생성했을때 set메소드 자체가 퍼블릭하게 노출이되어있기 때문에 값이 변경될 수 있음 한번세팅이 되고나면 바꿀 일이 잘 없음
정형화 된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔 사용한다.
정형화 되지 않거나 상황에 따라 구현 클래스를 변경해야하면 설정파일의 수정을 통해 스프링빈으로 등록한다. 직접 스프링빈을 등록하는 경우 이렇게 설정만 조금 수정해주면 변경을 쉽게 할 수 있다.
ex) 가상메모리 상태인 Repository를 실제 db에 연결하는 경우
<br>
10.스프링부트에서 그래들을 사용하여 순수 JDBC DRIVER연동
build.gradle에 설정정보(의존성)을 주입해 준다.
implementation 'org.springframework.boot:spring-boot-starter-jdbc'/*
runtimeOnly 'com.h2database:h2'
resources/templates/application.properties에 dbms url,name,id,password등의 정보를 입력해준다
h2 드라이버는 id,pw를 입력하지 않아도 된다.
레포지토리에 DataSource(접속정보)를 생성해줌
트랜잭션이 걸리면 같은 커넥션을 유지해줘야하기 때문에 DataSourceUtils를 통해서 커넥션을 획득해야함
닫을때도 데이터소스 유틸즈를 통해서 릴리즈 해줘야함
sql문 작성하고 connection연결
사용한 커넥션 close
H2 DBMS는 가볍기때문에 테스트용 로컬 DB로 사용하기 좋다
h2 드라이버 구동 방법
cmd 창을 열고 h2/bin 경로로 진입하여 h2.bat 실행
jdbc url을 jdbc:h2:tcp://localhost/~/test로 수정
application.properties에서 spring.datasource.username=sa 주입
주입하지않으면 스프링부트 2.4 버전 이후 name오류 발생 주의
cmd창 종료하면 자동으로 DBMS연결 종료
<br>
11.스프링의 장점
스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다.
기능을 완전히 변경해도 조립하는코드만 수정하고 실제 어플리케이션 수행에 필요한 코드를 하나도 변경하지 않고도 변경가능하다. 이게 OCP 개방폐쇄원칙을 지킨것
기능확장에는 열려있고, 실제코드의 수정이나 변경에는 닫혀있다.
<br>
12.스프링 통합 테스트
@SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행한다.
@Transactional : 테스트 케이스에 이 어노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지않는다
test에 @Commit 어노테이션 사용하면 커밋 해 버림
순수히게 단위단위 자른 테스트가 좋은 테스트일 확률이 높다. spring으로 서버를 올려서 테스트하는것 보다 스프링의 도움없이 순수한 테스트를 작성하여 잘 돌아가도록 설계하는것이 좋을 수 있다.
<br>
13.스프링 JdbcTemplate
JdbcTemplate을 사용하면 jdbc 코드를 현저히 줄일 수 있음
Template Method 디자인 패턴으로 코드를 줄인 라이브러리
스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해 줘야 함
<br>
14.JPA!!
jpa는 기존 반복코드는 물론이고 기본적인 sql도 jpa가 직접 만들어 실행해줌
sql과 객체중심 설계에서 객체중심 설계로 패러다임 전환을 할 수 있다.
개발 생산성을 높일 수 있음
jpa는 표준 인터페이스고 구현은 각 기업에서함
주로 hibernate를 사용
orm기술임 object와 relational(관계형)데이터베이스테이블을 mapping한다
application.properties 설정
show-sql : JPA가 생성하는 SQL을 출력한다.
ddl-auto : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none 를 사용하면 해당 기능을 끈다.
create 를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다.
JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.
<br>
15.스프링 데이터 JPA
스프링 부트와 jpa만 사용하더라도 개발 생산성이 굉장히 증가
스프링 데이터 JPA를 사용하면 리포지토리에 구현 클래스 없이 인터페이스 만으로 개발을 완료할 수 있음
기본 crud기능도 스프링 데이터 jpa가 모두 제공함
주의! 스프링 데이터 jpa는 jpa를 편하게 사용하도록 도와주는 프레임워크임 jpa가 선행되어있지 않을 경우 개발중 발생하는 문제들을 해결할 수 없어짐
스프링 데이터 JPA가(JpaRepository 상속) SpringDataJpaMemberRepository를 스프링 빈으로 자동 등록해줌
proxy기술로 등록

기본적인 crud 기능이 모두 JpaRepository에 만들어져 있음
findByName() , findByEmail() 처럼 메서드 이름 만으로 조회 기능 제공
페이징 기능 자동 제공
<br>
16.AOP
AOP는 공통적으로 사용해야하는 공통 관심 사항 코드가 많을때 묶어서 사용하기위해 사용한다.
공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리
공통 관심 사항 : 인프라 로직(실행 시간을 측정한다던지 등등)
핵심 관심 사항 : 비즈니스 로직(주 업무로직; 회원가입 조회 등등)

핵심관심사항을 깔끔하게 유지할 수 있음
원하는 적용 대상을 선택할 수 있음
변경이 필요하면 aop로직만 변경하면된다
실행과정

aop적용 전에는 컨트롤러에서 서비스를 의존하고있으니 서비스의 메소드를 호출함

aop를 적용하면 스프링컨테이너는 빈을 등록할 때 프록시를 이용해 가짜 memberService를 먼저 만들어 호출하게됨
가짜스프링빈이 끝나면 joinpoint.proceed()를 통해 진짜 메소드가 호출됨
joinPoint : pointcut의 후보가 되는 모든 메소드 Aspect적용이 가능한 모든 지점
Pointcut : 횡단관심사를 적용할 타깃 메서드를 선택하는 지시자, 타깃 클래스의 타깃 메서드 지정자 Joinpoint의 부분집합이다.
아래 코드를 사용하여 proxy 주입여부를 확인할 수 있다.

실행결과 -> memberService = class hello.hellospring.service.MemberServiceEnhancerBySpringCGLIBbd0aa3fe
서비스 뒤에 붙는게 복제를 하여 코드를 조작하는 기술

aop를 적용 후에는 그림과 같이 proxy가 주입되어 실행됨

![image](https://github.com/SungJunP/Spring_Basic/assets/149445382/d2c891da-8a3a-4c5c-8e7c-d48d33efde2e)

![image](https://github.com/SungJunP/Spring_Basic/assets/149445382/8428f643-f7b0-463d-8ae1-0ab73f3fbb0e)

![image](https://github.com/SungJunP/Spring_Basic/assets/149445382/b6c66a08-112f-40dd-9e5e-b2f3e1ada901)


출처 : 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술
