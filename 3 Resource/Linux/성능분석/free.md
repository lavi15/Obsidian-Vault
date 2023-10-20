## free
- 메모리 사용 현황 확인
- free -m : megabyte 단위로 표시

#### free vs available
- free : 어느 누구도 사용하고 있지 않은 메모리
- available : 어플리케이션에 실질적으로 할당 가능한 메모리
- buff : 블록 디바이스가 가지고 있는 블록 자체에 대한 캐시
- cache : I/O 성능 향상을 위해 사용하는 페이지 캐시
- 메모리 필요시 buff/cache 영역을 해제하고 어플리케이션이 사용할 수 있는 영역으로 바꿈
- available = free + buff/cache
#### I/O 성능 향상
- 한 번 읽었던 파일은 페이지 캐시에 저장 함으로써 블록 디바이스가 아닌 메모리에서 파일의 내용을 가져옴