### Mapping 예제
```java
@RequestMapping(value = "/hello.do", method = RequestMethod.GET)  
public ModelAndView hello() {  //사용자 콜벡 메소드  
    ModelAndView mav = new ModelAndView();  
    mav.addObject("result", "Hello Spring MVC!!");  
    mav.setViewName("/view/hello");  //jsp 파일 -> /view/hello.jsp    return mav;  
}
```

### jsp로 mapping 이 아닌 단순 문자를 화면에 보여줄 시 예제
```java
/*  
*   Spring에서 return type이 String이면 단순 문자열이 아니라, JSP 파일명으로 인식한다.  
*   만약에 단순 문자열로 처리 하고 싶으면 @ResponseBody를 써야 한다.  
 */  
@RequestMapping(value = "/hello3.do", method = RequestMethod.GET, produces = "text/html; charset = UTF-8")  
@ResponseBody  
public String hello3() {  //사용자 콜벡 메소드  
    return "welcome";  
}
```