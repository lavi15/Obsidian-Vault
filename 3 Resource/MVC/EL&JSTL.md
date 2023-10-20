## EL(Expression Language)

- Jsp 2.0에서 나왔으며 Jsp의 기본 문법 보완
- jar 파일 불필요
![[Pasted image 20230919105925.png]]

## JSTL (JSP Standard Tag Library)

- Jsp 표준 태그
- <%=%> ⇒ ${}
- jar 파일 필요(**[https://mvnrepository.com](https://mvnrepository.com))(jstl-1.2.jar)(standard-1.1.2.jar)**
- 태그 종류
![[Pasted image 20230919110010.png]]

### 변수설정
```html
<h3>*** 변수 설정 ***</h3>
<c:set var="name" value="홍길동" />
<c:set var="age">25</c:set>

나의 이름은 ${name} 입니다. <br>
내 나이는 <c:out value="${age}"/>입니다.<br>
```

### for & if

```html
//for
<h3>*** forEach ***</h3>
<c:forEach var="i" begin="1" end="10" step="1">  <%-- for(int i =1; i<=10; i++) 과 동일 --%>
    
</c:forEach>

//foreach
<c:forEach var="data" items="${paramValues.hobby}">
    ${data}
</c:forEach>

/if
<h3>*** forEach ***</h3>
<c:forEach var="i" begin="1" end="10" step="1">  <%-- for(int i =1; i<=10; i++) 과 동일 --%>
    
</c:forEach>

```

### UTF-8 incording

```html
<%@ taglib prefix="fmt" uri="<http://java.sun.com/jsp/jstl/fmt>" %>

<fmt:requestEncoding value="UTF-8" />
```

### choose when

```html
<c:choose>
    <c:when test="${param.color == 'red'}" >빨강</c:when>
    <c:when test="${param.color =='green'}" >초록</c:when>
    <c:when test="${param.color == 'blue'}" >파랑</c:when>
    <c:when test="${param.color == 'magenta'}" >보라</c:when>
    <c:otherwise>하늘</c:otherwise>
</c:choose>
```

### 예제

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="<http://java.sun.com/jsp/jstl/core>" %>
<%@ taglib prefix="fmt" uri="<http://java.sun.com/jsp/jstl/fmt>" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<fmt:requestEncoding value="UTF-8" />
<ul>
    <li>이름 : ${param.name}</li>
    <li>나이 : ${param.age}살
        <c:if test="${param.age >= 19}" >성인</c:if>
        <c:if test="${param.age < 19}" >미성년자</c:if>
    </li>
    <li>색깔 :
        <c:choose>
            <c:when test="${param.color == 'red'}" >빨강</c:when>
            <c:when test="${param.color =='green'}" >초록</c:when>
            <c:when test="${param.color == 'blue'}" >파랑</c:when>
            <c:when test="${param.color == 'magenta'}" >보라</c:when>
            <c:otherwise>하늘</c:otherwise>
        </c:choose>
    </li>
    <li>취미 :
        ${paramValues.hobby[0]}
        ${paramValues.hobby[1]}
        ${paramValues.hobby[2]}
        ${paramValues.hobby[3]}
        ${paramValues.hobby[4]}
    </li><br><br>
    <c:forEach var="data" items="${paramValues.hobby}">
        ${data}
    </c:forEach>

</ul>
</body>
</html>
```

### JSTL Data 전송

jsp의 내장객체는 9가지가 있습니다. 그중에서 저희가 다룬건

- page
- request
- response
- session 정도이지만
- application이라는 것도 있습니다. (존재정도만 설명)

이를 jstl에서는 pageScope requestScope responseScope sessionScope 로 불러낼수있습니다.

page < request < session < application 순서로 범위가 크다.

왜냐하면,

두 페이지간 발생하는 forward와 sendRedirect 명령에서 알수있다.

forward에서는 request가 이동전 페이지 하나만 잡히지만

sendRedirect는 request가 각각 잡힌다.