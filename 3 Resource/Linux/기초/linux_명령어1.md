

#### 1. Linux File

> - 파일의 종류
>     - 일반파일
>         - 텍스트 파일 : 메모장으로 열어 볼 수 있는 파일
>         - 바이너리(이진) 파일 : 실행 파일, 미디어/이미지 파일 등
>     - 디렉토리
>         - 파일 및 디렉토리를 계층 구조로 구성하여 보관
>         - 경로 제공
>     - 파일 링크
>     - 장치 파일

#### 2. Linux 명령어

> - wget [옵션] [URL]
>     - URL에서 파일을 download  
>         ![](https://velog.velcdn.com/images/lavi15/post/281a86db-ac8a-4e90-8bc0-b2d03a822c68/image.png)
> - curl [옵션] [URL]
>     - URL로 HTTP 등의 통신 수행  
>         ![](https://velog.velcdn.com/images/lavi15/post/dafc4798-3257-40d1-bc9f-d43d28c70ac5/image.png)
> - tar [기능/옵션] [아카이브 파일] [파일 or 디렉토리명]
>     - 파일과 디렉토리를 하나의 파일로 묶어 아카이브(tar파일) 생성
>         
>         ##### ※아카이브(archive) : 여러 파일이나 디렉토리를 하나의 단일 파일로 묶는 것
>         
>     - 주요기능
>         - c : tar 파일을 생성
>         - t : tar 파일의 내용 출력
>         - x : tar 파일에서 원본 파일 추출
>         - u : tar 파일 내의 수정된 내용 업데이트
>         - r : tar 파일 내에 새로운 파일 추가
>     - 주요 옵션
>         - f : tar 파일 이름을 지정
>         - v : 명령 실행 시 대상 파일을 출력
>         - j : bzip2로 압축 또는 해제
>         - g : gzip로 압축 또는 해제
>         - C : 디렉토리를 변경  
>             ![](https://velog.velcdn.com/images/lavi15/post/847ce5b6-86fe-414b-baa3-7660c19f5db5/image.png)![](https://velog.velcdn.com/images/lavi15/post/b026f646-b411-4df1-9cab-f15bb5b94d7c/image.png)![](https://velog.velcdn.com/images/lavi15/post/628837a3-371f-450a-ab52-136aab019c71/image.png)![](https://velog.velcdn.com/images/lavi15/post/b62462c4-44d8-4e3a-9d93-a639f5c0dd6a/image.png)![](https://velog.velcdn.com/images/lavi15/post/67222c50-9fca-4a5b-9fd5-9fbf0ac7f468/image.png)
> - gzip [옵션] [파일명]
>     - gzip 형식으로 파일 압축  
>         ![](https://velog.velcdn.com/images/lavi15/post/e3ffd98d-84d4-41b6-828d-3636ffccae51/image.png)
> - gunzip [옵션] [파일명]
>     - gzip 파일 압축 해제  
>         ![](https://velog.velcdn.com/images/lavi15/post/b8df4502-718b-4473-874d-3d28ab18fb59/image.png)
> - bzip2 [옵션] [파일명]
>     - bzip2 형식으로 파일 압축  
>         ![](https://velog.velcdn.com/images/lavi15/post/3ae3ccc6-3ed1-479a-b942-d57993d077a4/image.png)
> - bunzip2 [옵션] [파일명]
>     - bzip2 형식으로 파일 압축  
>         ![](https://velog.velcdn.com/images/lavi15/post/e49cdeb6-11da-4515-a822-262d412ee1f4/image.png)