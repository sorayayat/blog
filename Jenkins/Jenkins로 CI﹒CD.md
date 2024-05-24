
CI/CD에 대한 개요는 생략하겠다.

다양한 도구들이 많았지만 오픈 소스, java 기반이라는 점외에 널리 사용하고 있는 도구라 선정하게 되었다.


CI/CD 파이프라인은 다음과 같은 단계로 진행된다.

![[blog/img/Pasted image 20240524134431.png]]

--- 

일단 구축환경부터 정리해보면..
NAS 서버에 Proxmox라는 소프트웨어 서버를 사용했다.
각각 Jenkins와 배포할 Backend server를 구축해두었다.

![[blog/img/Pasted image 20240524104437.png]]
---




![[blog/img/Pasted image 20240524135604.png]]
- 새로운 Item을 선택

![[blog/img/Pasted image 20240524135700.png]]
- 상단에 이름과 pipeline 선택 후 ok 해준다

![[blog/img/Pasted image 20240524135929.png]]
- 배포할 코드가 있는 깃헙 주소 입력

![[blog/img/Pasted image 20240524140011.png]]
- 그리고 아래에 내용을 선택해준다.


![[blog/img/Pasted image 20240524141324.png]]
- Jenkinsfile을 이용할거라 pipeline script를 SCM으로 선택했다.
- 그리고 SCM 부분도 Git으로 선택하고 주소를 입력한다.
- Credentials 부분은 따로 등록해주고 선택해야한다.

![[blog/img/Pasted image 20240524142803.png]]
- 아래 Script Path에 Jenkinsfile을 경로를 입력해준다. 


![[blog/img/Pasted image 20240524141709.png]]
- jenkins관리 > Credentials로 가면 다음과 같은 화면이 보인다. 
- (global)을 클릭하고 다음 화면에서 add를 눌러 추가해준다.
- 깃허브 페이지에서 토큰을 발급 받는다. (검색!)

![[blog/img/Pasted image 20240524142106.png]]
- 이름과 id는 구분가능한 이름을 입력한다. 그냥 똑같이 해주면 편하다.
- password에 발급받은 토큰을 입력한다.

그리고 아까의 파이프라인 설정에서 키를 확인해보면 등록된 키를 볼 수 있다.

선택해주고 저장하면 초기 설정이 완료 된다.

그리고 중요한....!!!
plugin을 설치해줘야한다. (이 과정을 빼먹고 꽤 삽질을 했다.)

![[blog/img/Pasted image 20240524142548.png]]
- jenkins관리 > Plugin > pipeline 검색 (이미 설치되어 있어서 표시됨)
- ssh 관련 plugin도 설치
