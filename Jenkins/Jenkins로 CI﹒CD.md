
CI/CD에 대한 개요는 생략하겠다.

다양한 도구들이 많았지만 오픈 소스, java 기반이라는 점외에 널리 사용하고 있는 도구라 선정하게 되었다.


CI/CD 파이프라인은 다음과 같은 단계로 진행된다.


![Alt text](/img/Pasted%20image%2020240524134431.png)


--- 
### 서버 설정
일단 구축환경부터 정리해보면..
NAS 서버에 Proxmox라는 소프트웨어 서버를 사용했다.
각각 Jenkins와 배포할 Backend server를 구축해두었다.

![](/img/Pasted%20image%2020240524104437.png)

---


![Alt text](/img/Pasted%20image%2020240524135604.png)

- 새로운 Item을 선택
---

![](/img/Pasted%20image%2020240524135700.png)
- 상단에 이름과 pipeline 선택 후 ok 해준다

---

![](/img/Pasted%20image%2020240524135929.png)
- 배포할 코드가 있는 깃헙 주소 입력

![](/img/Pasted%20image%2020240524140011.png)
- 그리고 아래에 내용을 선택해준다.


![](/img/Pasted%20image%2020240524141324.png)
- Jenkinsfile을 이용할거라 pipeline script를 SCM으로 선택했다.
- 그리고 SCM 부분도 Git으로 선택하고 주소를 입력한다.
- Credentials 부분은 따로 등록해주고 선택해야한다.
![](/img/Pasted%20image%2020240524142803.png)
- 아래 Script Path에 Jenkinsfile을 경로를 입력해준다. 


![](/img/Pasted%20image%2020240524141709.png)
- jenkins관리 > Credentials로 가면 다음과 같은 화면이 보인다. 
- (global)을 클릭하고 다음 화면에서 add를 눌러 추가해준다.
- 깃허브 페이지에서 토큰을 발급 받는다. [[blog/etc/Obsidian - github 사용하기|Obsidian - github 사용하기]]
- 

![](/img/Pasted%20image%2020240524142106.png)
- 이름과 id는 구분가능한 이름을 입력한다. 그냥 똑같이 해주면 편하다.
- password에 발급받은 토큰을 입력한다.

그리고 아까의 파이프라인 설정에서 키를 확인해보면 등록된 키를 볼 수 있다.
ssh로 jar 파일을 전송하여 실행할 예정이라 서버에서 ssh키 발급과 등록도 함께했다.

선택해주고 저장하면 초기 설정이 완료 된다.

그리고 중요한....!!!
plugin을 설치해줘야한다. (이 과정을 빼먹고 꽤 삽질을 했다.)

![](/img/Pasted%20image%2020240524142548.png)
- jenkins관리 > Plugin > pipeline 검색 후 설치
- ssh 관련 plugin도 설치

Jenkinsfile을 작성하면 된다.

```
pipeline{
	agent any
	stages('Checkout') {
		steps {
			git credentialsId: 'github_Token', url: ['깃헙주소']
		}
		// 다음과 같이 메세지도 출력해 볼 수 있다.
		post {
	                success {
	                    sh 'echo "Success clone Repo"'
	                }
	                failure {
	                    sh 'echo "Fail clone Repo"'
	                }
	            }
	}
	
	 stage('Build') {
	            steps {
	                // Gradle을 사용하여 프로젝트 빌드
	                sh 'chmod +x ./gradlew'  //권한 문제로 추가해주었다.
	                sh './gradlew build'
	                sh './gradlew clean build'
	            }
	            post {
	                success {
	                    sh 'echo "Success build"'
	                    // 빌드된 파일 목록 확인
	                    sh 'ls -l build/libs/'
	                }
	                failure {
	                    sh 'echo "Fail build"'
	                }
	            }
	        }

	// 수정중....
	stage('Deploy') {
		steps {
			// 빌드된 JAR 파일을 원격 서버로 전송
			sshagent(credentials: [SSH_CREDENTIALS_ID]) {
				sh """
				ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} "pwd"
				scp -o StrictHostKeyChecking=no build/libs/${JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}
				"""
			// 원격 서버에서 기존 프로세스를 종료하고 새 JAR 파일로 애플리케이션을 시작
				sh """
				ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} "pkill -f 'java -jar ${REMOTE_DIR}/${JAR_NAME}' || true"
				ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} "nohup $JAVA_HOME/bin/java -jar ${REMOTE_DIR}/${JAR_NAME} > /dev/null 2>&1 &"
				"""
			}
		}
            post {
                success {
                    sh 'echo "Success deploy"'
                }
                failure {
                    sh 'echo "Fail deploy"'
                }
            }
        }

	//연결 상태 확인
	stage('Health Check') {
		steps {
			script {
				def response = httpRequest url: 'http://192.168.0.14:8200/health', validResponseCodes: '200'
				if (response.status != 200) {
					error "Health check failed with status: ${response.status}"
				} else {
					sh 'echo "Health check passed"'
				}
			}
		}
	post {
		always {
			// 빌드 결과를 정리
			cleanWs()
		}
    }
}
```

여기까지 작성했다면 빌드해보면 된다

![](/img/Pasted%20image%2020240526090520.png)

물론... 나는 에러가 났다.

콘솔 메세지를 확인해 보면 파이프라인의 단계별로 잘 실행이 되는 것을 볼 수 있다.
![](/img/Pasted%20image%2020240526090902.png)
post했던 메세지도 잘 나온다

![](/img/Pasted%20image%2020240526091129.png)

아직 해결중이다....

우선 파일이 제대로 전송 되었는지 서버에서 확인해 보자
![](/img/Pasted%20image%2020240526091439.png)

젠킨스 서버에서 원격 서버 진입 후 파일이 잘 들어온 것을 확인 할 수 있다.

배포 문제 해결 부분은 추후 계속 다루겠다.
