# 21-01-22(MVC기초2)

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

![1](https://user-images.githubusercontent.com/75012998/105633331-44ac0c80-5e9b-11eb-810b-5447c086aaee.png)

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

