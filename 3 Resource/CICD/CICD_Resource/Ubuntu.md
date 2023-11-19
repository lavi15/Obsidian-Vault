
docker download
```Linux
apt-get update

curl https://get.docker.com > docker-install.h
chmod 755 docker-install.h
./docker-install.h
```

docker 기본 명령어
``` docker
docker run helloworld  //해당 파일이 있는지 확인후 없으면 생성

docker ps -a   // container 확인

docker rm a0cd6b6daab9 //container 삭제
```

jenkins
- jenkins image install
```docker
docker run -d -p 8080:8080 -p 50000:50000 --name docker-jenkins --restart=on-failure jenkins/jenkins:lts-jdk17
```
- jenkins container bash 접근
``` docker
docker run -d -p 8080:8080 -p 50000:50000 --name docker-jenkins --restart=on-failure jenkins/jenkins:lts-jdk17  //jenkins container 생성 및 실행

docker exec -it docker-jenkins bash  //jenkins 배쉬 접근

docker logs docker-jenkins  //jenkins password를 넘겼을때 로그를 통해 확인
```

브라우저에서 젠킨스 실행
- 8080 port

젠킨스 java 설정
- Jenkins 관리 > Tools > JDK add
```docker
docker inspect docker-jenkins | grep jdk //docker 스펙확인
```
![[Pasted image 20231017174225.png]]

젠킨스 Maven Plugin 설치 및 설정
- Jenkins 관리 > Plugins > Available Plugin > 'Maven Integration' Install
- Jenkins 관리 > Tools > Maven installations
![[Pasted image 20231017175304.png]]

Maven Project 생성
- 새로운 item > Maven Project >  Git Repository 추가 > Build
![[Pasted image 20231018104718.png]]
![[Pasted image 20231018104813.png]]

Jenkins에 Tomcat 설치
- Jenkins 관리 > Plugins > Available Plugin > 'Deploy to container' Install
- Linux server
```Linux
docker run -d -it --name tomcat -p 8090:8080 tomcat:9
docker exec -it tomcat bash //tomcat에 bash 쉘로 접근

cd webapps.dist/
cp -R * ../webapps     //webapps-dist의 모든 파일을 webapps로 복사

cd ../webapps/manager/META-INF
apt-get update  //tomcat 서버의 update
apt-get install vim  //vim down
vi context.xml   //context.xml 수정(보안해제)

cd /usr/local/tomcat/conf
vi tomcat-users.xml //tomcat-users.xml 수정(유저 생성)
```

context.xml 수정사항
```xml
<!-- 아래의 내용을 주석처리 -->
<Valve className=""~~~>

```

**tomcat-users.xml 수정사항**
```xml
<!-- <users>안에 삽입 -->

<role rolename="manager-gui" /> 
<role rolename="manager-script" />
<role rolename="manager-jmx" /> 
<role rolename="manager-status" />
<user username="admin" password="111" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
<user username="deployer" password="111" roles="manager-script"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
```

build
- 프로젝트 > 구성 > 빌드 후 조치 > Deploy war/ear to a container
![[Pasted image 20231018121630.png]]![[Pasted image 20231018122110.png]]

Web Server 접근
- http://175.45.202.4:8090/web/

**git push시 자동 Build**
- Jenkins 설정
- 프로젝트 > 구성 > 빌드 유발 > GitHub hook trigger for GITScm polling
![[Pasted image 20231018123155.png]]

- github 설정
- 프로젝트 > settings > Webhooks
- ![[Pasted image 20231018123331.png]]![[Pasted image 20231018123557.png]]