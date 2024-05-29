## 중복 회원 방지

1.Service에서 회원 이름만을 받아서 중복되는지 확인하는 메소드를 작성한다.

```
@Override
    public boolean isUsernameAvailable(String username) {
        // 해당 username을 가진 회원이 존재하는지 조회
        Optional<Member> existingMember = memberRepository.findByUsername(username);
        return existingMember.isPresent();
    }
```

isPresent()는 값이 있다면 true를 반환해준다.


2.Controller

```
@GetMapping("/signup/checkUsername/{username}")
    public ResponseEntity<String> checkUsernameExists(@PathVariable String username) {
        boolean isAvailable = memberService.isUsernameAvailable(username);
        if(!isAvailable) {
            return ResponseEntity.ok("사용 가능한 사용자명입니다.");
        } else {
            return ResponseEntity.ok("이미 사용 중인 사용자명입니다. 다른 이름을 시도해주세요.");
        }
    }
```

GetMapping을 통해 http get 요청을 처리하며,
"/signup/checkUsername/{username}" 동적 경로로 들어오는 요청을 해당 메서드로 라우팅한다.

@PathVariable이라는 경로 변수로 사용자 이름을 받아와 처리한다.
반환 값은 ResponseEntity 문자열 형태의 응답을 한다.

boolean isAvailable = memberService.isUsernameAvailable(username);
중복 여부를 받아온다.
처음에 boolean 값으로 클라이언트에게 보냈을때 생각대로 동작되지 않는 문제가 있어서 문자열로 반환하도록 수정했다.




3.html (클라이언트)

사용자에게 이름과 비밀번호를 입력 받는다.

```
<div class="container">
    <h1>Sign Up</h1>
    <form id="signupForm" th:action="@{/signup}" method="post" onsubmit="return validateForm()">
        <label for="usernameInput">Username:</label>
        <input type="text" id="usernameInput" name="username">
        <div id="availabilityMessage"></div>
        <div><label>Password: <input type="password" name="password"/></label></div>
        <div><input type="submit" id="Register" value="Register"/></div> 
    </form>
</div>
```

중복 검증 부분은 jquery를 사용하였는데 이 부분을 많이 접하지 못하여 더 공부해야할것 같다.

```
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
    $(document).ready(function(){
        $('#usernameInput').on('input', function(){
            var username = $(this).val().trim();
            $.ajax({
                type: 'GET',
                url: '/signup/checkUsername/' + username,
                success: function (data) {
                    if (data === "사용 가능한 사용자명입니다.") {
                        $('#availabilityMessage').text(data);
                        $('#Register').prop('disabled', false); 
                    } else {
                        $('#availabilityMessage').text(data);
                        $('#Register').prop('disabled', true); 
                    }
                },
                error: function () {
                    $('#availabilityMessage').text('서버에서 사용자명을 확인하는 중 오류가 발생했습니다.');
                }
            });
        });
    });
```

라이브러리를 받은 후 스크립트를 보자.
사용자가 이름을 입력하면 ajax가 동작한다. 

type : GET 으로 데이터를 가져온다

url 부분은 서버 컨트롤러와 통신하는 부분이며 입력받은 이름으로 동적으로 추가된다.

success: function (data){...} 요청이 성공했을때 실행할 함수 부분이다.
중복된 이름일때 가입 버튼을 막아버렸다.

유효성 부분은 나중에 보강하도록하고 빈칸으로 제출 할 수 없도록 간단한 함수만 추가해주었다.


![](Pasted%20image%2020240529192155.png)