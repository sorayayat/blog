
## Spring Security란


Spring Security는 강력하고 사용자 정의가 가능한 인증 및 액세스 제어 프레임. 이는 Spring 기반 애플리케이션 보안을 위한 사실상의 표준입니다.

Spring Security는 Java 애플리케이션에 인증과 권한 부여를 모두 제공하는 데 중점을 둔 프레임워크입니다. 모든 Spring 프로젝트와 마찬가지로 Spring Security의 진정한 힘은 사용자 정의 요구 사항을 충족하기 위해 얼마나 쉽게 확장할 수 있는지에 있습니다.

### 특징

- 인증 및 권한 부여에 대한 포괄적이고 확장 가능한 지원

- 세션 고정, 클릭재킹, 크로스 사이트 요청 위조 등과 같은 공격으로부터 보호

- 서블릿 API 통합

- Spring Web MVC와의 선택적 통합


요청을 가로채서(인터셉트) filter를 거쳐 설정에 맞게 사용자의 권한 및 접근을 설정 할 수 있다.





### 구현

**특이 사항**

버전 별로 구현하는 방식이나 메소드들이 조금씩 바뀐다. 
자세한 내용은 공식문서나 제공하는 깃헙에서 확인하시길.
- 이 글은 3.2.4로 작성됨

gradle에 의존성을 추가한다.

```implementation 'org.springframework.boot:spring-boot-starter-security'```


security 설정을 관리할 패키지를 만들고 클래스를 생성한다

클래스명은 자유롭게 작성해도 된다


```
@Configuration
@EnableWebSecurity // spring security에서 클래스를 인식하고 관리한다.
public class SecurityConfig {

	@Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		return http;
	}
}
```


filterChain라는 인터페이스를 가지고 메소드를 작성한다. 이때 인자는 HttpSecurity라는 클래스를 받아서 사용한다.
그리고 http를 리턴해준다.

람다식을 사용하여 작성해보자

```
@Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		http
			.authorizeHttpRequests((auth) -> auth
				.requestMatchers("/", "login").permitAll()
			);
			
		return http.build();
		
		}
```




requestMatchers라는 메소드를 사용하여 해당 경로에 대한 여러가지 옵션을 컨트롤 할 수 있다.
![[blog/img/Pasted image 20240417181224.png]]

| method        | 기능                                 |
| ------------- | ---------------------------------- |
| hasRole       | 사용자의 역할에 따라 권한 설정이 가능하다 (관리자, 사용자) |
| permitAll     | 모든 사용자에게 접근허용                      |
| authenticated | 로그인시에 접근 허용                        |
| denyAll       | 모든 사용자가 접근하지 못하도록 한다               |

다음 내용을 추가한다.
```
.formLogin((formLogin)-> formLogin
				.loginPage("/login")
				.usernameParameter("username")
				.passwordParameter("password")
				.loginProcessingUrl("/login/login-proc")
				.defaultSuccessUrl("/home")
				.failureUrl("/login"))
		.logout((logout) -> logout
				.logoutSuccessUrl("/")
				.invalidateHttpSession(true)
                )
```














>참고자료 - https://spring.io/projects/spring-security
>youtube - https://youtu.be/y0PXQgrkb90?si=m9tQz_PYppu6F2dy