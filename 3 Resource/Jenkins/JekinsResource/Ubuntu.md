
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
``` docker
docker run -p 8080:8080 -p 50000:50000 --name docker-jenkins --restart=on-failure jenkins/jenkins:lts-jdk17.  //jenkins download

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
- Jenkins 관리 > Plugins > Available Plugin > Maven Install
- Jenkins 관리 > Tools > Maven installations
![[Pasted image 20231017175304.png]]



