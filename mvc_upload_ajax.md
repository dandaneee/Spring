* model = 데이터
  * 데이터베이스 , 사용자가 편집하길 원하는 **모든 데이터**를 가지고 있어야 한다.
  * 데이터 변경이 일어났을때 모델에서 화면 UI를 직접 조정해서 수정할수있도록 뷰를 참조하는 내부속성값을 가지면 안된다. **뷰나 컨트롤러에 대해서 어떤 정보도 가지면 안된다.**
  * 모델의 속성 중 텍스트 정보가 **변경**이 된다면, 이벤트를 발생시켜 **누군가에게 전달**해야 하며, 누군가 모델을 **변경하도록 요청**하는 이벤트를 보냈을 때 이를 **수신할 수 있는 처리 방법을 구현**해야 한다. 또한 모델은 **재사용가능해야** 하며 **다른 인터페이스에서도 변하지 않아야** 한다. 	 

* view= 최종적으로 보여주는것
  * 다시 말해 데이터 및 객체의 입력, 그리고 보여주는 **출력을 담당**. 데이타를 기반으로 사용자들이 볼 수 있는 **화면**입니다.  
  *  모델이 가지고 있는 정보를 따로 **저장해서는 안된다. **화면에 **표시하기만** 하고 그 화면을 그릴 때 필요한 정보들은 저장하지 않아야 한다.
  * 모델이나 컨트롤러와 같이 **다른 구성요소들을 몰라야 된다.**화면에 표시해주는 역할만 가진다.
  * **변경**이 일어나면 변경통지에 대한 **처리방법을 구현**해야만 한다. 모델과 같이 변경이 일어났을 때 이른 누군가에게 변경을 **알려줘야 하는 방법을 구현**해야 한다. 뷰에서는 화면에서 사용자가 화면에 표시된 **내용을 변경**하게 되면 이를 **모델에게 전달해서 모델을 변경**해야 할 것이다. 그 작업을 하기 위해 변경 통지를 구현한다. 그리고 **재사용가능**하게끔 설계를 해야 하며 다른 정보들을 표현할 때 쉽게 **설계를** 해야 한다

* controller = 컨트롤러는 앱의 사용자로부터의 입력에 대한 응답으로 모델 및/또는 뷰를 업데이트하는 로직을 포함

  * 데이터를 클릭하고, 수정하는 것에 대한 **"이벤트"들을 처리**하는 부분

  * 모델이나 뷰에 대해서 알고 있어야 한다.

    **모델이나 뷰는 서로의 존재를 모르고**, 변경을 **외부로 알리고, 수신하는 방법만** 가지고 있는데 이를 **컨트롤러가 중재**하기 위해 모델과 그에 관련된 뷰에 대해서 알고 있어야 합니다. 

  * **모델이나 뷰의 변경을 모니터링** 해야 한다. **모델이나 뷰의 변경** **통지를** 받으면 **이를 해석**해서 각각의 **구성 요소에게 통지**를 해야 합니다.  또한, 애플리케이션의 **메인 로직은 컨트롤러가 담당**하게 됩니다. 
  
    



spring mvc- mvc+ front controller 로 의무적 구현

front controller 1개 

요청분석 - c -m -v 응답1 = 모든 처리를 다 하는게 아니라 어떤 컨트롤러를 시켜서 일을 할지 컨트롤러 관리

요청분석 2- c-m-v 응답2



디스패처- 핸들러매핑-컨트롤러 (모델,view 만듦) - 디스패처서블릿으로 -완성된 뷰를 만들어주는 역할 뷰 해석자(viewResolver) - view 출력 



새로운요청-디스패처-핸들러매핑추가-b컨트롤러추가-뷰추가-모델추가



"/" ==> http://localhost:9090/컨텍스트명 / "."



-------------------

26장 annotation(xml 태그 + annotation)



하나의 컨트롤러에서 여러가지 request를 할 수 있다



spring mvc 실행 흐름

오류발생:http: ip:port/context/a.spring --> 404 error --> wem.xml(*.campus)  확장자가 틀렷다 spring!=campus ==> ("/")

1.web.xml  url 확인할것

2. servlet-context.xml 확인할것



400번오류 - controller -@requestmapping - 메소드 (int id ) 타입 확인

404번 오류 - 

405번 오류 -get->post / post->get

​					서블릿 get -> dopost 메소드 / post -> doget 메소드

​				jsp 에서는 get/post 방식을 동일하게 취급했다.

​			controller get /post 나누어서 처리

500번 오류 -자바 예외들



---------------------

과제

컨트롤러 만든다 

어노테이션 (@controller , @requestmapping ("/memberlist") ) 입력

어레이리스트 데이터 삽입

모델이에요 알려준다 (모델엔뷰 생성자 생성)

list 삽입

viewname 생성

mv 리턴



```java
@Controller
public class MemberController{
    @RequestMapping("/memberlist")
ModelAndView getMEmberList() {
    ArrayList<MemberVO> list =new ArrayList<MemberVO>();
		list.add(new MemberVO("member1", 1111, "김회원", "kim@mul.com"));
		list.add(new MemberVO("member2", 2222, "박대한", "park@mul.com")); 
		list.add(new MemberVO("member3", 3333, "김민국", "min@mul.com")); 
		list.add(new MemberVO("member4", 4444, "홍길동", "hong@mul.com")); 
		list.add(new MemberVO("member5", 5555, "최회원", "choi@mul.com"));
		ModelAndView mv = new ModelAndView();
    	mv.addObject("memberlist", list) ;
    	mv.setViewName("memberlist"); // ViewResolver api 가 확장자나 경로 지정해준다.
    	mv.setViewName("member/memberlist");  //뷰가있는 폴더까지 표시 가능 
    
}
    
}
```



### memberlist.jsp

```javascript
<% ArrayList list = (ArrayList)request.getAttribute("memberlist");
%>
```

WEB-INF/views/member/memberlist.jsp --error

WEB-INF : 보안폴더같은 개념, jsp파일은 직접 볼수는 없다



--------------------------



### MemberController.java

```java
//아이디 암호 이름 이메일 입력 form view 
//입력 -서버 요청 -확인 결과 view 필요 
	@RequestMapping(value="/insert" , method = RequestMethod.GET) 
//value="/insert" 가있을때, method = RequestMethod.GET : get method 를 실행한다 
	String memberForm() {
		return "member/insertform";
	}

	/* @RequestMapping(value="/insert", method=RequestMethod.POST ) */  
	@RequestMapping(value="/insertprocess", method=RequestMethod.POST ) 
	//get 방식으로 insertform요청하고  post 방식으로 insertprosess 요청
	ModelAndView memberProcess(MemberVO vo) {
		//memberProcess(MemberVO vo)== vo.setMemberid(request.getParameter("memberid"); vo의 메소드와 똑같은 데이터 타입으로 저장된다.
		
		ModelAndView mv= new ModelAndView();
		
		if(!vo.getId().equals("spring") ) { //spring 이 존재한다고 가정하고, 다른 아이디로 회원가입 가능하다
			mv.addObject("result", "정상 회원 아이디로 사용 가능합니다");
		}else {
			mv.addObject("result", "아이디 중복입니다 다른아이디 입력하세요");
		}
		mv.setViewName("member/insertprocess");
		return mv;
	}

```

form 을 보여주는 요청 , 입력을 받아서 요청

> ```java
> @RequestMapping(value="/insert" , method = RequestMethod.GET) 
> ```
>
> * value="/insert" 가있을때, method = RequestMethod.GET : get method 를 실행한다 
>
> ```java
> @RequestMapping(value="/insertprocess", method=RequestMethod.POST ) 
> ```
>
> * get 방식으로 insertform요청하고  post 방식으로 insertprosess 요청
>
> ```java
> ModelAndView memberProcess(MemberVO vo) {
> ```
>
> * memberProcess(MemberVO vo)== vo.setMemberid(request.getParameter("memberid"); 
>
>   vo의 메소드와 똑같은 데이터 타입으로 저장된다.



### src/main/webapp/web-inf/view/member/insertform

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원가입 화면입니다.</title>
</head>
<body>
<h1>회원가입 폼 </h1>
<form action="/multi/insertprocess" method=post>
<!-- <form action="/multi/insert" method=post> -->
아이디: <input type="text" name="id"> <br>
암호: <input type="password" name="pw"> <br>
이름: <input type="text" name="name"> <br>
이메일: <input type="text" name="email"> <br>
<input type="submit" value="회원가입"> 
</form>

<img src="/multi/resources/images/coffee.gif">
<script src="/multi/resources/jquery-3.2.1.min.js"></script>
<script >
$(document).ready(
		function() {
	alert(1);
});
</script>
</body>
</html>
```

### insertprocess.jsp

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
<h1> ${param.id} : ${result }</h1>
<a href="/multi/insert">회원가입</a>하러 가세요
</body>
</html>
```

### servlet-context.xml

```javascript
<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />
```

> * resources 경로를 참조한다



---------------------------

annotation 인식 설정 추가 @component @service @repository @autowired @qualifier

```java
	@Autowired //MemberDAO 객체가 있다면 자동으로 주입해줘 
	MemberDAO dao;
```



controller - memberservice - memberdao - membervo -



##### annotation을이용한 mvc 작성

1. main 없애고 controller 를 만든다
   * @Autowired ()MemberService ser -> result 모델
2. memberserviceIple(인터페이스)를 사용하지않고 autowired로 dao를 읽어온다
3. member/ memberserivce.jsp ->${result}

 

# di 와 mvc 결합 실습



#### web.xml

tomcat서버가 /multi 컨텍스트로 전달해야될 정보가 있으면 전달하는곳이 여기다

```javascript
<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
```

### servlet-context.mxl

### member.xml

### edu.spring.multi/ MemberServiceController.java

```java
package edu.spring.multi;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import annotation.service.member.MemberService;

@Controller
public class MemberServiceController {
	@Autowired
	MemberService service;
	
	@RequestMapping("/memberservice")
	ModelAndView getMemberService(annotation.service.member.MemberVO vo) { 
        //MemberVO 가 중복되므로 어느패키지에있는지 명시해줘야한다. 
		//vo 선언 = vo.setMemberid(request.getParameter("memberid")) ;
		
		ModelAndView mv = new ModelAndView();
		//id spring password 1111 정상로그인 사용자입니다.
		service.login(vo);
		service.register(vo);
		
		mv.addObject("loginresult", "로그인결과"); //view 에 전해줄거다
		mv.addObject("registerresult", "회원등록결과");
		mv.setViewName("member/memberservice");
		
		return mv;
	}
}

```

> ```java
> ModelAndView getMemberService(annotation.service.member.MemberVO vo) { 
> ```
>
> * MemberVO 가 중복되므로 어느패키지에있는지 명시해줘야한다. 
> * vo 선언 = vo.setMemberid(request.getParameter("memberid")) ; 
>   * memberid 파라미터를 받아서 vo에 저장된 타입의 형태로 Memberid를 저장하겠다.

### memberservice.jsp

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
<h1>${loginresult }</h1>
<h1>${registerresult }</h1>
</body>
</html>
```



### MemberService.java  membervo객체가 들어간 생성자 생성

```java
package annotation.service.member;

public interface MemberService {
	//기능을 나타내는 단위를 하나 더 둔거다
		public void register(); //main 연동
		public void login();  //main 연동
		public String register(MemberVO vo); //자바는 오버로딩이 가능하므로 
		public String  login(MemberVO vo);
		
}

```

> * MemberService 메소드에 MemberVO vo 변수가 들어간 생성자 생성

 ### MemberServiceImpl 오버라이딩

```java
	@Override
	public String register(MemberVO vo) {
		boolean result = dao.selectMember(); //true (spring: 1111)
		if(!result) { //result가 true가 아닌 false인때 insertmember 실행
			dao.insertMember();
			return vo.getMemberid() + " 회원님 정상 등록되었습니다.";
					}
		return vo.getMemberid() + " 회원님 중복 아이디입니다..";
	}

	@Override
	public String login(MemberVO vo) {
		boolean result = dao.selectMember();
		if(result) { //result가 true라면 sysout 실행
			System.out.println("정상 로그인 사용자");
		 return vo.getMemberid() + " 정상 로그인 사용자 입니다.";
 		}
	
	  return "비정상 로그인 사용자입니다.";
	}
```



* controller 만들고 -service - dao -vo  > controller 가 -model 만들고 - view 만든다



------------

# 28장 mvc 다양한 기능

-file upload 기능

서버 이력서 / 사진 전송 / 상품 이미지 업로드 / 게시판 첨부파일 서버로 업로드 

1>  file upload 

html태그 업로드 기능

<form action="… "  

<span style="color:red">method = "post" </span>

<span style="color:red">multipart/form-data </span>

 

<input type="file" à 파일선택 열기 창

서버단에서 파일 전송받는 기능 -spring api + 



1>  pom.xml 27장

스프링 라이브러리 관리해주는 파일

file upload / ajax / mybatis … 추가해야할 라이브러리  설정 



## spring_first/fom.pox.xml

1.pom.xml 에 태그 추가 

```javascript
		</dependency> 
<!-- 파일 업로드 -->
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>

<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
	</dependencies>

```



2. 라이브러리 추가(mvnrepository 사이트)

3. servlet-context.xml

   파일업로드-Multipart - 인식태그

   ```javascript
   <beans:bean id="multipartResolver"
   class="org.springframework.web.multipart.commons.CommonsMultipartResolver" />
   ```

   

4. 클라이언트 form 

   input type= file name="f" > ------> MultipartFile f 매개 변수로 선언

5. MultipartFile 메소드

   getOriginalFilename() -> 전송파일 명

   transfer(); -> 서버내부 내용 저장 

-------------------



UploadVO.java

UploadController.java - 2개 매핑 메소드

uploadform.jsp

uploadresult.jsp

src/main/java ;package upload / 

### UploadVO.java

```java
package upload;

import org.springframework.web.multipart.MultipartFile;

public class UploadVO { //전송된 파일 받도록 
//파일 2개 전송, 설명, 전송자 이름 -->4개 INPUT JSP
		String name;
		String description;
		MultipartFile file1;
		MultipartFile file2;
		
		//getter/setter 필수, 
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public String getDescription() {
			return description;
		}
		public void setDescription(String description) {
			this.description = description;
		}
		public MultipartFile getFile1() {
			return file1;
		}
		public void setFile1(MultipartFile file1) {
			this.file1 = file1;
		}
		public MultipartFile getFile2() {
			return file2;
		}
		public void setFile2(MultipartFile file2) {
			this.file2 = file2;
		}
		
		//vo특징 : 생성자추가, 
		
		
		
		//toString 추가
}

```

> ```java
> MultipartFile file1;
> MultipartFile file2;
> ```
>
> * 업로드할 파일의 타입은 MultipartFile

### UploadController.java 

파일 업로드 메소드 컨트롤러 

```java
package upload;

import java.io.File;
import java.io.IOException;
import java.util.UUID;

//<context>cpn
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.multipart.MultipartFile;

@Controller
public class UploadController {
	
	@RequestMapping(value="/fileupload" , method=RequestMethod.GET) 
	public String uploadForm() {//view의 이름이 된다	
	return "/upload/uploadform"; 
		}
		
	@RequestMapping(value="/fileupload" , method=RequestMethod.POST)
	public String uploadResult(@ModelAttribute("vo") UploadVO vo) throws IOException{ 
			
			//전송 파일 2개 객체 생성
			MultipartFile multi1= vo.getFile1();
			MultipartFile multi2= vo.getFile2();
			//파일명 추출
			String filename1 = multi1.getOriginalFilename();
			String filename2 = multi2.getOriginalFilename();
			//서버 c:/kdigital2/upload 폴더 저장 
			String savePath = "C:/kdigital2/upload/";
			
			//hello.java.txt  마지막 확장자 찾아내라
			String ext1 = filename1.substring(filename1.lastIndexOf("."));
			String ext2 = filename2.substring(filename2.lastIndexOf("."));
			
			filename1= getUuid()+ext1; //난수 + 확장자 --> 파일이름이 중복되는걸 막기위해서 난수와 확장자를 더해서 파일명으로 저장. (419a240df1.jpg)
			filename2= getUuid()+ext2;
			
			File file1 = new File(savePath + filename1) ; 
			File file2 = new File(savePath + filename2) ;
			
			//저장
			multi1.transferTo(file1);
			multi2.transferTo(file2);
			
			System.out.println(getUuid());
			
			return "/upload/uploadresult" ; //${vo.name} 작성자 이름 , ${vo.description} 설명, ${vo.file1} 첫번째 입력한 파일 
			
		} //uplaodResult end
		
		public static String getUuid() { //난수 생성
			return UUID.randomUUID().toString().replace("-", "").substring(0,10); 		//947ee4af66
			//return UUID.randomUUID().toString().replace("-", ""); // .substring(0,10); //4a193051f5f145ad9e67c439697196c7 659e7170f95b4fda8a8d418f4e7000c8
			//return UUID.randomUUID().toString() ;//.replace("-", "");  //c61d61a6-9a8f-422f-9b41-e8577368413a
			//난수를 만든다 중복되지 않은 값을 만드려고, 너무 길어서 tostring 메소드를 만들어 줄인다.
		}
		
		
}

```

> ```java
> public String uploadForm() {//view의 이름이 된다	
> ```
>
> ```java
> public String uploadResult(@ModelAttribute("vo") UploadVO vo) throws IOException{ 
> ```
>
> * (String name, String description, MultipartFile file1, MultipartFile file2 ) == UploadVO  vo
>
> * @ModelAttribute("vo") 은  Multipart multi1 = vo.getFile1(); 
>
>    ==> UploadVO 의 Multipart 타입의 file1을 불러온다
>
> ```java
> MultipartFile multi1= vo.getFile1();
> MultipartFile multi2= vo.getFile2();
> ```
>
> * 전송파일 2개 객체 생성
>
> ```java
> String filename1 = multi1.getOriginalFilename();
> String filename2 = multi2.getOriginalFilename();
> ```
>
> * 파일명 추출
>
> ```java
> String savePath = "C:/kdigital2/upload/";
> ```
>
> * 서버 c:/kdigital2/upload 폴더 저장 
>
> ```java
> return "/upload/uploadresult" ; 
> ```
>
> * ${vo.name} 작성자 이름 , ${vo.description} 설명, ${vo.file1} 첫번째 입력한 파일 
>
> ```java
> 			//hello.java.txt  마지막 확장자 찾아내라
> 			String ext1 = filename1.substring(filename1.lastIndexOf("."));
> 			String ext2 = filename2.substring(filename2.lastIndexOf("."));
> 			
> 			filename1= getUuid()+ext1;
> //난수 + 확장자 --> 파일이름이 중복되는걸 막기위해서 난수와 확장자를 더해서 파일명으로 저장. (419a240df1.jpg)
> 			filename2= getUuid()+ext2;
> 			
> 			File file1 = new File(savePath + filename1) ; 
> 			File file2 = new File(savePath + filename2) ;
> 			
> 			//저장
> 			multi1.transferTo(file1);
> 			multi2.transferTo(file2);
> 
> 	public static String getUuid() { //난수 생성
> return UUID.randomUUID().toString().replace("-", "").substring(0,10); 		//947ee4af66
> return UUID.randomUUID().toString().replace("-", ""); //4a193051f5f145ad9e67c439697196c7 659e7170f95b4fda8a8d418f4e7000c8
> return UUID.randomUUID().toString() ;
> //c61d61a6-9a8f-422f-9b41-e8577368413a
> //난수를 만든다 중복되지 않은 값을 만드려고, 너무 길어서 tostring 메소드를 만들어 줄인다.
> 	}
> ```
>
> * 파일이름이 중복되는걸 막기위해서 난수와 확장자를 더해서 파일명으로 저장.
> * getUuid 를 사용해 난수생성 - 마지막확장자 (".") 출력 - 난수+확장자로 파일명 저장



src/main/java/webapp/WEB-INF/views/upload/

### uploadform.jsp

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
<h1> 파일 전송 폼</h1>
<form action="/multi/fileupload" method="post" enctype="multipart/form-data">
전송자 : <input type=text name="name" > <br>
설명 : <input type=text name="description"> <br>
파일명1 : <input type="file" name="file1"> <br>
//type=file 일때 전송 메소드는 반드시 post
파일명2 : <input type="file" name="file2"> <br>
		<input type="submit" value="파일전송">
		</form>

</body>
</html>
```

> * type=file 일때 전송 메소드는 반드시 post
>
> ```java
> enctype="multipart/form-data"
> ```
>
> * *POST를* 통해 파일을 보낼 수 있는 인코딩 유형

### uploadresult.jsp

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
<h1>실행되었습니다.</h1>
</body>
</html>
```

 



### 요약 

> * input type = file 이면 MultipartFile로 받는다, (전송방법은 반드시 post)
>
> ```java
> enctype="multipart/form-data"
> ```
>
> * *POST를* 통해 파일을 보낼 수 있는 인코딩 유형
> * 파일명 중복을 막기위해 getUuid 를 사용 난수생성 - 마지막확장자 (".") 출력 - 난수+확장자로 파일명 저장
> * String savePath = "C:/kdigital2/upload/"; >> 경로 마지막에도 "/ " 잊지말것



------

## 29장 restful 기능 mvc



rest (더보기 기능)



http

1>요청 -응답

2>요청 처리 시간 동안 클라이언트 대기한다. 응답 올때까지 화면 아무 변화 없다

--> **동기화 통신 방식(synchronous)**

3> 변경- 요청- 화면 - 응답 - 변경 

--> **비동기적인 통신 방식(asynchronous)**

--> java script and

--> xml 형식(비동기통신시 전달 데이터형태 초기-)

​    ===> ajax (asynchronous javascript and xml) 화면은 유지되면서 사용자의 선택에따라 정보를 추가한다

--> xml에서 javascript로 바꼇다. (string값들 해석하기 쉬워서 )

-->자바스크립트 객체 { "a": "test", "b":100 , "c":true, "d":{...} }

-->java script object notation ===>json

-->프로젝트할때 공공데이터 exel, json 로 가져옴



a={ "변수명": "값" ,} == a = {"b": "값"}

==>  a,b



(http 이전 상태정보 유지할 수 없으니 추가 HttpSession 생성)

ajax에서 데이터형식을 받을때 json 방식을 사용한다



@ResponseBody



@Controller

classA{



@RequestMapping("/a")

String m(){

return"a";

}



@RequestMapping("/b") 내가 요청했던 "/b"한테 b를 결과로 리턴

@ResponseBody

String m2(){

return "b"; --> "b" 결과로 리턴

}



-------------

#### LoginAjaxController.java

```java
package ajax;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class LoginAjaxController {
	@RequestMapping(value= "/ajax/login", method=RequestMethod.GET)
	public String loginform() {
		return "/ajax/ajaxlogin"; //view 리턴
	}
	@RequestMapping(value= "/ajax/login", method=RequestMethod.POST, produces = 		{"application/json;charset=utf-8"})
	@ResponseBody 
	public String loginresult(String id, String pw) { //{'id':$('#id').val() , 'pw':$('#pw').val() }, //너의 문서의 바디에 내가 전송하는걸 포함시켜 
		//처리
		String result = "";
		if(id.equals("spring") && pw.equals("1111") ) {
			result = "{\"process\" : \"정상로그인\" , \"role\" : \"admin\"}" ;
		}
		else {
			result = "{\"process\" : \"비정상로그인\" , \"role\" : \"user\"}" ;
		}
		return result;  //spring - STring 리턴 - java script 전송받으면 json
	}
	
}

```

> ```java
> produces = {"application/json;charset=utf-8"})
> ```
>
> * 출력값 한글가능 인코딩
>
> ```java
> @ResponseBody 
> public String loginresult(String id, String pw) { 
> //{'id':$('#id').val() , 'pw':$('#pw').val() },
> //너의 문서의 바디에 내가 전송하는걸 포함시켜 
> ```
>
> * RequestBody
>   * **클라이언트에서 서버로** 필요한 데이터를 전송하기 위해서 JSON이라는 데이터를 요청 본문에 담아서 서버로 보내면, 서버에서는 **@RequestBody** 어노테이션을 사용하여 `HTTP 요청 본문에 담긴 값들을 자바 객체로 변환` 시켜, 객체에 저장시킵니다.
> * ResponseBody
>   * **서버에서 클라이언트로** 응답 데이터를 전송하기 위해서 **@ResponseBody** 를 사용하여 `자바 객체를 HTTP 응답 본문의 객체로 변환`하여 클라이언트로 전송시키는 역할을 합니다.
>
> ```java
> result = "{process : 정상로그인 , role : admin}" ;
> 		 "{\"process\" : \"정상로그인\" , \"role\" : \"admin\"}" ;		
> ```
>
> * String 으로 읽기위해 각 단어들에 쓰인 "" 앞에 \ 사용
>
> ```java
> return result;  
> ```
>
> * spring - STring 리턴 - java script 전송받으면 json

### servlet-context.xml

```java
<context:component-scan base-package="ajax" />
```

### pom.xml 

ajax를 사용하기위해 pom 에 <dependency> 추가

```javascript
<!-- ajax databind -->
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.4</version>
</dependency>
```

### ajaxlogin.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="/multi/resources/jquery-3.2.1.min.js"></script>
<script>
$(document).ready(function(){
	$("#ajaxbtn").on('click', function(){
		/* 	$("#id").val() //name=id 말고 id=id 이다!
			$("#pw").val() //id=pw */
		$.ajax({
			url : "/multi/ajax/login", 
			data : {'id':$('#id').val() , 'pw':$('#pw').val() }, // id라는 변수명으로 input id 를 넣겟다, pw라는 변수명으로 input pw 를 넣겟다
			type:  'post',
			dataType: 'json',
			success : function(serverdata){ //"{\"process\" :\"정상로그인\",\"role\" :\"admin\"}" ;
				alert(serverdata.process);
			}
			
		});//ajax 함수 
	});//on end
});//ready
</script>
</head>
<body>
<!-- <form action="/multi/ajax/login" method="post">
아이디 : <input type="text" name ="id"><br>
암호 : <input type="password" name="pw"><br>
<input type="submit" value="로그인">
</form>
-->

아이디 : <input type="text" name ="id" id="id"><br>
암호 : <input type="password" name="pw" id="pw"><br>
<button id="ajaxbtn">ajax로그인</button>

<div id=result></div>

</body>
</html>
```

> ```javascript
> $(document).ready(function(){
> 	$("#ajaxbtn").on('click', function(){
> 		/* 	$("#id").val() //name=id 말고 id=id 이다!
> 			$("#pw").val() //id=pw */
> 		$.ajax({
> 			url : "/multi/ajax/login", 
> 			data : {'id':$('#id').val() , 'pw':$('#pw').val() }, // id라는 변수명으로 input id 를 넣겟다, pw라는 변수명으로 input pw 를 넣겟다
> 			type:  'post',
> 			dataType: 'json',
> 			success : function(serverdata){ //"{\"process\" :\"정상로그인\",\"role\" :\"admin\"}" ;
> 				alert(serverdata.process);
> 			}
> 			
> 		});//ajax 함수 
> 	});//on end
> });//ready
> ```
>
> * $("#id").val() 에들어간 id는 <input>태그에서 name=id 말고 id=id 이다.
> * $.ajax({url, data, type, dataType, sucess}) 의 형태
> * url: requestmapping 했을때 url
> * data : id라는 변수명으로 input id 를 넣겟다, pw라는 변수명으로 input pw 를 넣겟다
> * type: post
> * dataType:json
> * success: function(serverdata){ alert(serverdata.process )}





# 정리

> * model = 데이터, view= 최종 출력화면 , controller = 입력에 대한 응답, 모델 및/또는 뷰를 업데이트하는 로직을 포함
>
> * ##### annotation을이용한 mvc 작성
>
>   1. main 없애고 controller 를 만든다
>      * @Autowired ()MemberService ser -> result 모델
>   2. memberserviceIple(인터페이스)를 사용하지않고 autowired로 dao를 읽어온다
>   3. member/ memberserivce.jsp ->${result}
>
> *  중복되는 객체가 있을경우, 어느패키지에있는지 명시해줘야한다. 
> * di mvc 복합 사용 가능 (복잡함)
> * **파일 업로드**
>   * input type = file 이면 MultipartFile로 받는다, (전송방법은 반드시 post)
>   * enctype="multipart/form-data":  POST를 통해 파일을 보낼 수 있는 인코딩 유형
>   * 파일명 중복을 막기위해 getUuid 를 사용 난수생성 - 마지막확장자 (".") 출력 - 난수+확장자로 파일명 저장
>   * String savePath = "C:/kdigital2/upload/"; >> 경로 마지막에도 "/ " 잊지말것
> * **ajax, json**
>   * @ResponseBody :  **서버에서 클라이언트로 자바 객체를 HTTP 응답 본문의 객체로 변환**
