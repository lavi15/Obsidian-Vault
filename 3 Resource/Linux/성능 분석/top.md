
- 프로세스들의 상태와 CPU, Memory 사용률 확인
![[Pasted image 20231020160504.png]]
- us(user) : 프로세시의 일반적인 CPU 사용량
- wa(waiting) : I/O 작업을 대기할 때의 CPU 사용량 


### hot key
- 1 : 평균이 아닌 각각의 cpu 마다의 사용률 확인(Default : 평균)
![[Pasted image 20231020160657.png]]

- d : 인터벌 변경(Default : 3초)

### 프로세스의 상태
- D : uninterruptible sleep **(I/O)**
- R : running **(CPU)**
- S : sleeping **(작업하지 않는 상태)**
- Z : zombie ()