# 2021-01-26(SP-MVC기초1)

### 1. 아래의 xml 에 대하여 설명하시오.

```jsx
-pom.xml
-web.xml
-context.xml
```

> pom.xml

- 프로젝트의 중요한 정보를 정의하고 정리하는 파일이다.
- 대부분 Maven의 중요 설정파일들이 들어가있고 라이브러리를 추가함으로써, 불러오는 기능 '의존성'이 있으며, 실제 라이브러리 파일이 저장되어 있는 저장소 서버의 위치를 지정하며, 사용할 라이브러리를 지정한다.

**중요★**

 1. dependency

 : "의존성"이라 하며, 두 가지 서로 다른 모듈간의 연결을 할 수있으며 클래스를 재사용할 수 있고 로직이 아닌 클래스를 생성하기에 있어서 아주 효율적인 태그다.

 2. properties

: "JavaBean"기술에 의해서 관리되는 데이터이다. 즉, 객체에 속한 데이터를 정의해놓은 것이다.

 3. groupId

: 프로젝트를 구분하는 값으로써, 도메인명을 사용한다.

 4. artifactld

: groupId와 마찬가지로 프로젝트를 구분하는 값이고 다른점은 프로젝트명을 사용한다.

> web.xml

- Web Application의 Deployment Descriptor(환경파일 : 배포서술자)로서 xml형식의 파일
- 어플리케이션 클래스,리소스,구성을 기술하고 웹 서버가 이를 사용해서 웹 요청을 처리하는 방법을 기술한다.
- 웹 서버가 어플리케이션에 대한 요청을 수신하면 배포 설명자를 사용해서 해당 요청을 처리해야 하는 코드로  요청의 URL을 맵핑한다.

**중요★**

 1. DispatcherServlet

: Spring Container(Controller의 생명주기 관리)를 생성한다, 클라이언트 요청을 처음으로 받는 클래스이며, 요청을 Handle(Controller)r에 보낸다.

 2. ContextLoaderListener

: Controller가 공유하는 Bean(Dao,DataSource,Service)들을 포함하는 Spring Container를 생성한다.

> context.xml

- 스프링에서 말하는 Context는 Application Context를 확장한 WebApplication Context 인터페이스의 구현체를 말하며, Application Context상속받아서 사용한다.

중요★

 1. Root WebApplicationContext

: ContextLoaderListener에 의해 생성되며, 여러 Servlet Context를 서로 공유해야하는 빈들을 등록하고 설정할 때 사용

2. Servlet WebApplicationContext\

: DispatcherServlet에 의해 생성되며, 해당 Servlet Context에서만 사용가능하고 DispatcherServlet에서 관리하는 독립적인 웹 어플리케이션 형태로 사용한다,

---

### 2. 스프링에서 게시판 사용을 위한 설계도를 그리시오.

![Untitled](https://user-images.githubusercontent.com/75012998/105828120-1c8cec80-6006-11eb-8134-ed59d626cbec.png)

---

### 3. 스프링에서 한글처리는 어떻게 하는가?

- 해당 프로젝트의 web.xml파일을 열어 아래 태그를 써준다.

```jsx
<!-- 한글처리 - filter태그 안에 선언해야한다. -->
	<filter>
		<filter-name>encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>

	<filter-mapping>
		<filter-name>encoding</filter-name>
		<servlet-name>appServlet</servlet-name>
	</filter-mapping>
```

---

### 4. 마이바티스에 대하여 설명하시오.

> MyBatis

- 객체 지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 좀 더 쉽게 할 수 있게 도와주는 개발 프레임워크이다.
- 프로그램에 있는 SQL쿼리들을 한 구성파일에 구성하여 프로그램 코드와 SQL을 분리할 수 있는 장점을 가지고있다.

**특징**

1. 복잡한 쿼리나 다이나믹한 쿼리에 강하다.
2. 프로그램 코드와 SQL 쿼리의 분리로 코드의 간결성 및 유지 보수성 향상

---

### 5. 스프링에서 hello.jsp가 유저에게 전달되기까지의 순서와 해당 객체를 그림으로 표현하시오.

![Untitled 1](https://user-images.githubusercontent.com/75012998/105828124-1dbe1980-6006-11eb-88fb-c8f61915fb32.png)
