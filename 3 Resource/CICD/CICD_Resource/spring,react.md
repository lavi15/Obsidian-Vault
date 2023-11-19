
## 1. 도커 컨테이너에서 사용할 브릿지 네트워크 생성
```
docker network ls  << network 확인
docker network create jenkins  << jenkins 이름으로 네트워크 생성
```

## 2. 젠킨스 이미지 가져오기
```
docker pull jenkins/jenkins:lts-jdk17    <<jenkins 이미지 가져오기
docker image ls     <<이미지 확인
```


## 3. 도커 이미지 만들기
### 3-1 jenkins 폴더 생성 및 접근
```
mkdir jenkins
cd ./jenkins
```

### 3-2 install-docker.sh 생성
```
#!/bin/sh
apt-get update

apt-get -y install apt-transport-https \
apt-utils \
ca-certificates \
curl \
gnupg2 \
zip \
unzip \
acl \
software-properties-common

curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey
apt-key add /tmp/dkey

add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable"
apt-get update

apt-get -y install docker-ce
```

### 3-3 Dockerfile 생성
```
FROM jenkins/jenkins:lts-jdk17 

USER root

COPY install-docker.sh /install-docker.sh
RUN chmod +x /install-docker.sh 
RUN /install-docker.sh

RUN usermod -aG docker jenkins
RUN setfacl -Rm d:g:docker:rwx,g:docker:rwx /var/run/ 

USER jenkins
```


### 3-4 Docker이미지 생성
```
docker build -t [nickname/project]:[version] .
//ex) docker build -t showg100/chapter05-docker:1.0 .
```


### 3-5 Docker login
```
docker login
```

### 3-6 DockerHub Push
```
docker push showg100/chapter05-docker:1.0
```

### 3-7 Docker 컨테이너 생성 및 실행
```
docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --restart=on-failure --network="jenkins" --name docker-jenkins showg100/chapter05:1.0
```

**✲docker-jenkins 컨테이너를 다시 생성시 삭제 필요 (jenkins_home도 제거 필요)**
```
docker container stop docker-jenkins
docker container rm docker-jenkins
docker volume stop docker-jenkins
```

### 3-8 jenkins 접속
```
docker exec -itu 0 docker-jenkins bash  //root 계정으로 터미널 접속
docker logs docker-jenkins //패스워드 확인
```


## 4. Jenkins 설정
### Nodejs Plugin install
- jenkins 관리 > Plugin
![[Pasted image 20231115114125.png]]
### Tools 설정
- jenkins 관리 > Tools
- java 설정 
![[Pasted image 20231115115616.png]]
- gradle 설정
![[Pasted image 20231115115702.png]]
- node 설정
![[Pasted image 20231115115732.png]]

### build 세팅
- new item > Freestyle project
- github 연결(password : gittoken)
![[Pasted image 20231115115921.png]]
![[Pasted image 20231115115939.png]]


**git push시 자동 Build**
- Jenkins 설정
- 프로젝트 > 구성 > 빌드 유발 > GitHub hook trigger for GITScm polling
![[Pasted image 20231116101227.png]]
![[Pasted image 20231116101252.png]]
![[Pasted image 20231116101525.png]]
```
clean build
docker build -t y showg100/hello-docker:1.0 .
docker login -u [DockerHub_id] -p [DockerHub_passwd] docker.io
docker push showg100/hello-docker:1.0
```

- github 설정
- 프로젝트 > settings > Webhooks
- ![[Pasted image 20231018123331.png]]![[Pasted image 20231018123557.png]]

젠킨스 환경설정 파일 접근 허가 설정
- 젠킨스 docker에서 실행
```
ls -l /var/run/docker.sock
chmod 666 /var/run/docker.sock
```

SSH key 생성
- jenkins docker에서 ssh key 생성
- -f : 뒤의 위치에 ssh-key 생성
```
ssh-keygen -t rsa -C "docker-jenkins-key" -m PEM -P "" -f /root/.ssh/docker-jenkins-key
```
- 생성된 public ssh-key를 linux로 복사
```
docker cp docker-jenkins:/root/.ssh/docker-jenkins-key.pub ./docker-jenkins-key.pub
```
- authorized_keys 스프링부트 서버에 생성(공개키 붙여넣기)

### Jenkins SSH-key 설정
- plug in 설정 (Publish Over SSH install)
![[Pasted image 20231116110306.png]]
- Jenkins 관리 > System




## 5. build.gradle
- Gradle을 이용해서 프로젝트 빌드하기
- springboot 프로젝트와 react 프로젝트를 하나로 묶을수 있도록 build 구성
- gradle을 이용해 프로젝트를 build 하는 경우 task가 서로 의존 관계를 가지기 떄문에 processResources를 기점으로 installReact > buildReact > processResources 실행

```gradle
plugins {
	id "com.github.node-gradle.node" version "4.0.0"
}

//React build 설정  
def reactAppDir = "${projectDir}/src/main/webapp"  
  
processResources {  
    // task간의 의존성 정의  
    // processResources task 실행되기 전에 copyReactFile task를 먼저 실행한다.  
    dependsOn "copyReactFile"  
}  
  
task copyReactFile(type: Copy) {  
    dependsOn "buildReact"  
    from "$reactAppDir/build"  
    into "$projectDir/src/main/resources/static/"  
}  
  
task buildReact(type: Exec) {  
    dependsOn "installReact"  
    workingDir "$reactAppDir"  
    //$reactAppDir 디렉토리의 변경 사항을 감지하고, 변경이 있을 경우에만 task가 실행되도록 한다.  
    inputs.dir "$reactAppDir"  
    group = BasePlugin.BUILD_GROUP  
    if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {  
        commandLine "npm.cmd", "run-script", "build"  
    } else {  
        commandLine "npm", "run-script", "build"  
    }  
}  
  
task installReact(type: Exec) {  
    workingDir "$reactAppDir"  
    inputs.dir "$reactAppDir"  
    group = BasePlugin.BUILD_GROUP  
    if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {  
        commandLine "npm.cmd", "audit", "fix"  
        commandLine "npm.cmd", "install"  
    } else {  
        commandLine "npm", "audit", "fix"  
        commandLine "npm", "install"  
    }  
}
```

```
//그래들 빌드하기
./gradlew build
```