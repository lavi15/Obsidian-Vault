
- 네트워크 연결 정보 확인 (sudo netstat -napo)

![[Pasted image 20231020223001.png]]

- 10.1.10.13:22 <> 1.240.235.98:51566 간 연결되어 있음
- PID 2528번이 쓰고 있으며 sshd임
![[Pasted image 20231020224138.png]]

![[Pasted image 20231022004500.png]]

### 소켓의 상태
- LISTEN
	- 포트가 열려 있는 상태
	- 80port가 nginx로 LISTEN 상태![[Pasted image 20231020224459.png]]

- TIMEWAIT
	- **keepalive_timeout**으로 인해 끊겼을 때 발생
	- cat /etc/nginx/nginx.conf | grep -i keepalive로 nginx 설정 확인 가능
	- 커넥션을 새로 맺지 않아도 해당 시간 동안 요청을 기다림
	- 요청을 기다려도 패킷이 오지 않으면 nginx가 **먼저 연결 끊음**

- CLOSE_WAIT
	- CLOSE_WAIT은 LAST_WAIT으로 넘어가지 못하고 끊겼을 때 발생
	- 소켓을 정리하는 등 연결을 끊기 위한 **정상 동작을 하지 못함**을 의미
	- CLOSE_WAIT은 **자연적으로 사라지지 않으므로** 발견시 반드시 확인 필요