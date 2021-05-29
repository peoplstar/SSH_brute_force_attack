# SSH_SSL
**It is illegal to practice on unauthorized systems. This is for learning, so please use a virtual machine to study.**

> SSH

*  SSH(Secure Shell)는 원격지 호스트 컴퓨터에 접속하기 위해 사용되는 인터넷 프로토콜이다. 뜻 그대로 보안 셸이다. 기존의 유닉스 시스템 셸에 원격 접속하기 위해 사용하던 텔넷은 암호화가 이루어지지 않아 계정 정보가 탈취될 위험이 높으므로, 여기에 암호화 기능을 추가하여 1995년에 나온 프로토콜이다.(SSH는 암호화 기법을 사용하기 때문에, 통신이 노출된다고 하더라도 이해할 수 없는 암호화된 문자로 보인다.) 셸로 원격 접속을 하는 것이므로 기본적으로 CLI 상에서 작업을 하게 된다. 기본 포트는 22번이다.

> SSH brute force attack

*  대부분의 암호화 방식은 이론적으로 무차별 대입 공격에 대해 안전하지 못하며, 충분한 시간이 존재한다면 암호화된 정보를 해독할 수 있따. 이번 무차별 대입 공격은 `hydra`를 이용할 것이다. 또한 `metasploit`의 `axuiliary/scanner/ssh/ssh_login` 보조 기능 모듈을 이용할 수 있다. 해당 기능은 추후에 사용해 볼 예정이다.  
*  위 공격을 위해서는 방화벽 등이 설치되었지 않으며, 업데이트가 이루어지지 않아 무방비된 환경이 있어야 한다. 즉, port 22는 항시 open 상태를 유지하여 무차별 접속이 가능해야한다.

