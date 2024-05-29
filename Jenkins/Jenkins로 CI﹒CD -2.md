
배포에서 문제 되었던 부분을 다시 수정하였다.

일단 DB 연결부터 해주었다.

mysql server에 계정 먼저 설정하여 접속 할 수 있도록 해주었다.

```
// root로 접속 
mysql -u root -p[pw]

//mysql 스키마 선택 (mysql의 기본 스키마에 권한 등의 메타 정보가 있다.)
use mysql;

// 계정등록 사용자명과 접근을 허용할 ip주소이다 
// '%'를 사용하면 모두접근 및 특정 범위 ('192.168.%')를 허용 할 수 있다.
create user '사용자'@'host' identified by '비밀번호';

// 유저와 호스트를 확인해보자
select user,host from user;
```

계정을 만들었다면 권한을 부여해야한다.
```
// 모든 데이터베이스의 모든 테이블에 모든 권한을 줌
grant all privileges on *.* to '사용자'@'localhost';

특정 스키마, 테이블, 컬럼 별로도 권한을 부여 할 수 있고 
select, insert, delete 처럼 행위에 대한 권한을 부여 할 수 있다.
```

**변경한 권한을 반영해 주자**
```
FLUSH PRIVIELGES;
```

![](/img/Pasted%20image%2020240529200948.png)

외부 환경에서 접속 하려면 포트만 열어주면 어디서든 접속 할 수 있다!

혹시나 안된다면.....(나의 경우 그랬다 물론 다른 문제도 너무 많았다.)

```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```


외부 접속을 위해서 바꿔 줘야한다.
bind-address = 127.0.0.1로 되어 있는 부분을 다음과 같이 바꿔준다.

![](/img/Pasted%20image%2020240529202137.png)




프로젝트의 yaml, properties에 mysql 서버 주소와 계정 비밀번호를 입력해준다.

```
// 예제 yaml

spring:  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: jdbc:mysql://[sql서버ip]:[port]/[DB명]  
    username: Backend  
    password: [계정 비밀번호]
```

--- 

이제 젠킨스로 돌아가보자!

몹시..황당하게도 jenkinsfile 경로 부분에  ``` \ ``` 역슬래쉬로 되어 있었다ㅠㅠ..
문법적인 오류가 있었던 부분을 싹 다 수정하고 다시 빌드 시도


👍


![](/img/Pasted%20image%2020240529193015.png)

![](/img/Pasted%20image%2020240529193100.png)


삽질을 정리해 보자면...

1. 플러그인을 먼저 확인 안해서 멀리 돌아갔다.
2. mysql 계정 등록 할때 아무 생각없이 이름을 root로 했다가 접속 자체가 안되서 강제로 모든 접속을 허용해서 해결했다.......~~나란....똥멍....~~
