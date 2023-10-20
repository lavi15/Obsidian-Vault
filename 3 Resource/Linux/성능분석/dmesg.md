## dmesg
- 커널에서 발생하는 다양한 메시지들을 출력
- dmesg -T : timestamp를 확인하기 좋게 출력
- dmesg -TL | grep -i oom : oom 발생 Log를 grep

### OOME(Out Of Memory)
- 가용한 메모리가 부족해서 더이상 프로세스에 할당해 줄 메모리가 업는 상황
- OOME 상황 시 oom killer(기존 사용하는 프로세스를 죽이고 새로운 프로세스에 할당) 동작

#### OOM Killer
- 기존 사용하는 프로세스를 죽이고 새로운 프로세스에 할당
- oom_score : score가 높은 프로세스가 더 먼저 종료
- /proc/2526 : oom_score가 있는 directory

### SYN Flooding
- 공격자가 대량의 SYN 패킷만 보내서 소켓을 고갈시키는 공격

#### SYN Cookie
- SYN 패킷의 정보들을 바탕으로 쿠키를 만든 후 그 값을 SA의 시퀀스 번호로 만들어 응답
- SYN Cookie가 동작하면 SYN Backlog에 쌓이지 않아 SYN Flooding 예방 가능
- TCP Option 헤더를 무시하기 때문에 Windows Scaling등 성능 향상을 위한 옵션이 동작하지 않음
- Cookie가 true여도 항상 동작하는 것이 아닌 Flooding 의심 패킷에 대해서만 동작
- aws의 elb등에서 막아주기에 잘 활용은 하지 않음