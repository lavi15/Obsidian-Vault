
1. 도커 컨테이너에서 사용할 브릿지 네트워크 생성
```
docker network create jenkins  << jenkins 이름으로 네트워크 생성
```

2. 젠킨스 이미지 가져오기
```
docker pull jenkins/jenkins:lts-jdk17
```

3. jenkins 폴더 생성 및 접근
```
mkdir jenkins
cd ./jenkins
```

4. install-dcoker.sh 만들기
```
#!/bin/sh 
apt-get update 
apt-get -y 
install apt-transport-https \
apt-utils \
ca-certificates \
curl \
gnupg2 \
zip \
unzip \
acl \
software-properties-common 

curl -fsSL [https://download.docker.com/linux/](https://download.docker.com/linux/)$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey;

apt-key add /tmp/dkey add-apt-repository \
"deb [arch=amd64] [https://download.docker.com/linux/](https://download.docker.com/linux/)$(. /etc/os-release; echo "$ID") \
$(lsb_release -cs) \ stable" && \ apt-get update apt-get -y install docker-ce
```

5. Dockerfile 만들기
```
FROM jenkins/jenkins:lts-jdk17 

USER root

COPY [install-docker.sh](http://install-docker.sh/) /install-docker.sh
RUN chmod +x /install-docker.sh 
RUN /install-docker.sh

RUN usermod -aG docker jenkins
RUN setfacl -Rm d:g:docker:rwx,g:docker:rwx /var/run/ 

USER jenkins
```


6. Docker이미지 생성
```
docker run --privileged -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --restart=on-failure --network="jenkins" --name docker-jenkins jenkins/jenkins:lts-jdk17
```

**✲docker-jenkins 컨테이너를 다시 생성시 삭제 필요 (jenkins_home도 제거 필요)**
```
docker container stop docker-jenkins
docker container rm docker-jenkins
docker volume stop docker-jenkins
```

1. build.gradle 설정
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