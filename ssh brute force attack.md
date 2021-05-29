# SSH_SSL
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

`nmap` 포트스캐닝으로 대상 시스템의 SSH 포트가 open인지를 확인한다.   
`nmap [대상자 IP] -sV -p 22` 
   
![ssh_nmap-bef](https://user-images.githubusercontent.com/78135526/120059532-2cd4d700-c08d-11eb-9ed2-9a878f04904b.png)