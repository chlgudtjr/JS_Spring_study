# 2021-02-04(myBatis)

### 1. 마이바티스 사용 4가지 방법에 대하여 설명하시오.

**1번째 방법**

1. Interface를 만들고 Interface가 있는 경로를 쿼리를 구현할 XML파일의 nameSpace에 Mapping한다. EX : (<mapper namespace="edu.bit.ex.one.IBDao">)

![Untitled](https://user-images.githubusercontent.com/75012998/106878949-e6deb680-671d-11eb-9f6e-2ffd12e72886.png)

![Untitled 1](https://user-images.githubusercontent.com/75012998/106878965-e9d9a700-671d-11eb-98a6-53970b8b4259.png)

  2. SqlSession.getMapper(Interface명.class)를 이용

![Untitled 2](https://user-images.githubusercontent.com/75012998/106878964-e9d9a700-671d-11eb-991b-39e182de1b3a.png)

**2번째 방법**

  1. 1번방법과 달리 Interface가 필요없음.

  2. SqlSession에서 제공하는 함수인 selectList(),selectOne()등을 이용하여 함수안에 XML nameSpace명(개발자 마음대로 정함).클래스에 선언된 메소드명을 입력 

![Untitled 3](https://user-images.githubusercontent.com/75012998/106878960-e9411080-671d-11eb-9c80-326bb17f6019.png)

3. sqlSession에서 제공하는 함수에 적은 nameSpace명을 쿼리를 구현할 XML파일의 nameSpace에 적는다.

![Untitled 4](https://user-images.githubusercontent.com/75012998/106878959-e8a87a00-671d-11eb-8e97-23383506c490.png)

**3번째 방법**

  1. BoardServiceImpt에 구현된 함수를 BoardMapper Interface에 선언

![Untitled 5](https://user-images.githubusercontent.com/75012998/106878957-e8a87a00-671d-11eb-9564-639394ba0edb.png)

  2. BoardMapper.XML에 쿼리문 작성

![Untitled 6](https://user-images.githubusercontent.com/75012998/106878955-e80fe380-671d-11eb-8f0d-88a428da8833.png)

**4번째 방법**

  1. BoardMapper 인터페이스에 @Select 어노테이션을 사용하여 구현할 쿼리문을 작성

![Untitled 7](https://user-images.githubusercontent.com/75012998/106878952-e7774d00-671d-11eb-81bf-110251efb86c.png)

---

### 2. ajax+json으로 list를 뿌리시오.



### 1. 게시판 설계도를 그리시오(Model 2,MVC)?

 **1. Model 2 패턴**

![Untitled](https://user-images.githubusercontent.com/75012998/104706621-b16a2d00-575e-11eb-83e7-f2539afc7cc7.png)

**2. MVC 패턴**

![Untitled 1](https://user-images.githubusercontent.com/75012998/104706630-b29b5a00-575e-11eb-9332-b64e28165c2b.png)

---





<script type="text/javascript">
	$(document).ready(function(event) {
		$("#delete").click(function(event){
			event.preventDefault(); // 이벤트를 취소할 때 동작을 멈춘다.
			console.log("ajax 호출전");
			// <a>의 parent(<td>)의 parent 즉, <tr>를 지칭한다.(클로저)
			/*
				어떻게 제이쿼리는 this가 <a>인 것을 알고있을까?
				: a 태그내 .a-delete 클릭 이벤트가 발생 되었으므로!
				: $('.a-delete').click(function(event)
			 */
			var trObj = $(this).parent().parent();


			$.ajax({
				type : 'DELETE'; // AJAX의 타입(삭제)
				url : $(this).attr("href"), // <a>의(this) 속성(href)을 가져온다.(attr)
				cache : false,// 캐시를 false 설정하여 페이지가 새로 고쳐질때 데이터를 남기지 않는다.
				success : function (result) {
					console.log(result);
					if(result == "SUCCESS"){
						if(confirm("정말 삭제하시겠습니까?") == true) // 삭제 확인
							$(trObj).remove(); // trObj 변수를 삭제한다.(게시글 삭제)
							console.log("REMOVE");// 삭제 확인
							$(location).attr('href', '${pageContextn.request.contextPath}/board/list');// 삭제를 작업 후 리스트로 돌아감
						}else {
							return;
						}
					}
				},
				error : function(e) {
					console.log(e);
				}
			})
		});
	});

</script>
