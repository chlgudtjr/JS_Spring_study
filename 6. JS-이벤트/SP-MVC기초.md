### 1. Command 객체에 대하여 설명하시오.

> Command

- HttpServletRequest를 통해 들어온 요청 파라미터들을 setter메소드를 이용하여 객체에 정의 되어있는 속성에 바인딩 되는 객체
- getter와 setter가 필수적으로 있어야한다.

**역할**

 1. 컨트롤러에서 View로 바인딩 : View단에서 form태그를 사용

 2. View에서 컨트롤러로 바인딩 : View단에서 input type="text"혹은 input type="hidden"으로 값을 전송

---

### 2. ModelAndView 객체에 대하여 설명하시오.

> ModelAndView

- 데이터와 view페이지로 넘길 값을 모두 가지고 있는 객체이다.

**사용법**

```java
@RequestMapping("/board/modelAndView")
	public ModelAndView contentview() { // ModelAndView 객체생성
		
		ModelAndView mv = new ModelAndView(); // ModelAndView 객체생성
		mv.addObject("id", "hello");//ModelAndView객체에 데이터를 담음
		mv.setViewName("/content/contentView"); // view 이름 설정

		return mv;
	}

// jsp로 Data : ${객체.data}로 값을 받아오면된다.
```

---

### 3. 아래 골뱅이에 대하여 설명하시오.

```jsx
@Controller
@RequestParam
@RequestMapping
```

> @Controller

- 주로 View를 반환하기 위해 사용한다.
- 클라이언트가 URI를 요청하면 DispatcherServlet이 요청을 인터셉트한다. 그리고 Controller가 요청을 처리한 후에 응답을 DispatcherServlet으로 반환하고 DispatcherServlet은 View를 사용자에게 반환하는 역할

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca945b2c-10f0-4e45-9707-9a4cff11b21d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca945b2c-10f0-4e45-9707-9a4cff11b21d/Untitled.png)

> @RequestParam

- 단일 파라미터를 전달받을때 사용하는 어노테이션입니다.

**사용법**

```java
**@RequestMapping("board/checkId")
	public String checkId(@RequestParam("id") String id,@RequestParam("pw") int pw, Model model) {
		model.addAttribute("identify", id);
		model.addAttribute("password", pw);

		return "board/checkId";
	}**
```

> @RequestMapping

- 클라이언트가 보내는 request요청에 담겨진 key=value구조에서 특정한 key에 해당하는 value를 바로 가져오기 위한 용도
- Servlet에서 request.getParameter()와 같은 기능

**사용법**

```java
@RequestMapping(value = "/board/content_view", method = RequestMethod.GET)
	public String content_view(Model model) { // 값을 저장할 객체만들기
		logger.info("content_view() 실행");
		
		model.addAttribute("id", 30); // key ,value값 지정
		return "content_view";
	}
```

---

### 4. 아래를 프로그래밍 스프링으로 프로그래밍 하시오.

```jsx
-국영수 입력 페이지
-국영수점수를 서버에서 받아서,총점과 평균을 리턴
-해당 총점과 평균의 점수의 색깔은 각각 빨간색과 파란색으로 만들것(JS로).
```

- 점수입력 소스코드

    ```html
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <%@ page session="false" %>
    <html>
    <head>
    	<title>Home</title>
    </head>
    <body>
    <h1>
    	점수 입력페이지입니다.
    </h1>
    	<form action="print">
    		국어 : <input type="number" name="kor"><br/>
    		영어 : <input type="number" name="eng"><br/>
    		수학 : <input type="number" name="math"><br/>
    		<input type="submit" value="전송">
    		<input type="reset" value="리셋">
    	</form>
    </body>
    </html>
    ```

- 점수출력 소스코드

    ```html
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <%@ page session="false" %>
    <html>
    <head>
    	<title>Home</title>
    	<script>
    		window.onload = function(){
    			var tdNode = document.getElementById("t1");
    			tdNode.style.fontWeight = "bold";
    			tdNode.style.color = "red";
    			
    			var tdNode1 = document.getElementById("t2");
    			tdNode1.style.fontWeight = "bold";
    			tdNode1.style.color = "blue";
    		}
    	</script>
    	
    </head>
    <body>
    <h1>
    	점수 출력페이지입니다.
    </h1>
    	<table border="1">
    	<tr>
    		<td colspan="2">국어 : ${score.kor }</td>
    	</tr>
    	<tr>	
    		<td colspan="2">영어 : ${score.eng }</td>
    	</tr>
    		<td colspan="2">수학 : ${score.math }</td>
    	</tr>
    	<tr>
    		<td id="t1">총점 : ${score.kor + score.eng + score.math }</td>
    		<td id="t2">평균 : ${score.kor + score.eng + score.math / 3.0}</td>
    	</tr>
    	</table>

    </body>
    </html>
    ```

- DataCommand 소스코드

    ```java
    package edu.bit.ex;

    import java.text.DateFormat;
    import java.util.Date;
    import java.util.Locale;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    import org.springframework.web.servlet.ModelAndView;

    import edu.bit.grade.Score;

    /**
     * Handles requests for the application home page.
     */
    @Controller
    public class GradeController {
    	
    	private static final Logger logger = LoggerFactory.getLogger(GradeController.class);
    	
    	/**
    	 * Simply selects the home view to render by returning its name.
    	 */
    	@RequestMapping("/grade/Input")
    	public String grade() {
    		
    		return "/grade/Input";
    	}
    	
    	@RequestMapping("grade/print")
    	public String grade(Score score) {
    		
    		return "grade/print";
    	}
    	
    	
    	
    	
    	
    }
    ```

- get,set메소드 소스코드

    ```java
    package edu.bit.grade;

    public class Score {
    	private int kor;
    	private int eng;
    	private int math;
    	
    	public Score() {
    		
    	}

    	public int getKor() {
    		return kor;
    	}

    	public void setKor(int kor) {
    		this.kor = kor;
    	}

    	public int getEng() {
    		return eng;
    	}

    	public void setEng(int eng) {
    		this.eng = eng;
    	}

    	public int getMath() {
    		return math;
    	}

    	public void setMath(int math) {
    		this.math = math;
    	}
    	

    }
    ```

- 결과

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5f82219-0749-4776-8836-cc828c814e38/.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5f82219-0749-4776-8836-cc828c814e38/.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56ca829b-74bf-4d52-a82e-2a285fbb1d4d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56ca829b-74bf-4d52-a82e-2a285fbb1d4d/Untitled.png)

---

```jsx
-제일위에 입력버튼 하나를 만든다.
-버튼을 누르면 200*200 파란색 박스가 하나씩 계속해서 생기는 프로그램을 만드시오.
```

- 자바스크립트 소스코드

    ```jsx
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    <script>
    	window.onload = function() {
            function boxClick(){
            	var boxNode = document.createElement("div");
            	// 자바스크립트로 HTML요소를 만드는 createElement를 사용하여 div태그 생성
            	var textNode = document.createTextNode("Box");
            	// 선택한 요소에 텍스트를 추가하는 요소인 createTextNode사용하여 텍스트를 추가
            	
            	//css속성을 자바스크립트로 작성
            	boxNode.style.width = "200px";
            	boxNode.style.height = "200px";
            	boxNode.style.lineHeight = "300px";
            	boxNode.style.textAlign = "center";
            	boxNode.style.color = "#F4FFFF";
            	boxNode.style.backgroundColor = "#0000CD";
            	boxNode.style.margin = "10px";
            	
            	//버튼을 누르면 박스를 생성하기위해 선택한 요소에 자식요소를 추가하는 appendChild를 사용
            	//ex)버튼을 누르면 div안에 div안에 div안에 div...그만누를때까지 무한생성
            	boxNode.appendChild(textNode); // 텍스트추가	
            	document.body.appendChild(boxNode); // 상자생성
            };
         var button = document.getElementById("boxButton");
         // input태그를 선택
         button.addEventListener("click", boxClick, false);
         // 버튼을 누를때 boxClick함수를 호출한다.
      };
    	</script>
    </head>
    <body>
    	<input type="button" id="boxButton" value="Click Here!!" />
    </body>
    </html>
    ```

- 결과

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb8ed3e7-b77f-41e2-9689-17c39cb051ae/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb8ed3e7-b77f-41e2-9689-17c39cb051ae/Untitled.png)
