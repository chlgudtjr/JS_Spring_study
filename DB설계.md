# 2021.02.03(DB설계)

### 1. DB 설계의 순서는?

> 데이터베이스 설계

- 현실 세계에서 데이터베이스화하려고 하는 대상을 설정하는 작업이다.

**과정**

 **1. 요구사항 수집 및 분석 단계**

 : 데이터베이스를 궁극적으로 사용할 구성원들의 요구사항과 기능적 요구사항을 함께 명시해야 한다.

**2. 개념적 설계 단계**

:  첫번째 단계를 거쳐서 얻어진 데이터베이스 요구사항을 기반으로 엔티티 추출하고 애트리뷰트와 관계를 정의하며, 그에 따른 산출물로 ER다이어그램을 작성한다.(하향식 방법)

 **3. 논리적 설계 단계**

: 두번째 산출물인 ER 다이어그램을 관계형 스키마와 같은 구현 데이터 모델로 변환하게 된다.

 **4. 물리적 설계 단계**

: 데이터베이스 파일에 대한 내부 저장 구조, 인덱스, 접근 경로 등을 명시함.

**도식화**

![DB](https://user-images.githubusercontent.com/75012998/106724776-fd1c4200-664b-11eb-91ce-02d7d53c3407.png)

---

### 2. 개념적 설계의 순서에 대하여 설명하시오.

 **1 . 데이터베이스 요구사항**

: 데이터베이스를 사용할 구성원들의 요구 사항을  수집 및 분석

 **2. 엔티티 추출**

 : 분석한 요구사항에서 현실 세계에서 실제하는 것을 추출한다.

 **3. 명사 정제 및 그룹 짓기**

:  데이터베이스 요구사항으로부터 얻어낸 명사들중 엔티티라고 생각되는 명사들을 추출,구분하고 대표하는 명사로 그룹의 이름을 붙이고 다른 그룹에 속하는 단어들 제거.

**4. 관계 설정하기**

: 동사를 포함하는 문장 속에서 관계를 추출, 엔티티 간의 관계를 설명하는 동사나 이벤트를 나타내는 동사로부터 찾음

**5. 애트리뷰트 결정**

 : 엔티티마다 서로 다른 값을 가지는 키 애트리뷰트를 설정하여 각각의 값이 식별이 가능하게 해야한다.

**설계 단계 도식화**

![02-03(DB설계) 62f8a57d979947b4a566ff3a7f8cb1f1](https://user-images.githubusercontent.com/75012998/106724775-fb527e80-664b-11eb-855b-57c3992b42bb.png)

---

### 3. list 및 content_view함수의 mock 테스트를 하시오.

```java
package edu.bit.ex.controller;

import static org.junit.Assert.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.forwardedUrl;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import lombok.Setter;
import lombok.extern.log4j.Log4j;

@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration({ "file:src/main/webapp/WEB-INF/spring/root-context.xml",
		"file:src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml" })
@Log4j
public class BoardControllerTest {

	@Setter(onMethod_ = { @Autowired })
	private WebApplicationContext ctx; // IOC컨테이너 = 스프링

	private MockMvc mockMvc;
	/*
	 * MockMvc란? 실제 객체와 비슷하지만 테스트에 필요한 기능만 가지는 가짜 객체를 만들어서 애플리케이션 서버에 배포하지 않고도 스프링
	 * MVC 동작을 재현할 수 있는 클래스를 의미.
	 */

	@Before
	public void setup() {
		this.mockMvc = MockMvcBuilders.webAppContextSetup(ctx).build();
	}

	@Test
	public void testList() throws Exception {
		mockMvc.perform(get("/board/list")).andExpect(status().isOk()).andDo(print())
				.andExpect(forwardedUrl("/WEB-INF/views/board/list.jsp"));
	}

	@Test
	public void testContent_view() throws Exception {
		mockMvc.perform(get("/board/content_view")).andExpect(status().isOk()).andDo(print())
				.andExpect(forwardedUrl("/WEB-INF/views//board/content_view.jsp"));
	}
}
```

**결과**

![Untitled](https://user-images.githubusercontent.com/75012998/106724780-fdb4d880-664b-11eb-81d3-09733dc45f3e.png)
