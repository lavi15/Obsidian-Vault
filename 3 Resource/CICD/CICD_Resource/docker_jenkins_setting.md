
## Setting
#### docker install
- apt 업데이트
```Linux
apt update
```
- docker file install
```Linux
curl https://get.docker.com > docker-install.h
```
- 인스톨파일 권한 부여
```linux
chmod 755 docker-install.h
```
- docker install
```linux
./docker-install.h
```


- docker network 생성
```docker
docker network ls  << network 확인
docker network create jenkins  << jenkins 이름으로 네트워크 생성
```

- docker image pull
```docker
docker pull jenkins/jenkins:lts-jdk17    <<jenkins 이미지 가져오기
docker image ls     <<이미지 확인
```
  ✲ image download site : https://hub.docker.com/


