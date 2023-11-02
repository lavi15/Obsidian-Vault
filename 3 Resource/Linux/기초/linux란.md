
#### 1.Linux란?

> - 오픈 소스로 자유롭게 사용, 수정, 배포할 수 있는 운영 체제
> - 오랜 기간 동안 테스트와 개발이 이루어져 안정성과 신뢰성이 뛰어나다
> - Redhat 계열(ex.RHEL, CentOS)과 Devian 계열(ex.Ubuntu, Debian)으로 나누어져 있음
> - Docker, Container가 Linux 기반으로 동작

#### 2. Ubuntu란?

> - Linux 기반의 Devian 계열 운영 체제
> - 오픈 소스로 자유롭게 사용, 수정, 배포할 수 있는 운영 체제
> - GNOME 데스크톱 환경을 기본으로 제공하여 사용이 편리 하여 초보자 접근이 쉽다

#### 3. Package

> - 리눅스에서 패키지는 소프트웨어를 설치, 업데이트, 제거하는 데 사용되는 소프트웨어 단위
> - Binary Package는 다운로드 후 바로 사용이 가능하고, Source Package는 추가 Compile이 필요
> - Ubuntu Package Category  
>     - Main : 무료 및 오픈소스 (우분투 지원 O)  
>     - Universe : 무료 및 오픈소스 (우분투 지원 X)  
>     - Restricted : 법적 제한이 있는 소프트웨어 (우분투 지원 O)  
>     - Multiverce : 법적 제한이 있는 소프트웨어 (우분투 지원 X)
>     
>     > #### 3.1 deb 파일
>     > 
>     > - Devian 계열 Linux 배포판에서 사용하는 소프트웨어 패키지 파일  
>     >     ![](https://velog.velcdn.com/images/lavi15/post/ce01e569-8f11-4e5d-931e-611d79a316e4/image.png)
>     > 
>     > #### 3.2 Devian 계열 Package 관리 도구
>     > 
>     > - dpkg
>     >     - Package의 설치, 제거 등 관리 수행 (다운로드 X)
>     >     - 사용자가 의존성 문제 직접 해결 필요  
>     >         ※의존성 : 소프트웨어가 설치 및 실행되기 위해 필요한 조건
>     > - APT
>     >     - 설정 파일에 등록된 저장소에서 패키지 다운로드 후 설치 (dpkg 단점 보완)
>     > 
>     > #### 3.3 Devian 계열 Package 관리 명령어
>     > 
>     > #### 3.3.1 dpkg 명령어
>     > 
>     > - dpkg -l
>     >     - 현재 설치된 패키지 목록 출력  
>     >         ![](https://velog.velcdn.com/images/lavi15/post/3bc5f164-e549-4faf-8b07-0e840c2b0b31/image.png)
>     > - dpkg -L
>     >     - 패키지에서 설치된 파일의 목록 출력  
>     >         ![](https://velog.velcdn.com/images/lavi15/post/e7024b45-f060-4432-a25a-15dfa9563a9c/image.png)
>     > - dpkg -s
>     >     - 패키지 살세 정보 출력  
>     >         ![](https://velog.velcdn.com/images/lavi15/post/f041f93e-773d-45fa-bb92-958b553e395f/image.png)
>     > - dpkg -S
>     >     - 경로명의 파일이 포함된 패키지 출력  
>     >         ![](https://velog.velcdn.com/images/lavi15/post/e0ae1908-c52e-4778-8932-a6e473aa2780/image.png)
>     > - dpkg -i
>     >     - 해당 패키지 파일(.deb)을 설치
>     > - dpkg -r
>     >     - 해당 패키지를 삭제
>     > - dpkg -P
>     >     - 해당 패키지와 설정 정보 모두 삭제
>     > 
>     > #### 3.3.2 APT 명령어
>     > 
>     > - apt-get [옵션] [서브 명령] [패키지명]
>     >     - 소프트웨어 패키지를 관리
>     >     - 서브 명령  
>     >         - update : 패키지 저장소에서 새로운 패키지 정보를 업데이트  
>     >         - upgrade : 현재 설치된 모든 패키지를 최신 버전으로 업그레이드  
>     >         - install : 패키지 설치  
>     >         - remove : 패키지 삭제  
>     >         - autoremove : 오래되거나 불필요한 패키지를 삭제  
>     >         ![](https://velog.velcdn.com/images/lavi15/post/95964b7b-a239-414f-8779-9c24e0e5afbb/image.png)![](https://velog.velcdn.com/images/lavi15/post/15eb2ee9-c8c7-46bc-a215-4f7530cb3c12/image.png)![](https://velog.velcdn.com/images/lavi15/post/88e6ab0e-0107-4e98-8a4c-d0c6099646bb/image.png)![](https://velog.velcdn.com/images/lavi15/post/3576ddaf-3476-4e3d-ae13-daee852982cf/image.png)![](https://velog.velcdn.com/images/lavi15/post/47ae22b6-3c38-4558-964b-98a15ff6fe1a/image.png)
>     > - apt-cache [옵션] [서브 명령] [패키지명]
>     >     - 패키지 데이터베이스에서 정보를 검색하여 출력
>     >     - 서브 명령  
>     >         - search : 패키지 이름과 간단한 설명 검색  
>     >         - show : 패키지 관련 상세 정보 출력  
>     >         - showpkg : 패키지에 대한 의존성/역의존성 정보 출력  
>     >         - depends : 지정한 패키지의 의존성 목록 출력  
>     >         - rdepends : 지정한 패키지의 역의존성 목록 출력  
>     >         - policy : 패키지의 설치 상태 및 이를 포함하는 저장소 정보 출력  
>     >         - pkgnames : 사용 가능한 전체 패키지의 이름 출력  
>     >         ![](https://velog.velcdn.com/images/lavi15/post/7a523d48-29eb-4161-adfb-bbf6e62b8852/image.png)![](https://velog.velcdn.com/images/lavi15/post/8e654f5e-56a2-4e33-a88d-6386589bd440/image.png)![](https://velog.velcdn.com/images/lavi15/post/4179426e-ab84-433a-b4a0-c5c44798691e/image.png)![](https://velog.velcdn.com/images/lavi15/post/45009569-e6f2-447a-9506-311dccfe6833/image.png)![](https://velog.velcdn.com/images/lavi15/post/c6d91f61-a6d9-4d52-ab21-ee45035672d4/image.png)![](https://velog.velcdn.com/images/lavi15/post/211ede7b-2cd5-4385-ba91-1ce87dc8932a/image.png)
>     

#### 4. nano editor

> - Linux 환경에서 사용할 수 있는 editor
> - 텍스트 편집을 위해 사용
> - nano [파일명] 통해 파일을 수정 가능  
>     ![](https://velog.velcdn.com/images/lavi15/post/d6159bf2-3151-4b1a-984a-42bf1f50f85c/image.png)
> - ^는 Ctrl, M-는 Alt 키 통해 명령어 사용  
>     ex) ^G = Ctrl + g, M-U = Alt + u  
>     ![](https://velog.velcdn.com/images/lavi15/post/42688cd0-1301-4e0e-9cca-feb791c9e87a/image.png)
