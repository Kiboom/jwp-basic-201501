#### 1. Tomcat 서버를 시작할 때 웹 애플리케이션이 초기화하는 과정을 설명하라.
* ##### 1) ContextLoaderListener
Tomcat 서버가 시작하여 WebServerLaunch가 구동되면, ContextLoaderListener가 시행된다.
ContextLoaderListener는 ServletContextListener를 구현하는 클래스이다. 이 클래스는 웹 애플리케이션의 라이프 사이클을 감지하여 그에 따라 사용자가 정의한 코드를 실행한다. ContextLoaderListener 안에는 contextInitialized( )라는 메서드가 있다. 이 메서드는 WebServerLaunch가 웹 애플리케이션을 초기화 했을 때 이를 감지하여 사용자가 정의한 초기화 과정을 실행한다. 현재 프로젝트에서 contextInitialized에 정의한 초기화는 데이터베이스와 관련된 초기화다. 왜냐면 DAO 객체나 Connection 관련 객체들은 여러 Controller들이 공통적으로 사용하는데, 매번 인스턴스가 생성되면 많은 garbage이 발생하여 반응시간이 느려지기 때문이다. contextInitialized 메서드에서는 이러한 중복을 제거하기 위해 ResourceDatabasePopulator를 이용한다. ConnectionManager의 getDataSource를 통해 DB 연결 정보를 얻고, 이를 통해 DB와의 연결을 초기화하는 작업을 시행한다. 이렇게 초기화 작업이 완료되면 “Completed Load SerletContext!”라는 로그를 찍는다.

* ##### 2) DispatcherServlet
(ContextLoaderListener에서 DispatcherServlet으로 넘어가는 과정을 아직 잘 모르겠습니다) DispatcherServlet이 서블릿 컨테이너에 담기면 init( ) 메서드를 통해 초기화가 이뤄진다. DispatcherServlet에서 재정의된 init에서는, RequestMapping의 initMapping 메서드를 통해 서블릿과 주소를 연결하는 작업을 한다. RequestMapping 안에 있는 Map에 주소와 서블릿 객체를 짝지어 담는 것이다. 즉, @WebServlet( )의 역할을 한 군데서 처리하는 것이다. 이렇게 한 클래스 안에서 매핑을 처리하는 이유는, 다른 여러 서블릿들의 중복을 DispatcherServlet 한 군데서 도맡아 처리하기 위해서다.


#### 2. Tomcat 서버를 시작한 후 http://localhost:8080으로 접근시 호출 순서 및 흐름을 설명하라.
* ##### 1) ContextLoaderListener
Tomcat 서버가 시작하여 WebServerLaunch가 구동되면, ContextLoaderListener가 시행된다.
ContextLoaderListener는 ServletContextListener를 구현하는 클래스이다. 이 클래스는 웹 애플리케이션의 라이프 사이클을 감지하여 그에 따라 사용자가 정의한 코드를 실행한다. ContextLoaderListener 안에는 contextInitialized( )라는 메서드가 있다. 이 메서드는 WebServerLaunch가 웹 애플리케이션을 초기화 했을 때 이를 감지하여 사용자가 정의한 초기화 과정을 실행한다. 현재 프로젝트에서 contextInitialized에 정의한 초기화는 데이터베이스와 관련된 초기화다. 왜냐면 DAO 객체나 Connection 관련 객체들은 여러 Controller들이 공통적으로 사용하는데, 매번 인스턴스가 생성되면 많은 garbage이 발생하여 반응시간이 느려지기 때문이다. contextInitialized 메서드에서는 이러한 중복을 제거하기 위해 ResourceDatabasePopulator를 이용한다. ConnectionManager의 getDataSource를 통해 DB 연결 정보를 얻고, 이를 통해 DB와의 연결을 초기화하는 작업을 시행한다. 이렇게 초기화 작업이 완료되면 “Completed Load SerletContext!”라는 로그를 찍는다.

* ##### 2) DispatcherServlet
(ContextLoaderListener에서 DispatcherServlet으로 넘어가는 과정을 아직 잘 모르겠습니다) DispatcherServlet이 서블릿 컨테이너에 담기면 init( ) 메서드를 통해 초기화가 이뤄진다. DispatcherServlet에서 재정의된 init에서는, RequestMapping의 initMapping 메서드를 통해 서블릿과 주소를 연결하는 작업을 한다. RequestMapping 안에 있는 Map에 주소와 서블릿 객체를 짝지어 담는 것이다. 즉, @WebServlet( )의 역할을 한 군데서 처리하는 것이다. 이렇게 한 클래스 안에서 매핑을 처리하는 이유는, 다른 여러 서블릿들의 중복을 DispatcherServlet 한 군데서 도맡아 처리하기 위해서다.

#### 7. ListController와 ShowController가 멀티 쓰레드 상황에서 문제가 발생하는 이유에 대해 설명하라.
* 

