매개변수= 외부에서 받기 위함 

전달 받을걸 생성자에 매개변수로 정의하고 필요하면 외부에서 전달받아라 

di = 생성자 , setter



MVC --> spring MVC

dao vo = jsp 에 보여줄 결과물 데이터를 만들어낼 모델 <span style = "color:red">**(Model)**</span>

jsp파일들 = 웹서버에서 보여주는 <span style = "color:red">**VIEW**</span> 역할을 한다

servlet = 중앙에서 통제하는 역할 <span style = "color:red">(**Controller)**</span>



-----------

DispatcherServlet : front controller 

/*.mvc -->servlet을 호출할때  여러개의 mvc가 이 servlet을 호출한다  (모든 요청이 이 호출로 부르게된다)

/ --> front controller  모든url 이 이 servlet 을 호출하게 된다.



webmvc2 project

DispatcherServlet 실행, hello.jsp 화면 출력, Hellocontroller.java , controller, HandlerMapping , DispatcherServlet

#### DispatcherServlet.java

####  <span style = "color:red"> handlermapping, controller ,view 를 중앙에서 다 컨트롤 한다 </span>

````java
package test;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


/* 
 * http://localhost:9090/webmvc/
 * http://localhost:9090/webmvc/boardlist.spring
 * http://localhost:9090/webmvc/login.spring
 * http://localhost:9090/webmvc/shop.spring
 * */

@WebServlet("/")
public class DispatcherServlet extends HttpServlet { //controller 역할, 요청분석함

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			//a.spring전달 a.mvc 전달
			//hello.spring 입력만 - hello.jsp로 이동 
		String uri =request.getRequestURI(); // URI = 프로토콜과 포트번호 서버를 뺀 나머지 = webmvc/a.spring , webmvc/ , 리턴타입 String이다
		String sp[]= uri.split("/");
		String result =sp[sp.length-1];
		System.out.println(result); // webmvc2 ,주소창에 a.spring ,주소창에 	hello.mvc
		//hello.spring가 -->HelloController만 호출 --> "hello jsp" -->hello.jsp 응답
		
		//  c (Controller con) , mapping 되어진것만 받아와라 
		HandlerMapping mapping = new HandlerMapping();
		Controller con = mapping.getController(result); //매핑된 타입은 Controller 
		
		// m v 
		String viewname = con.handleRequest(request, response); // 현재요청/응답  --> view
		
		//forward로 이동
		RequestDispatcher rd = request.getRequestDispatcher(viewname); // 이 viewname 으로 이동하라
		//HelloController 에서 reqest.setA..("insa")
		rd.forward(request,response);
		
	} //hanlermapping controller view 를 중앙에서 다 컨트롤 한다 

}

````



> ```java
> public class DispatcherServlet extends HttpServlet { 
> ```
>
> * controller 역할, 요청분석함
> * a.spring전달 >> a.mvc 전달
>   hello.spring 입력만 >> hello.jsp로 이동 
>
> ```java
> String uri =request.getRequestURI();
> 		String sp[]= uri.split("/");
> 		String result =sp[sp.length-1];
> 		System.out.println(result); 
> ```
>
> *  URI = 프로토콜과 포트번호 서버를 뺀 나머지 = webmvc/a.spring , webmvc/ , 리턴타입 String이다
> *  uri.split("/"); " /" 를 기준으로 분리
> * String result =sp[sp.length-1];  >> webmvc2 
> * hello.spring가 -->HelloController만 호출 --> "hello jsp" -->hello.jsp 응답
>
> ```java
> 		HandlerMapping mapping = new HandlerMapping();
> 		Controller con = mapping.getController(result); 
> ```
>
> * <span style = "color:red"> c (Controller con) , mapping 되어진것만 받아와라 </span>
> *  매핑된 타입은 Controller
>
> ```java
> String viewname = con.handleRequest(request, response); 
> ```
>
> * m v  (Model and View)
> * 현재 <span style = "color:red"> **요청/응답  --> view**</span>
>
> ```java
> RequestDispatcher rd = request.getRequestDispatcher(viewname); 
> 		rd.forward(request,response);
> ```
>
> * forward로 이동
> * 이 viewname 으로 이동하라
> * HelloController 에서 reqest.setA..("insa")



#### (interface)Controller <span style= "color:red">**공통 기능 뭐다 정의** </span>

```java
package test;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface Controller {
	//여러가지 서로다른 하위 클래스 내부에서 공통 기능 뭐다 정의 
	public String handleRequest(HttpServletRequest request, HttpServletResponse response) ;
	
}
```

> * 여러가지 서로다른 하위 클래스 내부에서 <span style= "color:red">**공통 기능 뭐다 정의** </span>
>
> ```java
> public String handleRequest(HttpServletRequest request, HttpServletResponse response) ;
> ```
>
> * 하위 클래스는 handleRequest(request, response) 의 형태이다



#### HelloController  

#### <span style="color:red">전송할 request,response 오버라이딩하여 handlerequest로 리턴</span>

```java
package test;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloController implements Controller { //Controller

	@Override
	public String handleRequest(HttpServletRequest request, HttpServletResponse response) {
		request.setAttribute("insa", "hello jsp~~"); 
		return "hello.jsp";
		
		//<%=request.getAttribute("insa")%> jsp에서 쓸수있는 표현
		//${requestScope.insa} = ${insa} jsp에서 쓸수있는 표현
		
	} 

}

```



> ```java
> public class HelloController implements Controller { 
> ```
>
> * 인터페이스 Controller 의 형태를 가져온다
>
> ```java
> @Override
> 	public String handleRequest(HttpServletRequest request, HttpServletResponse response) {
> ```
>
> * 메소드 오버라이딩 해야한다 hello를 받아오는 역할
>
> ```java
> request.setAttribute("insa", "hello jsp~~");
> return "hello.jsp";
> ```
>
> * modle선정, 전달해라 ,포워드했을때 전달할 데이터 
> * view 선정해서 view를 알려주고있다
> * name: "insa"  에 data:"hello jsp~~"  저장
> * String handleRequest  로 "hello.jsp" 리턴!
>
> ```javascript
> <%=request.getAttribute("insa")%> jsp에서 쓸수있는 표현
> ${requestScope.insa} = ${insa} jsp에서 쓸수있는 표현
> ```
>
> * jsp 에 출력할수있는 문장



#### HandlerMapping 

<span style= "color:red">HelloController 로부터 data를 받아오고 이를 출력할 Name을 저장 </span>

```java
package test;

import java.util.HashMap;

public class HandlerMapping {
	HashMap<String, Controller> mappings;
	public HandlerMapping() {
		mappings = new HashMap<String, Controller>();
		mappings.put("hello.spring", new HelloController() ) ;
	}

		public Controller getController(String key) {
			return mappings.get(key); 
		}
}

```

> ```java
> HashMap<String, Controller> mappings;
> 	public HandlerMapping() {
> ```
>
> * HashMap의 형태로 <저장할 이름, 받아올 컨트롤러 (데이터)> 를 mappings 로 선언
> * HandlerMapping 메소드 선언
>
> ```java
> mappings = new HashMap<String, Controller>();
> 		mappings.put("hello.spring", new HelloController() ) ;
> 	}
> ```
>
> * mappings 에 "hello.spring"의 name을 저장하고 입력시 Hellocontroller.java를  호출한다 
>
> ```java
> public Controller getController(String key) {
> 			return mappings.get(key); 
> 	}//Controller end
> }
> 
> ```
>
> * Controller 의 기능 메소드 선언
> * 위에서 저장된 hashmap ("hello.sptring", new HelloController() ).get(key) 의 형태로 리턴



#### hello.jsp 

<span style= "color:red"> controller에서 받은 데이터를 handlermapping 을 통해 dispatcherservlet 의 명령을 받아 화면에 출력될 내용</span>

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title> <!--view  -->
</head>
<body>
<h1> 컨트롤러부터 전달하는 모델 데이터</h1>
<h3> <%=request.getAttribute("insa") %></h3>
<h3> ${insa }</h3>
</body>
</html>
```

> ```javascript
> <h3> <%=request.getAttribute("insa") %></h3>
> <h3> ${insa }</h3>
> ```
>
> * controller 에서 받은 "insa" 네임과 "hello jsp~~" 를 화면에 출력 
> * el 과 jstl 문법 사용





### 요약

> * 작성순서는 Controller(interface)- Controller - HandelrMapping - dispatcherServlet
>
> * 역할
>
>   *  DispatcherServlet(중앙 제어 controller)
>   * HandlerMapping (Controller 받아아줘 modelling)
>   * Controller (출력할데이터 view)
>     * jsp (출력할 데이터 서식 형식 view)
>
> * dispatcherservlet url 받고 찾아줘 - HandlerMapping - Controller 
>
> * 새로운 url 늘어날때마다 할일 
>
>   * controller(a.spring - new AController()) 만들고 
>
>   * model(A Controller) 만들고 
>
>   * view(a.jsp) 만들어

-----------

spring_first project

#### src/main/java/ edu.spring.multi / HelloController.java

```java
package edu.spring.multi;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class HelloController implements Controller {

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		ModelAndView mv= new ModelAndView();
		mv.addObject("insa", "Hello Spring mvc~~");
		mv.setViewName("hello");  //WB-INF/views/hello.jsp
		return mv;
	} //Controller
```

> ```java
> public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
> 		ModelAndView mv= new ModelAndView();
>   	    mv.addObject("insa", "Hello Spring mvc~~");
> 		mv.setViewName("hello");  //WB-INF/views/hello.jsp
> 		return mv;
> ```
>
> * ModelAndView 타입의 handleRequest 메소드 생성
> * ModelAndView mv 에 "insa" 객체와 "Hello Spring mvc~~" 데이터를 저장
> * View 의 이름 "hello"  저장

#### src / main / webapp / WEB-INF / views / hello.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title> <!--view  -->
</head>
<body>
<h1> 컨트롤러부터 전달하는 모델 데이터</h1>
<h3> <%=request.getAttribute("insa") %></h3>
<h3> ${insa }</h3>
</body>
</html>
```



#### src / main / webapp / WEB-INF / spring / appServlet / servlet-context.xml

**<span style="color:red">어떤 컨트롤러가 어떤 매핑을 할지 </span>**

```javascript
<!--hello.spring url이  - new HelloController() 매핑할거다 핸들러 매핑에게   -->
	<beans:bean id= "hello" class= "edu.spring.multi.HelloController" />
	
					<beans:bean id= "urlMapping" class= 					             "org.springframework.web.servlet.handler.SimpleUrlHandlerMapping" >
					<beans:property name="mappings">
					<beans:props>
                
	<beans:prop key="/hello.spring">hello</beans:prop>
	<!--hello.spring을 만들면 hello객체를 만들어줘 -->
			</beans:props>	
		</beans:property>
```

> ```javascript
> <beans:bean id= "hello" class= "edu.spring.multi.HelloController" />
> ```
>
> * hello.spring url이  - new HelloController() 매핑할거다 핸들러 매핑에게
> * 객체: hello , 위치 :"edu.spring.multi.HelloController" 
>
> ```javascript
> <beans:prop key="/hello.spring">hello</beans:prop>
> ```
>
> * hello.spring을 만들면 hello객체를 만들어줘



### 요약

> * servlet-context.xml 을 통해 위에서 사용했던 Controller 간략화(ModelAndView , addObject, set viewname 사용)
> * handler나 interface는 사용하지 않았다

---------------------------



package ; mvcannotation

#### **HelloAnnotationController:**

``` java
package mvcannotation;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
@Controller
public class HelloAnnotationController  {
	@RequestMapping("/hello.spring") //hello.spring 요청하면 실행하겠따. 1개의 컨트롤러에서 여러개의 요청을 해결하겠다.
	public ModelAndView handleRequest() {
		ModelAndView mv= new ModelAndView();
		mv.addObject("insa", "Hello Spring mvc~~");
		mv.setViewName("hello");  //WB-INF/views/hello.jsp
		return mv;
	} //Controller
		
	@RequestMapping("/hello2.spring") //hello2.spring 요청하면 실행하겠따. 중복되면 Ambiguous mapping에러 발생
	public ModelAndView handleRequest2() {
		ModelAndView mv= new ModelAndView();
		mv.addObject("insa", "Hello2 Spring mvc~~");
		mv.setViewName("hello");  //WB-INF/views/hello.jsp
		return mv;
	}
```

> ```java
> @Controller
> public class HelloAnnotationController  {
> 	@RequestMapping("/hello.spring") 
> 	public ModelAndView handleRequest() {
> 		ModelAndView mv= new ModelAndView();
> 		mv.addObject("insa", "Hello Spring mvc~~");
> 		mv.setViewName("hello");  //WB-INF/views/hello.jsp
> 		return mv;
> 	} //Controller
> ```
>
> * @Controller annotation 클래스 상부에 선언, (이건 Controller 야)
>
> *  @RequestMapping("/hello.spring")  :
>
>    hello.spring 요청하면 실행하겠다. 1개의 컨트롤러에서 여러개의 요청을 해결하겠다.
>
> ```java
> @RequestMapping("/hello2.spring") 
> ```
>
> * hello2.spring 요청하면 실행하겠다. **<span style = "color:red">중복되면 Ambiguous mapping에러 발생  </span>**



#### **src / main / webapp / WEB-INF / spring / appServlet / servlet-context.xml**

```javascript
<!-- mvccannotation.HelloAnnotationController .. --> 
	<context:component-scan base-package="mvcannotation" />
<!-- 	<context:component-scan base-package="annotation.service.member" /> -->
```

> ```javascript
> <context:component-scan base-package="mvcannotation" />
> ```
>
> * mvcannotation 패키지에서 어노테이션 사용하겠다 선언.



### 요약 

> * @Controller 클래스 상부에 선언
>
> *  @RequestMapping("/hello.spring")   ModelAndView 상부에 선언
>
> * <span style="color:red">**Annotation을 이용해 더욱 간략화 가능, 상부에 선언하고 import 하는거 잊지말자**</span>
>
> * servlet-context.xml 에서 annotation 사용 선언해줘야한다
>
> * 스프링은 dispatcher를 거쳐서 controller(폼을 보여주기 위해 ) 를 매핑하여 result 값을 return 한다
>
>   (spring >> dispatcher >> controller >> result >> return>> spring)
>
> * 컨트롤러는 일반 class

-----------------------



#### package edu.spring.multi; <span style="color:red">LoginController.java</span>

```java
@Controller
public class LoginController {
@RequestMapping("/loginform")	
	public ModelAndView loginstart() {
			//로그인 폼 화면을 보여준다
			ModelAndView mv = new ModelAndView();
			//mvc (m 없는 경우, vc만 존재)
			mv.setViewName("loginform");
			return mv;
		}
@RequestMapping("/loginform")	
	public String loginstart() {
			return "loginform"; //view 의 이름으로 취급한다
		} 
	
@RequestMapping("/loginform")	
	public void loginstart() {
			//자동 uri 동일 이름 뷰 결정
		} 
	
@RequestMapping("/loginform")	
	public ModelAndView loginstart() {
			//로그인 폼 화면을 보여준다
			ModelAndView mv = new ModelAndView();
			//mvc (m 없는 경우 전달할 매개변수가 없다, vc만 존재)
			mv.addObject("model", "모델값입니다.");
			//자동 uri 동일 이름 뷰 결정
			return mv;
		}	
    @RequestMapping("login")
	public String login() {
			return "redirect:/loginform";
			
			//return 타입이 String이고 redirect로 시작한다면 "/loginform" 으로 호출하라고 한다. 다시 호출해라
	}
	
}
```

> ```java
> @RequestMapping("/loginform")
> public ModelAndView loginstart() {
> 			ModelAndView mv = new ModelAndView();
> 			mv.setViewName("loginform");
> 			return mv;
> 		}
> ```
>
> * 로그인 폼 화면을 보여준다
> * mvc (m 없는 경우, vc만 존재)
>
> ```java
> @RequestMapping("/loginform")	
> 	public String loginstart() {
> 			return "loginform";
> 		} 
> ```
>
> * **String 타입**의 loginstart() 메소드를 선언
> * **return 타입이 위에 참고한 "loginform" 과 같을경우**, view 의 이름으로 취급한다
>
> ```java
> @RequestMapping("/loginform")	
> 	public void loginstart() {
> 		} 
> ```
>
> * **void타입**의 loginstart() 메소드 선언
> * **return타입이 없을 경우**, 자동 uri 동일 이름 view 결정
>
> ```java
> @RequestMapping("/loginform")	
> 	public ModelAndView loginstart() {
> 			ModelAndView mv = new ModelAndView();
> 			mv.addObject("model", "모델값입니다.");
> 			return mv;
> 		}	
> ```
>
> * ModelAndView 타입의 logintstart()선언을 해도  **mv.addObject 로 값을 저장**하면 
>
>   자동 uri 동일 이름 뷰 결정
>
> ```java
> @RequestMapping("login")
> 	public String login() {
> 			return "redirect:/loginform";
> 	}
> ```
>
> * @RequestMapping("login")으로 주소가 달라도,
> * return 타입이 **String이고 redirect로 시작한다면** "/loginform" 으로 호출하라고 한다. 다시 호출해라



### 요약 

> * modelandview에서 따로 viewname 을 지정하지 않아도, 메소드와 return 타입에 따라 viewname이 **자동으로 호출** 가능하다. (String, void type)
> * **mv.addObject 로 값을 저장**하면 자동 uri 동일 이름 뷰 결정
> * 호출 주소가 **달라도** return "redirect:/loginform"; 를 통해 loginform 으로 **다시 호출 할 수 있다.**

#### src / main / webapp / WEB-INF / views /<span style="color:red"> loginform.jsp</span>

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<form action="http://localhost:9090/multi/loginresult" method="get">
<!-- "/multi/loginresult" -->
<!-- "loginresult" -->
id : <input type="text" name="memberid"> <br>
password: <input type="password" name="password"> <br>
<input type="submit" value="스프링로그인">
</form>
</body>
</html>
```

> * form action의 주소는
>
>   *  "http://localhost:9090/multi/loginresult" 경로를 전부 지정하거나
>
>   *  "/multi/loginresult" 호스트 주소를 생략하거나
>   * "loginresult" 참고할 jsp의 이름만 입력해도 된다.

#### package edu.spring.multi; <span style="color:red">LoginController.java </span>

```javascript
	@RequestMapping("/loginresult") // '/' 앞에 multi 까지 포함되어 있다. /multi/loginresult 와 다른거다
	public ModelAndView loginresult(HttpServletRequest request) {
		String memberid = request.getParameter("memberid"); //request가 있으므로 parameter 읽어와야한다
		String password = request.getParameter("password");
		String result = "";
		
		if(memberid.equalsIgnoreCase("spring") && password.equalsIgnoreCase("1111") ) {
				result= "정상 로그인 사용자입니다.";
		}else {
				result= "로그인 정보 오류입니다.";
		}
		ModelAndView mv = new ModelAndView();
		mv.addObject("login", result);//모델 정보를 넘기는 명령어
		mv.addObject("user", memberid); //입력된 memberid 를 출력
		//mv.addObject("user", new memberVO()); 
		mv.setViewName("loginresult");
		return mv;
	}
	
```

> ```java
> @RequestMapping("/loginresult")
> ```
>
> *  '/' 앞에 multi 까지 포함되어 있다. /multi/loginresult 와 다른거다
>
> ```java
> public ModelAndView loginresult(HttpServletRequest request) {
> 		String memberid = request.getParameter("memberid"); 
> 		String password = request.getParameter("password");
> 		String result = "";
> ```
>
> * request가 있으므로 parameter 읽어와야한다
>
> ```java
> ModelAndView mv = new ModelAndView();
> 		mv.addObject("login", result);
> 		mv.addObject("user", memberid); 
> 		//mv.addObject("user", new memberVO()); 
> 		mv.setViewName("loginresult");
> 		return mv;
> ```
>
> * 모델 정보를 넘기는 명령어 ( if문에 출력된 결과 result 를 login 객체에 저장)
> * 입력된 memberid 를 출력 ( VO에 있는 memberid 를 user 객체에 저장)
> * 저장되는 Object는 java의 모든게 다 들어갈 수 있다. 
> * loginresult로 view이름을 설정하고 리턴한다.

#### src / main / webapp / WEB-INF / views /<span style="color:red"> loginresult.jsp</span>

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>로그인 처리 결과입니다.</h1>
<h3>${user  } </h3>
<h3>${login } </h3>

</body>
</html>
```

> ```java
> <h3>${user  } </h3>
> <h3>${login } </h3>
> ```
>
> * 전달받은 user(memberid) 와 login(result)값을 출력한다.	

#### package edu.spring.multi; <span style="color:red">LoginController.java</span>

```javascript
	@RequestMapping("/loginresult") // 
	public ModelAndView loginresult
    (@RequestParam("id") String memberid,@RequestParam("pw") int password) throws Exception{
	//public ModelAndView loginresult
    //(String id, int pw) throws Exception{ 
	

		String result = "";
		
		if(memberid.equalsIgnoreCase("spring") && password==1111 ) {
				result= "정상 로그인 사용자입니다.";
		}else {
				result= "로그인 정보 오류입니다.";
		}
		ModelAndView mv = new ModelAndView();
		mv.addObject("login", result);//모델 정보를 넘기는 명령어
		mv.setViewName("loginresult");
		return mv; //
	}
```

> ```java
> public ModelAndView loginresult(String id, int pw) throws Exception{ 
> ```
>
> *  loginform 에서 <span style= "color:red">**input값과 같은 name의 값으로 매개변수값을 주면**</span> 실행이 간략해진다.
> * 매개변수 int pw 는 pw= Integer.paseInt(request.getparameter("pw")) 문장과 같다 (자동형변환)
>
> ```java
> 	public ModelAndView loginresult
>     (@RequestParam("id") String memberid,@RequestParam("pw") int password) throws Exception{
> ```
>
> * <span style= "color:red">@RequestParam</span>
>   * 매개변수값이 달라도 ( ) 안의 매개변수값으로 읽어오도록 한다



#### package edu.spring.multi; <span style="color:red">LoginController.java</span>

```javascript
@RequestMapping("/loginresult") //
	public ModelAndView loginresult(@ModelAttribute("vo") LoginVO vo ) throws Exception{ //@ModelAttribute("vo") jsp에서 쓸수있는 vo를 지정하겠다
	//public ModelAndView loginresult(LoginVO vo ) throws Exception{ 
		//vo 변수이름 = form 파라미터이름 자동 전달 - set.Id, set.Pw 호출 
		
		String result = "";
		System.out.println(vo.getId() + ";" + vo.getPw() );
		if(vo.getId().equalsIgnoreCase("spring") &&vo.getPw()==1111 ) {
				result= "정상 로그인 사용자입니다.";
		}else {
				result= "로그인 정보 오류입니다.";
		}
		ModelAndView mv = new ModelAndView();
		mv.addObject("login", result);//모델 정보를 넘기는 명령어

		mv.setViewName("loginresult");
		return mv;
```

> ```java
> public ModelAndView loginresult(LoginVO vo ) throws Exception{
> ```
>
> * (vo 변수이름 = form 파라미터이름) 자동 전달 - set.Id, set.Pw 호출 
>
> ```java
> public ModelAndView loginresult(@ModelAttribute("vo") LoginVO vo ) throws Exception{ 
> ```
>
> * @ModelAttribute("vo") jsp에서 쓸수있는 vo를 지정하겠다
>
> ```java
> System.out.println(vo.getId() + ";" + vo.getPw() );
> ```
>
> * ModelAndview 에서 따로 선언하지 않아도 (LoginVO vo) 로 선언해서 get.Id, get.Pw 사용가능

#### package edu.spring.multi;<span style="color:red">LoginVO.java</span>

```java
package edu.spring.multi;
public class LoginVO {
		String id;
		int pw;
		public String getId() {
			return id;
		}
		public void setId(String id) {
			this.id = id;

	}
		public int getPw() {
			return pw;
		}
		public void setPw(int pw) {
			this.pw = pw;
		}
}
```

> * public ModelAndView loginresult(LoginVO vo ) throws Exception{ 
>   * vo 변수이름 = form 파라미터이름 자동 전달 - set.Id, set.Pw 호출 
> * public ModelAndView loginresult(@ModelAttribute("vo") LoginVO vo ) throws Exception{
>   * @ModelAttribute("vo") jsp에서 쓸수있는 vo를 지정하겠다
> * System.out.println(vo.getId() + ";" + vo.getPw() ); --> 변경



### 요약

> * modelandview에서 따로 viewname 을 지정하지 않아도, 메소드와 return 타입에 따라 viewname이 **자동으로 호출** 가능하다. (String, void type)
>
> * **mv.addObject 로 값을 저장**하면 자동 uri 동일 이름 뷰 결정
>
> * 호출 주소가 **달라도** return "redirect:/loginform"; 를 통해 loginform 으로 **다시 호출 할 수 있다.**
>
> * form action의 주소는
>
>   * 주소 전부입력
>   * host주소 생략
>   * 참고할 jsp 이름 만 입력해도 된다.
>
> * mv.addObject("user", **new memberVO()**); 저장되는 Object는 java의 모든게 다 들어갈 수 있다.
>
> * loginform 에서 <span style= "color:red">**input값과 같은 name의 값으로 매개변수값을 주면**</span> 실행이 간략해진다. 
>
>   ```java
>   public ModelAndView loginresult
>   (@RequestParam("id") String memberid,@RequestParam("pw") int password) throws Exception{
>   ```
>
> * 매개변수 int pw 는 pw= Integer.paseInt(request.getparameter("pw")) 문장과 같다 **(자동형변환)**
>
> * <span style= "color:red">@RequestParam</span>: 매개변수값이 달라도 ( ) 안의 매개변수값으로 읽어오도록 한다
>
> ```java
> public ModelAndView loginresult(@ModelAttribute("vo") LoginVO vo ) throws Exception{ 
> ```
>
> * (vo 변수이름 = form 파라미터이름) 자동 전달 - set.Id, set.Pw 호출 
> * @ModelAttribute("vo") jsp에서 쓸수있는 vo를 지정하겠다
> * ModelAndview 에서 따로 선언하지 않아도 (LoginVO vo) 로 선언해서 get.Id, get.Pw 사용가능



