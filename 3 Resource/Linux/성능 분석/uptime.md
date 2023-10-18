## uptime
- 시스템의 가동 시간과 로그인한 사용자 수, Load Average 확인
![[Pasted image 20231005111006.png]]
- 0.17 : 1분 평균
- 0.05 : 5분 평균
- 0.01 : 15분 평균

### Load Average
- 단위시간 동안의 R(CPU 위주의 작업)과 D(I/O 위주의 작업) 상태의 프로세스 개수
- Load Average는 상대적인 값
- vmstat : R, D 상태의 프로세스 수 확인 가능 
- R 상태의 프로세스가 많을 시 : CPU 개수를 늘리거나 스래드 수 조절
- D 상태의 프로세스가 많을 시 : IOPS가 높은 디바이스로 변경하거나 처리량을 줄임
![[Pasted image 20231005112301.png]]
![[Pasted image 20231005112504.png]]

✲ lscpu -e : cpu 수 확인 가능




, dmesg, free, df, top, netstart