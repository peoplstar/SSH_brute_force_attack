# SSH
**It is illegal to practice on unauthorized systems. This is for learning, so please use a virtual machine to study.**

> SSH

*  SSH(Secure Shell)는 원격지 호스트 컴퓨터에 접속하기 위해 사용되는 인터넷 프로토콜이다. 뜻 그대로 보안 셸이다. 기존의 유닉스 시스템 셸에 원격 접속하기 위해 사용하던 텔넷은 암호화가 이루어지지 않아 계정 정보가 탈취될 위험이 높으므로, 여기에 암호화 기능을 추가하여 1995년에 나온 프로토콜이다.(SSH는 암호화 기법을 사용하기 때문에, 통신이 노출된다고 하더라도 이해할 수 없는 암호화된 문자로 보인다.) 셸로 원격 접속을 하는 것이므로 기본적으로 CLI 상에서 작업을 하게 된다. 기본 포트는 22번이다.

> SSH brute force attack

*  대부분의 암호화 방식은 이론적으로 무차별 대입 공격에 대해 안전하지 못하며, 충분한 시간이 존재한다면 암호화된 정보를 해독할 수 있따. 이번 무차별 대입 공격은 `hydra`를 이용할 것이다. 또한 `metasploit`의 `axuiliary/scanner/ssh/ssh_login` 보조 기능 모듈을 이용할 수 있다. 해당 기능은 추후에 사용해 볼 예정이다.  
*  위 공격을 위해서는 방화벽 등이 설치되었지 않으며, 업데이트가 이루어지지 않아 무방비된 환경이 있어야 한다. 즉, port 22는 항시 open 상태를 유지하여 무차별 접속이 가능해야한다.


__공격자 IP__ : 192.168.203.128   
__대상자 IP__ : 192.168.203.130    
*(가상머신을 이용해 인접한 IP에 위치한다. 그리고 현재 두 OS는 Kail Linux를 이용하고 있다.)*

__OpenSSH-Server가 설치되어 있지 않는 경우 `apt-get install openssh*`로 설치한다.__ 

nmap 포트스캐닝으로 대상 시스템의 SSH 포트가 open인지를 확인한다.   
`nmap [대상자 IP] -sV -p 22` 
    
![ssh_nmap-bef](https://user-images.githubusercontent.com/78135526/120061661-382dff80-c099-11eb-9456-3fd4806692fd.png)

또한, 관리자의 접속 허가 혹은 불가 설정을 위해 `vi /etc/ssh/sshd_config`로 PermitRootLogin의 옵션을 주석 해제하여 ON 상태로 만든다.


![permitrootlogin](https://user-images.githubusercontent.com/78135526/120060872-395d2d80-c095-11eb-88b4-62d351f70627.png)

이 후, SSH 서버 접속을 위해 `service ssh start`로 시작 후 `service ssh status`로 **Active : active** 인지 확인한다.

패스워드가 복잡할수록 시간이 많이 소요되므로 확인용으로 password를 0123으로 변경하겠습니다.

SSH brute force attack이 성공적으로 되었는지를 확인하기 위해 대상자에 SSH.txt 파일을 만듭니다. _(이 과정을 생략해도 되겠습니다.)_

_____
## `iptables`

22번 PORT를 열기 위해서는 LINUX에서 제공하는 `iptables`를 이용해 공격자에게 해당 포트를 허용합니다.

`$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT`   
`$ sudo iptables -A INPUT -p udp --dport 22 -j ACCEPT`   
`iptables -L`를 통해 현재 22번 PORT가 어느곳에서든 허용한다는 것을 볼 수 있다. 

![iptables](https://user-images.githubusercontent.com/78135526/120061139-bb018b00-c096-11eb-8136-5bc5d63eb490.png)
____

포트스캐닝으로 대상자의 IP를 확인해보면 OPEN 인 것을 알 수 있다.

![nmap_aft](https://user-images.githubusercontent.com/78135526/120061261-585cbf00-c097-11eb-9ae1-998bcffe5693.png)

모든 준비는 끝났고, 이제 `hydra` 명령어로 무차별 대입공격을 시도한다.

(root@kali)~#`hydra -l root 4:4:1 [대상자 IP] ssh -V -f`
+ -l : 아이디
+ -L : 아이디 리스트 파일
+ -p : 비밀번호
+ -P : 비밀번호 사전 파일
+ -x 4:4:1 : 최소 4자리 : 최대 4자리 : 숫자로 되어 있는 비밀번호
+ -v : 자세히 
+ -V : login + 비밀번호 Vision
+ -f : 비밀번호 발견시 종료

`hydra -l root -x 4:4:1 192.168.203.130 ssh -V -f` 

![passwd suc](https://user-images.githubusercontent.com/78135526/120061475-66f7a600-c098-11eb-9d33-5a8759f1a12f.png)

임의로 변경했던 PASSWORD : 0123이 확인이 된다. `ssh root@[대상자 IP]`로 접속을 시도한다. 우리가 알아낸 PASSWORD를 입력하면 대상 컴퓨터에 접속이 가능해진다. 대상 컴퓨터에 작성했던 SSH.TXT를 확인해본다.

![FINLA](https://user-images.githubusercontent.com/78135526/120061567-d2417800-c098-11eb-975d-572fde79a456.png)

SSH brute force attack 성공한 것을 알 수 있다.    
이후 다운로드 : `scp 아이디@[대상자 IP]:[서버경로][로컬경로]` || 업로드 : `scp [로컬경로]아이디@[대상자 IP]:[서버경로]`를 통해 악성파일을 업로드, 실행, 주요 자료 다운 등이 가능하다.