servlet-context.xml : 인식이 필요하니 각 컨트롤러의 설정, html 설정들 관리 

MultipartFile 메소드 : 

* getOriginalFilename() -> 전송파일명
* transfer(); -> 서버내부 내용 저장



29장 ajax

1> 기기 다양 - 폰, 패드, 노트북, pc다양한 화면 크기 화면 너무 많은 내용 들어가면 힘들다 

2> map

3> 기본화면구성 + 추가 내용 변경 동적 



http
1>요청응답
2>기본적 요청 처리 시간동안 클라이언트 대기한다. 화면 아무 변화없다. 응답 후에 전체 화면은 변경된다.

---> 동 기 화 통 신 방 식 synchronous)



ajax특징

서버가 다른 작업을 하는 도중에도 다른 요청을 할 수 있다.

--------

ajax

### LoginAjaxController

```java
@Controller
public class LoginAjaxController {
	@RequestMapping(value= "/ajax/login", method=RequestMethod.GET)
	public String loginform() {

		return "/ajax/ajaxlogin"; //view 리턴
	}
```

> * loginform 보여준다

### LoginAjaxController

```java
@RequestMapping(value= "/ajax/login", method=RequestMethod.POST, produces = {"application/json;charset=utf-8"})
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
```

> * String 타입의 ajax 파일로 전송한다.

### ajaxlogin.jsp

```javascript
$(document).ready(function(){
	$("#ajaxbtn").on('click', function(){
		/* 	$("#id").val() //name=id 말고 id=id 이다!
			$("#pw").val() //id=pw */
		$.ajax({
			url : "/multi/ajax/login",  //action속성에 지정될 값,  <form action="/multi/ajax/login"
			data : {'id':$('#id').val() , 'pw':$('#pw').val() }, // id라는 변수명으로 input id 를 넣겟다, pw라는 변수명으로 input pw 를 넣겟다
			type:  'post', // method="post"
			dataType: 'json', // 서버로 ajax 요청  - 서버처리 결과저장변수
			success : function(serverdata){ //"{\"process\" :\"정상로그인\",\"role\" :\"admin\"}" ;
				$("#result").html("<h1>" + serverdata.role + "역할로" + serverdata.process + "</h1>");	
				$("#result").css("color","blue"); 
				var o = JSON.parse(serverdata);
				alert(o); //

			} //String 결과
			
		});//ajax 함수 
	});//on end
    
    </script>
</head>
<body>
    아이디 : <input type="text" name ="id" id="id"><br>
암호 : <input type="password" name="pw" id="pw"><br>
<button id="ajaxbtn">ajax로그인</button>

<button id="ajaxbtn2">회원정보주세요</button>
```



#### LoginAjaxController

```java
@RequestMapping(value= "/ajax/memberinform", method=RequestMethod.GET, produces = {"application/json;charset=utf-8"})
	@ResponseBody 
	public MemberVO getMemeberInform() { //parameter 없다 , @ResponseBody가  리턴타입=자바객체 ==> json형태로 변환
		//처리
		MemberVO vo =new MemberVO("MEMBER1", 1111, "김철수", "KIM@MULTI.COM");
		return vo;  //{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }		
	}
```

> * MemberVO 타입으로 전송한다.
> * @ResponseBody 가  리턴타입=자바객체 ==> json형태로 변환
>   * { "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" } 



### ajaxlogin.xml

```java
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
	$("#ajaxbtn2").on('click', function(){ //회원정보 주세요
		$.ajax({
			url : "/multi/ajax/memberinform", 
			type:  'get', // 
			success : function(serverdata){ 
				 //{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }
				$("#result").html("<h1>" + serverdata.memberid + " " + serverdata.password + "</h1>");	
				$("#result").append("<h1>" + serverdata.membername + " " + serverdata.email + "</h1>"); //append 위에있는거에 추가하겠따.
				$("#result").css("color","blue"); 
				//자바 객체.toString() 패키지명.클래스명@16진수
				//js에서 json객체를 String(문자열) 로 변경해주는 방법 ==> JSON.stringify(serverdata)
				// alert(serverdata); object Object
				alert(JSON.stringify(serverdata)); //{"memberid":"MEMBER1","password":1111,"membername":"김철수","email":"KIM@MULTI.COM"}
				//서버데이터가 String형태인걸 json형태로 바꾸고 싶다 ==> JSON.parse(serverdata)
				var o = JSON.parse(serverdata) ; // 객체로 못만드는 오류 날때
				alert(o);
			}
		});//ajax 함수 
	});//on end
	
	
});//ready
</script>
</head>
<body>

아이디 : <input type="text" name ="id" id="id"><br>
암호 : <input type="password" name="pw" id="pw"><br>
<button id="ajaxbtn">ajax로그인</button>

<button id="ajaxbtn2">회원정보주세요</button>

<div id=result></div>

</body>
</html>
```

### LoginAjaxController

```java
@RequestMapping(value= "/ajax/memberlist", method=RequestMethod.GET, produces = {"application/json;charset=utf-8"})
	@ResponseBody 
	public ArrayList<MemberVO> getMemeberList(int count) { // 매개변수 int count 가 자동형변환한다
			//처리
		ArrayList<MemberVO> list = new ArrayList<MemberVO>();
		list.add(new MemberVO("member1", 1111, "김회원", "kim@mul.com"));
		list.add(new MemberVO("member2", 1111, "박대한", "kim@mul.com"));
		list.add(new MemberVO("member3", 1111, "김민국", "kim@mul.com"));
		list.add(new MemberVO("member4", 1111, "홍길동", "kim@mul.com"));
		list.add(new MemberVO("member5", 1111, "최회원", "kim@mul.com"));

			return list;  //{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }	
		
			//자바객체 ArrayList의 객체는 뭐냐 = 자바 스크립트 배열 
  /*	[	{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }
			{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }
			{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }
			{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" }
			{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" } ]   */
	}
```

> * Arrarylist<MemberVO> 타입으로 전송한다



### ajaxlogin.jsp

```javascript
$(document).ready(function(){
$("#ajaxbtn3").on('click', function(){ //회원정보 주세요
		$.ajax({
			url : "/multi/ajax/memberlist", 
			data: {'count' : 5}, //5명의 회원정보  , 전송하는 count 
			type:  'get', // 
			success : function(serverdata){ 
				 //[{ "memberid" : "MEMBER1" , "password": "1111" , "membername" : "김철수" , "email" : "KIM@MULTI.COM" } ... 5개]
				 	$("#result").css("color","blue"); 
				for( var i = 0 ; i <serverdata.length; i++){
				$("#result").append("<h1>" + serverdata[i].memberid + " " + serverdata[i].password  + " "); //반복이라 html -> append 로 바꾼다.	
				$("#result").append( serverdata[i].membername + " " + serverdata[i].email + "</h1>"); //append 위에있는거에 추가하겠따. append 구분된 줄로 출력된다.
	
				}//for end
				//자바 객체.toString() 패키지명.클래스명@16진수
				//js에서 json객체를 String(문자열) 로 변경해주는 방법 ==> JSON.stringify(serverdata)
				// alert(serverdata); object Object
				alert(JSON.stringify(serverdata)); //{"memberid":"MEMBER1","password":1111,"membername":"김철수","email":"KIM@MULTI.COM"}
				//서버데이터가 String형태인걸 json형태로 바꾸고 싶다 ==> JSON.parse(serverdata)
				var o = JSON.parse(serverdata) ; // 객체로 못만드는 오류 날때
				alert(o);
				
			
			} // 반복문 형태 
		});//ajax 함수 
	});//on end
</script>
</head>
<body>
<button id="ajaxbtn3">회원리스트주세요</button>
<div id=result></div>
```

> * 



login ajax는 

@RestController 가 작성되면 @ResponseBody 가 없어도 전부 json형태, @ResponseBody가 들어가있다고 취급한다. 여기선 loginform이 @ResponseBody 가 안붙어있어서 쓸 수 없다 .





-----------

# mybatis





### EmpVO

```java
package mybatis;

public class EmpVO {
	//employees 테이블에있는 컬럼들을 변수로 지정해서 = 1개의 레코드를 나타내도록
	int employee_id;
	String first_name, last_name, email, phone_number, hire_date,job_id;
	double salary, commision_pct;
	int manager_id, department_id;
	
	public int getEmployee_id() {
		return employee_id;
	}
	public void setEmployee_id(int employee_id) {
		this.employee_id = employee_id;
	}
	public String getFirst_name() {
		return first_name;
	}
	public void setFirst_name(String first_name) {
		this.first_name = first_name;
	}
	public String getLast_name() {
		return last_name;
	}
	public void setLast_name(String last_name) {
		this.last_name = last_name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPhone_number() {
		return phone_number;
	}
	public void setPhone_number(String phone_number) {
		this.phone_number = phone_number;
	}
	public String getHire_date() {
		return hire_date;
	}
	public void setHire_date(String hire_date) {
		this.hire_date = hire_date;
	}
	public String getJob_id() {
		return job_id;
	}
	public void setJob_id(String job_id) {
		this.job_id = job_id;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public double getCommision_pct() {
		return commision_pct;
	}
	public void setCommision_pct(double commision_pct) {
		this.commision_pct = commision_pct;
	}
	public int getManager_id() {
		return manager_id;
	}
	public void setManager_id(int manager_id) {
		this.manager_id = manager_id;
	}
	public int getDepartment_id() {
		return department_id;
	}
	public void setDepartment_id(int department_id) {
		this.department_id = department_id;
	}
	
}

```



### mybatis-config.xml

```javascript
```

mybatis는 sql 실행할때 별도의 xml 정의한다. 

select 문이면 select / insert면 insert 사용하고 id가 중요하다 읽어올때 id 를 읽어오고, mapper namespce도 정해서 namespace.id 식으로 읽어온다



### sql-mapping.xml

```java
<mapper namespace="emp"> <!--jstl 태그 쓸때 <c:foreach처럼 emp.emplist -->
<select id="emplist" resultType="empVo"> <!--emplist로 실행할거다. 한개의 레코드 타입은 empVO <typeAlias 에서 정의한것  -->
select * from employees
</select>
```





### EmpMain.java

```java
package mybatis;

import java.io.IOException;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class EmpMain {

	public static void main(String[] args) throws IOException {
		//마이바티스 설정관련된 모든 파일 읽어와라
		SqlSessionFactoryBuilder builder= new SqlSessionFactoryBuilder();
		
		//MYBATIS-CONFIX.XML 읽어와야된다, 설정 읽어서 연결, 결과 리턴받을 타입이 뭔지 sql정의해놓은 파일 뭔지 알 수 있다.
		SqlSessionFactory factory= builder.build(Resources.getResourceAsReader("mybatis/mybatis-config.xml") );
		
		//데이터베이스 연결객체 생성
		SqlSession session = factory.openSession();  //여기까지 기본 시작문장
		
		//sql 정의해놓은 태그중에  id=emplist 실행- 결과 List로 받아온다
		List<EmpVO> list=  session.selectList("emp.emplist"); //sql-mapping.xml 의 select id 읽어온다 , 여러개 가져올거같으면 selectList

		
		for (EmpVO vo : list) {
			System.out.println(vo.getEmployee_id()+ ":" + vo.getFirst_name() + ":" + vo.getHire_date() + ":" + vo.getSalary());
		} 
	
}

```

> *

### EmpMain.java

```java
EmpVO vo =session.selectOne("emp.empone"); //  한개만 가져올거같으면 selectOne
		System.out.println(vo.getEmployee_id()+ ":" + vo.getFirst_name() + ":" + vo.getHire_date() + ":" + vo.getSalary());

EmpVO vo =session.selectOne("emp.empone", 200); // 200번 사원 골라와라
		System.out.println(vo.getEmployee_id()+ ":" + vo.getFirst_name() + ":" + vo.getHire_date() + ":" + vo.getSalary());
		
		
```

### sql-mapping.xml

```java
<select id="empone" resultType="empVo">
select * from employees where employee_id=100 // 100번 사원 조회  

<select id="empone" resultType="empVo" parameterType="int">
select * from employees where employee_id=#{id} <!-- #{변수명} 변수명 사원 조회   -->
</select>
</mapper>
```



EmpService(interface)

```java
package mybatis;

import java.util.List;

public interface EmpService {
	public List<EmpVO> getEmpList(); //전체 목록,
	public EmpVO getEmpOne(int id); //int id 매개변수 , 해당id 하나만 가져온다
}

```

EmpServiceImpl

```java
package mybatis;

import java.util.List;

//main /mvc === service (1개 기능(여러개sql실행) 메소드 ) == dao (1개 sql실행하는 메소드 )
public class EmpServiceImpl implements EmpService {

		EmpDAO dao ;
		public void setDao(EmpDAO dao) {
			this.dao = dao;
		}
		
	@Override
	public List<EmpVO> getEmpList() {
	 // dao 1개 메소드 : 몇명이나 잇는지 확인하고 싶다 : select count(*) from employees sql 실행 몇몇 사원 
		return dao.getEmpList(); //List<EmpVO> list =  dao.getEmpList();
	}

	@Override
	public EmpVO getEmpOne(int id) {
		return dao.getEmpOne(id); //여기서받은값을 dao한테 준다
	}

}

```



EmpDAO

```java
package mybatis;

import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class EmpDAO {
	SqlSession session ;
	public void setSqlSession(SqlSession session) {
		this.session = session;
	}
	public List<EmpVO> getEmpList(){ //dao는 컨트롤러랑 연결될수있어서  
	//sql 정의해놓은 태그중에  id=emplist 실행- 결과 List로 받아온다
		List<EmpVO> list=  session.selectList("emp.emplist"); 
			return list;	
		
		} 
	
	public EmpVO getEmpOne(int id) {
	EmpVO vo =session.selectOne("emp.empone", id); //  한개만 가져올거같으면 selectOne
	return vo;
	//System.out.println(vo.getEmployee_id()+ ":" + vo.getFirst_name() + ":" + vo.getHire_date() + ":" + vo.getSalary());
	}	
	
	
}//dao는 db에 직접 접근해서 session객체가 필요해서 필드변수로 sqlsession 선언, setter메소드로 받아오도록 한다.

```



### EmpMain.java

```java
package mybatis;

import java.io.IOException;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class EmpMain {

	public static void main(String[] args) throws IOException {
		//마이바티스 설정관련된 모든 파일 읽어와라
		SqlSessionFactoryBuilder builder= new SqlSessionFactoryBuilder();
		
		//MYBATIS-CONFIX.XML 읽어와야된다, 설정 읽어서 연결, 결과 리턴받을 타입이 뭔지 sql정의해놓은 파일 뭔지 알 수 있다.
		SqlSessionFactory factory= builder.build(Resources.getResourceAsReader("mybatis/mybatis-config.xml") );
		
		//데이터베이스 연결객체 생성
		SqlSession session = factory.openSession();  //여기까지 기본 시작문장
		
		EmpDAO dao = new EmpDAO();
		dao.setSqlSession(session); // dao한테 session을 줘야 받아서 실행
		
		EmpServiceImpl service = new EmpServiceImpl(); //main은 service만 호출
		service.setDao(dao);
		
		List<EmpVO> list =service.getEmpList();
		for(EmpVO vo :list) {
		System.out.println(vo.getEmployee_id()+ ":" + vo.getFirst_name() + ":" + vo.getHire_date() + ":" + vo.getSalary());
		}
		
		System.out.println("=========================================================");
		EmpVO vo = service.getEmpOne(123);
		System.out.println(vo.getEmployee_id()+ ":" + vo.getFirst_name() + ":" + vo.getHire_date() + ":" + vo.getSalary());
		
	}

}

```





main : EmpService

EmpService(인터페이스) : 꼭 구현해야할 메소드 구현

EmpServiceImpl: 1개의 기능  단위를 나타내는 메소드 

EmpDAO : sql 1개를 실행하는 단위

--------------

EmpMain.java

```java
EmpVO vo = new EmpVO();
		vo.setEmployee_id(300);
		vo.setFirst_name("길동");
		vo.setLast_name("홍");
		vo.setEmail("hong@a.com");
		vo.setPhone_number("010.123.4567");
		vo.setJob_id("IT_PROG");
```

EmpService.java

```java
	public void insertEmp(EmpVO vo);
```

EmpServiceImpl

EmpDAO

sql-mapping.xml





---

mapping.xml

empdao

Empservice

EmpServiceImpl

EmpMain



--------------

mapping.xml

empdao

empservice

empserviceimpl

empmain



-----

# mybatis 반드시 들어가야할 문장

### main.java

```java
public static void main(String[] args) throws IOException {
		//마이바티스 설정관련된 모든 파일 읽어와라
		SqlSessionFactoryBuilder builder= new SqlSessionFactoryBuilder();
		//MYBATIS-CONFIX.XML 읽어와야된다, 설정 읽어서 연결, 결과 리턴받을 타입이 뭔지 sql정의해놓은 파일 뭔지 알 수 있다.
		SqlSessionFactory factory= builder.build(Resources.getResourceAsReader("mybatis/mybatis-config.xml") );
		//데이터베이스 연결객체 생성
		// insert 수행 - 자동 commit 종료 - commit
		SqlSession session = factory.openSession(true);  //여기까지 기본 시작문장
		//트랜잭션 - insert, update , delete 수행 - commit -db 영구 저장
		// insert, update , delete 수행 - rollback - 취소
		EmpDAO dao = new EmpDAO();
		dao.setSqlSession(session); // dao한테 session을 줘야 받아서 실행
		
		EmpServiceImpl service = new EmpServiceImpl(); //main은 service만 호출
		service.setDao(dao);
```

### DAO

```java
public class EmpDAO {
	SqlSession session ;
	public void setSqlSession(SqlSession session) {
		this.session = session;
	}
```

### Service

```java
생략
```

### ServiceImpl

```java
package mybatis;
import java.util.List;
//main /mvc === service (1개 기능(여러개sql실행) 메소드 ) == dao (1개 sql실행하는 메소드 )
public class EmpServiceImpl implements EmpService { 
		EmpDAO dao ;
		public void setDao(EmpDAO dao) {
			this.dao = dao;
		}
```

###  ???-config.xml

```javascript
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <!-- mybatis db 연결정보 세팅 파일 -->
<configuration>

<!-- 1. sql 실행 결과 ResultSet이 아니라 원하는 결과 EmpVO으로 매핑 -->
<typeAliases>
<typeAlias type="mybatis.???VO" alias="???Vo"/>
<typeAlias type="mybatis.EmpVO" alias="empVo"/>
</typeAliases>
<!-- 2.DataSource 설정 -->
<environments default="development">
<environment id="development">
	<transactionManager type="JDBC"/>
		<dataSource type="POOLED">
			<property name="driver" value="oracle.jdbc.driver.OracleDriver"/>
			<property name="url" value="jdbc:oracle:thin:@localhost:1521"/>
			<property name="username" value="hr"/>
			<property name="password" value="hr"/>
		</dataSource>
	</environment>
</environments>
<!-- 3. sql정의한 매퍼 설정  -->
<mappers>
<mapper resource="mybatis/sql-mapping.xml" />
<mapper resource="mybatis/???-mapping.xml" />
</mappers>


</configuration>

```

> <typeAlias type="mybatis.???VO" alias="???Vo"/>
> <typeAlias type="mybatis.EmpVO" alias="empVo"/>
>
> <mapper resource="mybatis/sql-mapping.xml" />
> <mapper resource="mybatis/???-mapping.xml" />



### ???-mapping.xml

```javascript
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="emp"> <!--jstl 태그 쓸때 <c:foreach처럼 emp.emplist -->
<mapper namespace="???">
    <select> </select>
	<insert> </insert>
	<update> </update>
	<delete> </delete>
	<select>
    	<foreach> </foreach>
	</select>
</mapper>
    
    
```

