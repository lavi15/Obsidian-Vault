
## Rest Docs vs Swagger
### Rest Docs
**장점**
- 테스트를 통과해야 문서가 만들어 짐(신뢰도가 높음)
- 프로덕션 코드에 비침투적

**단점**
- 코드 양이 많음
- 설정이 어려움
### Swagger
**장점**
- 적용이 쉬움
- 문서에서 바로 API 호출 가능

**단점**
- 프로덕션 코드에 침투적
- 테스트와 무관하기 때문에 신뢰도가 떨어질 수 있음


## Rest Docs 사용 방법
- gradle 설정
```gradle
plugins {  
    id 'java'  
    id 'org.springframework.boot' version '2.7.7'  
    id 'io.spring.dependency-management' version '1.0.15.RELEASE'  
    id "org.asciidoctor.jvm.convert" version "3.3.2"  //아스키독 사용
}
dependencies {  
    // Spring boot  
    implementation 'org.springframework.boot:spring-boot-starter-web'  
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'  
    implementation 'org.springframework.boot:spring-boot-starter-validation'  
  
    // test  
    testImplementation 'org.springframework.boot:spring-boot-starter-test'  
  
    // lombok  
    compileOnly 'org.projectlombok:lombok'  
    annotationProcessor 'org.projectlombok:lombok'  
  
    // h2  
    runtimeOnly 'com.h2database:h2'  
  
    // Guava  
    implementation("com.google.guava:guava:31.1-jre")  
  
    // RestDocs  
    asciidoctorExt 'org.springframework.restdocs:spring-restdocs-asciidoctor'  
    testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'  
}
ext { // 전역 변수  
    snippetsDir = file('build/generated-snippets')  
}
asciidoctor {  
    inputs.dir snippetsDir  
    configurations 'asciidoctorExt'  
  
    sources { // 특정 파일만 html로 만든다.  
        include("**/index.adoc")  
    }    baseDirFollowsSourceFile() // 다른 adoc 파일을 include 할 때 경로를 baseDir로 맞춘다.  
    dependsOn test  
}  
  
bootJar {  
    dependsOn asciidoctor  
    from("${asciidoctor.outputDir}") {  
        into 'static/docs'  
    }
```