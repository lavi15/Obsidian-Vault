
tomcat-embed-jasper
- SpringBoot에서는 JSP사용을 권장하지 않음
- SpringBoot에서 JSP 파일을 컴파일하기 위해서는 tomcat-emebed-jasper를 의존성 추가 해야함
- JSP 파일은 URL을 통한 직접적인 접근을 하지 못하게 WEB-INF 폴더 아래 위치
- WEB-INF 디렉토리는 외부에서 접근이 불가능