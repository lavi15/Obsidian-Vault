## 스프링 MVC의 구성 요소

1.  [[DispatcherServlet]]
- 클라이언트의 요청을 전달 받음
- 컨트롤러에게 클라이언트의 요청을 전달하고 Controller가 리턴한 결과 값을 View에 전달

2. [[HandlerMapping]]
- 클라이언트의 요청 URL을 어떤 Controller가 처리할지를 결정

3. [[Controller]]
- 클라이언트의 요청을 처리한 뒤 결과를 DispatcherServlet 전달

4. [[ModelAndView]]
- 컨트롤러가 처리한 결과 정보 및 뷰 선택에 필요한 정보를 담음

5. [[ViewResolver]]
- 컨트롤러의 처리 결과를 생성할 뷰를 결정

6. [[View]]
- 컨트롤러의 처리 결과 화면을 생성
- JSP나 Velocity 템플릿 파일등을 뷰로 사용

![[Pasted image 20230919110419.png]]

