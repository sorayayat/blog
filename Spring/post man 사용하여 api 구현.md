post man을 사용하여 api 서버를 구현 하고자 했다.

post man을 사용하여 검증 과정을 하여 백엔드를 먼저 구성하려는 계획 이였다!

간단한 회원가입 로직을 만들고 post man을 통해 값을 던졌다.

![](/img/Pasted%20image%2020240630165338.png)

??? 403.....

터미널에는 rawPassword cannot be null 에러가 발생

🛠️ 코드 수정 후 시도...실패....

구글링 후 컨트롤러에 
```@RequestBody```
어노테이션 추가 했지만 415 에러...

post man에서 param으로 시도하고 body로 했지만 같은 결과가 나왔다.

그래서 post man을 여기 저기 뜯어 보다가
값을 json으로 던져보기로 했다

![](/Img/Pasted%20image%2020240630165755.png)

![](/img/Pasted%20image%2020240630165821.png)


why? 왜? 왜됨...왜안됨 궁금증에 여기저기 뒤져보았다

문제가 뭔지 살펴보던중

spring security 설정에서 csrf를 disable 했지만, 문법상 오류가 있었는지 이쪽에서 걸린듯 하였다.

다시 수정한 후에 form-data로 송신하니 성공하였다.!

