---
title: 📖 Spring Security in Action (진행중)
author: Rosie Yang
date: 2023-04-14
category: study
layout: post
---

> 책의 모든 내용이 아닌 기억할 내용 + 추가 조사한 내용을 위주로 정리한 포스팅입니다.

### Contents
<table>
    <tr>
        <td style="width:30%;">
            <img src="/assets/gitbook/post_images/spring/spring_security.jpg">
        </td>
        <td>
            <ul>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#spring-security-5에서-추가된-새로운-기능">Spring Security 5에서 추가된 새로운 기능</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#1장-오늘날의-보안">1장. 오늘날의 보안</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#2장-안녕-스프링-시큐리티">2장. 안녕! 스프링 시큐리티</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#3장-사용자-관리">3장. 사용자 관리</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#4장-암호처리">4장. 암호처리</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#5장-인증-구현">5장. 인증 구현</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#6장-실전-작고-안전한-웹-애플리케이션">6장. 실전: 작고 안전한 웹 애플리케이션</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#7장-권한-부여-구성-액세스-제한">7장. 권한 부여 구성: 액세스 제한</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#8장-권한-부여-구성-제한-적용">8장. 권한 부여 구성: 제한 적용</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#9장-필터-구현">9장. 필터 구현</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#10장-csrf-보호와-cors-적용">10장. CSRF 보호와 CORS 적용</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#11장-실전-책임의-분리">11장. 실전: 책임의 분리</a></li>
            </ul>
        </td>
    </tr>
</table>

<br><br>

### Spring Security 5에서 추가된 새로운 기능
**OAuth 2.0 지원 개선**
+ OAuth 2.0 클라이언트 지원 개선
+ OAuth 2.0 리소스 서버 지원 개선
+ JWT(JWT Token) 지원 개선
+ JDK 9 모듈 시스템 지원

**WebFlux 지원**
+ Spring WebFlux를 사용하는 애플리케이션에서 Spring Security를 사용할 수 있습니다.

**PasswordEncoder 인터페이스 변경**
+ PasswordEncoder 인터페이스가 변경되어 더욱 유연한 비밀번호 암호화 구현이 가능해졌습니다.

**Deprecated 클래스 및 메서드 제거**
+ AuthenticationProvider 인터페이스의 Deprecated 및 제거
+ RememberMeAuthenticationProvider 클래스의 Deprecated 및 제거 등
   
<br><br>

## 1장. 오늘날의 보안
스프링 시큐리티는 아파치 2.0 라이선스에 따라 릴리스되는 오픈 소스 소프트웨어입니다. 스프링 프레임워크와 함께 애플리케이션 단위의 보안개발을 도와주며 스프링의 방식인 <span style="background-color:#fff5b1">어노테이션, 빈, SpEL(Spring Expression Language)</span> 등을 이용합니다.
+ 아파치 시로
+ GDRR(Genernal Data Protection Regulations, 일반 데이터 보호 규정)

보안에 대한 부분은 시스템의 아키텍쳐적인 성격, 모놀로식인지 아니면 여러 애플리케이션으로 구선된 시스템인 마이크로서비스 패턴인지에 대해서 고려해야 합니다. 그리고 인증과 권한 부여 부분에서 필요 이상의 권한이 부여될 수 없도록 설정하는 것이 중요합니다. 이외에도 데이터 보안시에 전송중인 데이터와 저장된 데이터의 보안, 내부 메모리 관리, 힙 덤프의 이용관리 등 다방면으로 애플리케이션의 정보 보안을 위해 고민할 필요가 있습니다.  
여기서 살펴보기 좋은 <span style="background-color:#fff5b1">오픈소스 웹 보안 프로그램으로 OWASP(Open Web Application Security Project)</span>가 있는데, 10대 보안 취약성에 대한 부분들을 다루고 있고 세부 항목으로는 인증 취약성, 세션 고정, XSS, CSRF, 주입, 기밀 데이터 노출, 메서드 접근 제거 부족, 알려진 취약성이 있는 종속성 사용 등이 있습니다.

**인증 취약성**   
인증, 권한부여(인가)에 대한 취약점으로 인증과 세션 관리가 명확히 구분되어 있지 않는 등, 인증, 인가에 필요한 토큰 탈취 가능성에 대한 취약점을 말합니다. 여기서 인증이란 애플리케이션을 사용자를 식별하는 프로세스를 말하고, 권한 부여는 인증 호출자가 기능과 데이터 이용 권리가 있는지를 확인하는 과정을 말합니다.

**세션 고정**  
공격자가 정한 세션ID를 이용해서 사용자가 로그인 인증을 하게 되는 경우, 공격자도 같은 세션 ID를 가지고 있기 때문에 세션 하이재킹으로 사용자와 동일하게 정보를 공유받게 됩니다.

**XSS**  
교차 사이트 스크립트로 권한이 없는 사용자가 공격하려는 사이트에 스크립트를 넣는 방식으로 그 스크립트가 실행되게 하는 방식을 말합니다. 책의 사례에서는 댓글을 이용한 XSS로 다른 사용자들이 그 사이트를 실행할 때, 입력된 댓글 스크립트가 실행되며 다수의 이용자가 스크립트와 관련된 요청을 보내는 경우를 예시로 들고 있습니다.  
![632b48eb23fdda5e4f22a740_XSS In Action.jpg](/assets/gitbook/post_images/spring/632b48eb23fdda5e4f22a740_XSS%20In%20Action.jpg)

**CSRF**  
사이트 간 위조는 특정 애플리케이션에서 작동하는 url이 외부에서 재사용하므로써, url의 일부 파라미터를 변경하는 등의 방식으로 쉽게 변경해서 프로그램이 악용될 수 있도록 하는 방식을 말합니다.
```
GET http: //banking.com/transfer.do?acct=John&amount=1000 HTTP/1.1
GET http: //banking.com/transfer.do?acct=Mike&amount=5000
```

**주입**  
SQL 주입, XPath 주입, OS 명령 주입, LDAP 주입 등이 있습니다. <span style="background-color:#fff5b1; margin-right:5px">중요데이터는 볼트에 넣어줘야 합니다.</span> 볼트는 일반적으로 클라우드 기반의 데이터베이스 및 데이터 저장소 서비스인데, 중요한 데이터를 안전하게 저장하고 관리할 수 있는 기능을 제공합니다.

<span style="background-color:#DCFFE4; margin-right:5px">LDAP(Lightweight Directory Access Protocol)이 무엇일까?</span>  
LDAP는 사용자가 조직, 구성원 등에 대한 데이터를 찾는 데 도움이 되는 프로토콜입니다. 대부분 검색에 대한 요청으로 DAP보다 통신 네트워크 대역폭 상의 가벼운 특성이 있습니다. LDAP 서버에서 주로 특정 데이터를 중앙에서 관리하는 경우 트리구조로 저장하거나 조회하는 경우에 사용됩니다.

**기밀 데이터 노출**  
공개 정보는 로그화하지 말아야 합니다. 예를 들어 500 에러가 발생했을 때, 그대로 에러 내역이 클라이언트 단에 노출된다면 애플리케이션 구조와 그 종속성까지 다 알 수 있기 때문에 위험합니다.

<br><br>

## 2장. 안녕! 스프링 시큐리티

![Spring-Security-Architecture-1024x576.jpg](/assets/gitbook/post_images/spring/Spring-Security-Architecture-1024x576.jpg)

스프링 시큐리티에서 지원되는 ```UserDetailService```와 ```PasswordEncoder```를 재정의해서 위 그림처럼 인증 논리를 처리합니다. 지금은 ```@Decrypted```된 ```WebSecurityConfigurerAdapter```로 책 내용이 설명된 부분들이 많아 약간의 차이점은 있지만 인증 논리를 위해 config를 구성하는 큰 흐름은 변화가 없다는 부분에 중심을 두고 보는 것이 좋을 것 같습니다.
+ ```UserDetailService``` 계약을 구현하는 객체로 사용자에 대한 세부정보를 관리합니다. 내부 메모리에 기본 자격 증명을 등록하므로 스프링 컨텍스트가 로드될 때 자동으로 생성됩니다.
+ ```PasswordEncoder```는 말 그대로 비밀번호를 암호화 처리하는 방식으로 암호화 뿐만 아니라 암호화된 비밀번호가 기존 인코딩된 비밀번호와의 일치를 확인할 수 있도록 ```match``` 메서드를 제공하는 기능을 합니다.

지금 스프링 시큐리티와 달라진 부분이 많아서 책에서 제시하는 구현단 예시만 몇 가지 작성했습니다.

**구현 사례로 든 예시 코드 1**  
```UserDetailService```와 ```PasswordEncoder```를 구현하는 다양한 방법이 있지만, 두 부분의 메서드를 ```@Bean```으로 설정하지 않고 한 번에 같은 config로 설정하는 경우에는 구성이 혼합되어 책임이 분리되지 않았기 때문에 권장하지 않습니다.  
아래 코드에서는 ```@Deprecated```된 ```NoOpPasswordEncoder```를 사용해서 암호화를 구현했지만, 실제로는  ```return PasswordEncoderFactories.createDelegatingPasswordEncoder();```와 같은 방식 또는 개발자 나름의 재정의를 통해 작성하는 것이 바람직합니다.
```java
@Configuration
public class UserManagementConfig {
    @Bean
    public UserDetailsService userDetailsService() {
        var userDetailsService = new InMemoryUserDetailsManager();
        var user = User.withUsername("john")
                .password("12345")
                .authorities("read")
                .build();
        userDetailsService.createUser(user);
        return userDetailsService;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return NoOpPasswordEncoder.getInstance();
    }
}
```

**구현 사례로 든 예시 코드 2**  
```UserDetailService```와 ```PasswordEncoder```를 구 이 두 구성요소에 작업을 위임하는 ```AuthenticationProvider(인증공급자)```를 구현합니다. 그리고 ```AuthenticationManagerBuilder```를 이용한 구현을 config에서 ```AuthenticationProvider(인증공급자)```을 주입받아서 처리하는 경우의 사례도 있습니다.
```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {
    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        String username = authentication.getName();
        String password = String.valueOf(authentication.getCredentials());
        if ("john".equals(username) && "12345".equals(password)) {
            return new UsernamePasswordAuthenticationToken(username, password, Arrays.asList());
        } else {
            throw new AuthenticationCredentialsNotFoundException("Error!");
        }
    }

    @Override
    public boolean supports(Class<?> authenticationType) {
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authenticationType);
    }
}
```

<br><br>

## 3장. 사용자 관리
3장은 ```User```, ```UserDetailService```, ```UserDetailManager```에 대한 내용을 다루고 있습니다. 사용자가 인증을 하는 과정에서 인증 논리에 따라 인증 제공자는 사용자를 인증하는 과정을 거치게 되는데 이 때, 메모리 User를 관리하는 체계에 대한 내용입니다.

<br>

**UserDetils와 구현**  
사용자 기술을 위해서 스프링 시큐리티는 ```UserDetils``` 인터페이스를 구현하고 준수합니다. ```UserDetails```를 직접 클래스로 구현해도 되고, 사용에 따라 ```UserDetail```를 빌드해서 사용할 수 있습니다.
```UserDetails```는 하나 이상의 권한, password, username을 조회하거나 계정의 활성화 및 비활성화를 관리하는 메서드들을 가지고 있습니다. 계정의 관리는 기본적으로 ```true``` 처리를 하지만 별도 내부 서비스 방침에 따라 커즈터마이징해 구현할 수 있습니다.
```java
public interface UserDetails extends Serializable {
	Collection<? extends GrantedAuthority> getAuthorities();
	String getPassword();
	String getUsername();
	boolean isAccountNonExpired();
	boolean isAccountNonLocked();
	boolean isCredentialsNonExpired();
	boolean isEnabled();
}
```  
이렇게 ```UserDetails```를 하나의 클래스로 implements 해서 구현하는 방법도 있지만, ```UserDetailService```를 위해 제공되는 ```User``` 모델을 빌드할 수 있습니다. 이 모델도 ```UserDetails``` 구현체입니다.
이 경우 ```withUsername``` 메서드를 이용해 ```UserBuilder```를 만들어 ```UserDetiails``` 객체를 만들 수도 있습니다.
```java
UserDetails userDetails = User.withUsername(member.getEmail())
                            .password(member.getPassword())
                            .roles(member.getRole())
                            .build();
```

<br>

**UserDetailService와 세 가지 UserDetailManager에 대하여**  
```UserDetailService```는 인증의 핵심이 되는 부분으로 인증 논리에서 메모리 내에서 User를 관리합니다. ```UserDetailService``` 인터페이스는 단 한 가지 메서드를 가지고 있는데 ```loadUserByUsername(String username)```를 가지고 Username에 해당하는 사용자가 있는지 메모리에서 확인합니다. 해당하는 사용자가 없다면, ```UsernameNotFoundException```로 ```RuntimeException```를 던집니다.
```java
public interface UserDetailsService {
	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}
```
책에서 제시한 ```loadUserByUsername``` 구현의 예입니다. User들 중에서 username이 동일한 User만을 추출해 UserDetails로 리턴하는 것을 볼 수 있습니다.
```java
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException{
  return Users.stream()
    .filter(u -> u.getUsername().equals(username))
    .findFirst()
    .orElseThrows(() -> new UsernameNotFoundException("User not found"));
}
```
```UserDetailManager```는 ```UserDetailService```의 기능을 확장하고 메서드를 추가합니다. ```UserDetailManager```의 구현클래스로 ```InMemoryUserDetailsManager```와 ```JdbcUserDetailsManager```를 제공하고 있습니다. 내부 메모리를 사용하는지 또는 SQL 데이터베이스에 저장된 사용자를 관리하며 JDBC를 통해 데이터베이스에 직접 연결하는지에 따라 다르게 사용됩니다.  
```LdapUserDetailsManager```도 제공하고 있지만 LDAP를 사용할 경우 [별도 dependency 설정](https://www.baeldung.com/spring-security-ldap)이 필요합니다.
```java
public interface UserDetailsManager extends UserDetailsService {
	void createUser(UserDetails user);
	void updateUser(UserDetails user);
	void deleteUser(String username);
	void changePassword(String oldPassword, String newPassword);
	boolean userExists(String username);
}
```
추가로 ```setUsersByUsernameQuery``` 등과 같이 ```JdbcUserDetailsManager```의 쿼리를 변경할 수 있으며 @Bean으로 ```UserDetailsService``` config로 등록할 수 있습니다.
```java
@Bean
public UserDetailsService userDetailsService(DataSource dataSource) {
    String usersByUsernameQuery = "select username, password, enabled from spring.users where username = ?";
    String authsByUserQuery = "select username, authority from spring.authorities where username = ?";
    var userDetailsManager = new JdbcUserDetailsManager(dataSource);
    userDetailsManager.setUsersByUsernameQuery(usersByUsernameQuery);
    userDetailsManager.setAuthoritiesByUsernameQuery(authsByUserQuery);
    return userDetailsManager;

}
```

<br><br>

## 4장. 암호처리
인증공급자가 제공한 password를 이용해서 ```PasswordEncorder```를 이용해서 암호를 검증합니다.
이 부분은 작성시점인 현재(2023-09)와 차이가 좀 있지만 프로세스를 이해하기 위한 것으로 책을 기반으로 서술하고자 합니다.

**PasswordEncorder 인터페이스**  
````encode()````를 통해 암호화를 ```matches()```를 통해 인코딩된 문자열이 암호와 일치여부를 확인할 수 있습니다. ```upgradeEncoding(CharSequence encodedPassword)```는 기본값이 ````false````이지만 true 처리하는 경우 인코딩된 암호를 다시 인코딩하게 됩니다.

Spring security에서 제공하는 ```PasswordEncoder``` 구현 옵션들은 다음 옵션들이 있습니다. [각 해싱 알고리즘에 대한 설명](https://livebook.manning.com/book/real-world-cryptography/chapter-2/17)
+ NoOpPasswordEncoder 
+ BCryptPasswordEncoder 
+ Pbkdf2PasswordEncoder 
+ SCryptPasswordEncoder 
+ Argon2PasswordEncoder 
+ DelegatingPasswordEncoder

**NoOpPasswordEncoder**  
테스트나 이전 시스템과의 호환성을 위한 경우에만 사용되어야 합니다. 현재는 Deprecated 되었습니다.

**BCryptPasswordEncoder**

```java
@Bean  
public BCryptPasswordEncoder passwordEncoder() {  
	return new BCryptPasswordEncoder(16);  
}
```

이렇게 인코딩 프로세스에 이용되는 로그 라운드를 나타내는 강도 계수를 지정할 수도 있습니다. 아래 예제에서 4는 강도 계수입니다. 이 값은 2의 4제곱 즉, 16번의 해싱 라운드를 의미합니다. b는 BCrypt에서 솔트(salt)를 생성할 때 사용됩니다.
```java
SecureRandom s = SecureRandom.getInstanceStrong();
PasswordEncoder p = new BCryptPasswordEncoder(4, s);
```


**DelegatingPasswordEncoder**  
여러 해싱 전략을 유연하게 관리할 수 있게 도와줍니다. 접두사 ```{noop}```에 대해 NoOpasswordEncoder가, ````{bcrypt}````인 경우에는 BCryptPasswordEncoder, ```{scrypt}```이면, SCryptPasswordEncoder를 등록합니다.
스프링 시큐리티에는 DelegatingPasswordEncoder의 구현을 반환하는 정적 메서드를 제공합니다.
```java
PasswordEncoder passwordEncoder = PasswordEncoderFactories.createDelegatingPasswordEncoder();
```
스프링 시큐리티에서 DelegatingPasswordEncoder를 기본적으로 사용하게 되면서, NoOpPasswordEncoder와 같은 deprecated된 PasswordEncoder를 포함하여 여러 전략을 지원하게 되었습니다. 이때, `NoOpPasswordEncoder`에 대한 deprecated 경고를 피하기 위해 `createDelegatingPasswordEncoder` 메서드에 `@SuppressWarnings("deprecation")`을 추가된 것을 볼 수 있습니다.

<br>

**그럼 비밀번호 암호화 외에는 어떻게 암호화를 구현할까요?**  
스프링 시큐리티 암호화 모듈(SSCM)에는 키 생성기와 암호기를 구현하는 대안이 있습니다.
```java
StringKeyGenerator keyGenerator = KeyGenerators.string();
String salt = keyGenertor.generateKey();


BytesKeyGenertor keyGenerator = KeyGenerators.secureRandom(16);

BytesKeyGenertor keyGenerator = KeyGenerators.shared(16);
```
암호기는 암호화 알고리즘을 구현하는 객체입니다. BytesEncryptor와 TextEncryptor라는 암호기가 있고 각각은 다른 데이터 형식을 처리합니다. BytesEncryptor가 문자열로 출력을 반환하는 반면, TextEncryptor는 더 범용적이고 바이트 배열로 입력 데이터를 받습니다.
```java
BytesEncryptor e = Encryptors.stronger(password, salt);
```
TextEncryptor는 ```Encryptors.text()```, ```Encryptors.delux()```, ```Encryptors.queryableText()```의 세 가지 주요 형식을 가지고 있습니다. ```Encryptors.text()```, ```Encryptors.delux()```는 ```encrypt()``` 메서드를 반복 호출해도 다른 출력이 반환되는데, 초기화 벡터가 생성되기 때문입니다.
```Encryptors.queryableText()```는 쿼리 가능 텍스트로 입력이 같으면 같은 출력을 반환하는 것을 보장합니다.
```java
TextEncryptor e = Encryptors.queryableText(password, salt);
String encrypted = e.encrypt(valueToEncrypt);
```

<br><br>


## 5장. 인증 구현

+ 맞춤형 AuthenticationProvider로 인증 논리 구현
+ HTTP Basic 및 양식 기반 로그인 인증 메서드 이용
+ SecurityContext 구성 요소의 이해 및 관리

인증 필터가 가로챈 요청을 그리고 그 인증 책임을 인증 관리자에 위임합니다. 그리고 인증 공급자에서 확인한 인증 결과는 보안 컨텍스트(SecurityContext)에 저장됩니다.

~~**AuthenticationProvider의 이해**~~  
Authentication 계약인 Principal 계약을 상속하는데 Authentication에서는 암호같은 요구 사항이나 인증 요청에 대한 세부 정보를 추가할 수 있습니다. 이는 자바 시큐리티 API의 Principal 계약을 확장하도록 설계되어 호환성 측면에 이점이 됩니다. (마이그레이션 이점)
```java
public interface Authentication extends Principal, Serializable {  
	Collection<? extends GrantedAuthority> getAuthorities();  
	Object getCredentials();  
	Object getDetails();  
	Object getPrincipal();   
	boolean isAuthenticated();    
	void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;  
}
```

AuthenticationProvider 인터페이스는 사용자를 찾는 것은 UserDetailsService에 위임하고 PasswordEncoder로 인증 프로세스에서 암호를 관리합니다.

하지만, ```AuthenticationProvider```는 Spring Security 5부터 Deprecated 되었으며, 대신에 AuthenticationManager를 사용하도록 권장됩니다. AuthenticationManager는 AuthenticationProvider와 다양한 인증 방식을 지원하는 Provider들을 통합적으로 관리하고, 실제 인증 처리를 담당하는 역할을 수행하기 때문입니다.

구현부의 ```AuthenticationProvider```와 ```WebSecurityConfigurerAdapter``` 둘 다 현재는 사용되지 않는 부분으로 작성하지 않았습니다.

<br>

글 마지막에 이런 내용이 존재하는데 이 부분이 오히려 책의 핵심 내용이자, 의존성을 사용하는 개발자들이 필히 고려해야 할 부분이라고 생각합니다.

프레임워크, 특히 애플리케이션에 널리 이용되는 프레임워크는 수많은 똑똑한 개발자의 참여로 개발된다. 그렇다고 해도 프레임워크가 잘못 구현되는 경우는 많지 않다. 어떤 문제가 프레임워크의 잘못이라고 결론 내리기 전에 애플리케이션을 분석하자.

프레임워크를 이용하기로 결정했다면 최대한 프레임워크의 의도된 용도에 맞게 이용한다. 예를 들어 스프링 시큐리티를 이용하면서 보안 구현에 프레임워크가 제공하는 것보다 맞춤형 코드를 작성하는 일이 많다고 느낀다면 이런 일이 왜 생기는지 의문을 가져야 한다.

<br>

**SecurityContext 이용**  
보안 컨텍스트는 Authentication 객체를 저장하는 인스턴스입니다. 
```java
public interface SecurityContext extends Serializable{
	Authentication getAuthentication();
	void setAuthentication(Authentication authentication);
}
```
SecurityContext를 관리하기 위해 스프링 시큐리티는 SecurityContextHolder라는 객체를 제공합니다.  MODE_THREADLOCAL, MODE_INHRITABLETHREADLOCAL, MODE_GLOBAL 옵션을 제공하고 설정은 Config에서 다음과 같이 설정할 수 있습니다.
```java
return() -> SecurityContextHolder.setStrategyName(	SecurityContextHodler.MODE_INHERITABLETHREADLOCAL);
```

**MODE_THREADLOCAL**  
(보안 컨텍스트를 위한 보유 전략 이용)   
각 스레드가 보안 컨텍스트 각자 세부 정보를 저장하는 것으로 기본값입니다. 새 스레드도 자체 보안 컨텍스트를 가지며 상위 스레드의 세부 정보가 새 스레드의 보안 컨텍스트로 복사되지 않습니다.
이 SecurityContext에서 Authentication을 얻기 위해서는 앤드포인트 매개변수에 바로 주입해서 얻을 수 있습니다.
```java
@GetMapping
public String hello(Authentication a){
	return a.getName();
}
```

**MODE_INHRITABLETHREADLOCAL**  
(비동기 호출을 위한 보유 전략 이용)  
MODE_THREADLOCAL와 비슷하지만 비동기 메서드의 경우 보안 컨텍스트를 다음 스레드로 복사하도록 스프링 시큐리티에 지시합니다.
부모 스레드에서 생성된 SecurityContext를 자식 스레드에서도 공유할 수 있습니다. 즉, 원래 스레드에 있는 세부 정보를 비동기 메서드의 새로 생성된 스레드로 복사합니다.

**MODE_GLOBAL**  
(독립형 애플리케이션을 위한 보유 전략 이용)  
애플리케이션의 모든 스레드가 같은 보안 컨텍스트 인스턴스를 보게 합니다. 스레드 안전을 지원하지 않음에 유의해야 합니다. 공유 스레드 전략에서는 스프링이 관리하는 스레드에만 전략이 적용된다는 것을 기억해야 합니다.

<br>

보안 컨텍스트를 새로 생성한 스레드로 전파하게 도와주는 몇 가지 스프링 시큐리티의 유틸리티 툴이 있습니다.

**DelegatingSecurityContextRunnable**  
Runnable 인터페이스를 구현한 클래스로, 생성자로 Runnable 객체와 현재 SecurityContext를 받아서, 새로 생성한 스레드에서 Runnable 객체를 실행할 때 보안 컨텍스트를 전파합니다. 반환값이 없는 작업 실행 후 이용할 수 있습니다.
```java
SecurityContext securityContext = SecurityContextHolder.getContext();
Runnable runnable = new MyRunnable();
Runnable wrappedRunnable = new DelegatingSecurityContextRunnable(runnable, securityContext);
new Thread(wrappedRunnable).start();
```

**DelegatingSecurityContextCallable**  
반환값이 있는 작업에는 DelegatingSecurityContextCallable 대안을 사용할 수 있습니다. 현재 보안 컨텍스트를 복사해 비동기적으로 실행할 Callable 작업을 지정합니다.

**DelegatingSecurityContextExecutorService**  
ExecutorService 인터페이스를 구현한 클래스로, 보안 컨텍스트를 새로 생성한 스레드에서 실행하는 작업을 자동으로 처리할 수 있습니다. 이와 유사하게 DelegatingSecurityContextScheduledExecutorService는 예약된 작업을 위해 보안 컨텍스트 전파를 구현해야 하는 경우의 데코레이터로 사용됩니다.
```java
SecurityContext securityContext = SecurityContextHolder.getContext();
ExecutorService executorService = Executors.newFixedThreadPool(10);
ExecutorService wrappedExecutorService = new DelegatingSecurityContextExecutorService(executorService, securityContext);
wrappedExecutorService.submit(new MyRunnable());
```

<br>

**HTTP Basic 인증 양식**

```java
http.httpBasic(c->{
		// 영역 이름을 변경하는 방법
		c.realName("OTHER");
		// custom AuthenticationEntryPoint 적용
		c.authenticationEntiryPoint(new CustomEntryPoint());
	})
	http.authorizeRequests().anyRequest().authenticated();
```

AuthenticationEntryPoint 인터페이스  
Spring Security에서는 여러 가지 AuthenticationEntryPoint 구현체를 제공하며, 이를 통해 다양한 인증 실패 시나리오에 대응할 수 있습니다. 예를 들어, 웹 애플리케이션에서는 ```LoginUrlAuthenticationEntryPoint```가 자주 사용되며, REST API에서는 ```Http403ForbiddenEntryPoint```가 자주 사용됩니다.

**양식 기반 로그인 인증**  
> 수평적 확장성이 필요한 대형 애플리케이션에서 보안 컨텍스트를 관리하는 데 서버 쪽 세션을 이용하는 것은 좋지 않습니다.

양식 기반 로그인을 사용하는 경우에는 formLogin() 메서드를 이용합니다. 스프링 시큐리티에서 로그인을 하지 않는 경우는 로그인 페이지로 리디렉션되게 처리됩니다.
```java
http.formLogin();
```
```@RestController```가 아닌 ```@Controller```를 사용하므로써 메서드 반환 값을 HTTP 응답으로 보내는 것이 아닌 ```home.html```로 랜더링해줍니다.
이 때 로그인을 하지 않았다면, /login 로그인 페이지로 리디렉션되고 /logout 경로로 접근하면 로그아웃 페이지로 리디렉션됩니다. (HTTP 302)
```java
@Controller
public class HelloController{
	@Getmapping("/home")
	public String home(){ return "home.html"; }
}
```
```formLogin()```의 맞춤 구성을 위해 ```AuthenticationSuccessHandler```와 ```AuthenticationFailureHandler``` 객체를 이용할 수 있습니다.

<br><br>

## 6장 실전: 작고 안전한 웹 애플리케이션
주로 구현단에 대한 이야기가 있는 챕터입니다.  작성일자 기준 Deprecated된 내용은 제외했습니다.

+ 의존성 추가
    + SQL 버전 지정을 위한 플라이웨이, 리퀴베이스 종속성 이용할 수 있습니다.
+ 비밀번호 암호화를 위한 PasswordEncoder ```@Bean```으로 등록
+ User, Authority 엔티티 설정
+ UserDetails 인터페이스의 구현

```java
@Override
public CustomUserDetails loadUserByUsername(String username){
	Supplier<UsernameNotFoundException> s = () -> new UsernameNotFoundException("user not found");
	User user  = userRepository.findByUsername(username)
	.orElseThrow(s);
	return new CustomUserDetails(user);
}
```
인증논리에서 암호가 일치하면 ```encoder.matches(rawPassword, user.getPassword())``` 인증이 되었으므로, Authentication을 반환합니다.
> 대부분은 같은 기능을 여러 가지 다른 방법으로 구현할 수 있으며, 가장 단순한 해결책을 선택해 코드를 이해하기 쉽게 만들어 오류와 보안 침해 여지를 줄일 필요가 있습니다.

<br><br>

## 7장. 권한 부여 구성: 액세스 제한
스프링 시큐리티에서는 인증 필터에서 보안 컨텍스트에 인증된 사용자 세부 정보를 저장하고 이후에 권한 부여 필터로 요청을 허용할지 결정합니다.

한 사용자는 하나 이상의 권한을 갖습니다. 즉 GrantedAuthority로 UserDetils는 하나 이상의 권한을 갖습니다. UserDetils의 ```getAuthorities()``` 메서드로 사용자 세부 정보의 모든 권한을 반환합니다.

**엔드포인트 권한 제어**  
```permitAll()```이 아닌 특정 권한 제어로 "WRITE" 권한을 가진 경우에만 접근이 가능하도록 할 수 있습니다. 이외에도 권한 제어를 위한 다음 세 가지 메서드가 있습니다.
+ hasAuthority()
+ hasAnyAuthority()
+ access()

```java
// .hasAuthority()의 이용
http.authoriseRequests()
.anyRequest()
.hasAuthority("WRITE");

// .access()의 이용
// 복잡한 식을 작성해야 하는 경우의 실제 시나리오가 있을 수 있습니다.
http.authoriseRequests()
.anyRequest()
.access("hasAuthority('WRITE') and !hasAuthority('DELETE')");
```

<span style="background-color:#DCFFE4; margin-right:5px">스프링 식(SpEL)이란?</span>  
Spring Framework에서 제공하는 표현 언어입니다.  
스프링 식은 XML이나 애노테이션 기반의 구성에서 사용할 수 있으며, 객체 그래프를 탐색하고 조작하는 데 사용됩니다. 스프링 식은 Java의 다양한 연산자와 함수를 지원하며, 객체의 프로퍼티나 메서드 호출, 리스트나 맵의 인덱스 접근, 산술 연산 등 다양한 기능을 제공합니다.
책의 예제에서는 정오 이후에만 엔드포인트 접근을 하용하는 경우 등 SpEL 식을 활용해서 ```access()``` 메서드를 사용하는 더 범용적인 상황에서 사용이 가능합니다.

권한이 없는 경우의 요청은 HTTP 상태는 ```HTTP 403 Forbidden```을 반환합니다.
여기서 권한과 달리 역할은 권한보다 결이 굵은데 같은 GrantedAuthority가 사용되며, 역할이름은 ```ROLE_```로 시작해야 합니다.
```java
User.withUsername("john")
.password("qwer1234")
.authorities("ROLE_MEMBER")
.build();
```

```permitAll()```로 모든 접근 권한을 허용할 수 있으며, ```denyAll()```로 모든 요청을 거부할 수도 있습니다.

<br><br>

## 8장. 권한 부여 구성: 제한 적용
선택기 메서드로 엔드포인트 선택  
스프링 시큐리티에서 제공하는 선택기 메서드는 MVC 선택기, 앤트 선택기, 정규식 선택기가 있습니다.

> 이용하는 선택기가 어떤 것이지도 모르고 하는 복사-붙여넣기 프로그래밍의 위험한 접근법을 초보 개발자가 너무 자주 이용합니다.   
> 어떻게 작동하는지 이해하기 전에는 이용하지 말아야 합니다!

**MVC 선택기**  
책에서는 앤트 선택기보다 MVC 선택을 권장했는데, 매핑으로 인한 위험을 방지하기 위함입니다.
```java
http.authorizeRequests()
        .mvcMatchers(HttpMethod.Post, "/hello").authenticated()
        // 길이와 관계없이 숫자를 포함하는 문자열을 나타내는 정규식
        .mvcMatchers("/reg/{code:^[0-9]*$*}").permitAll()
        .anyRequest().authenticated();
```

**앤트 선택기**  
MVC 선택기와 달리 스프링 MVC의 작동을 고려하는 것이 아닌 방식으로 /hello 경로의 엔드포인트를 설정한다고 했을 때, /hello/ 경로는 보호되지 않습니다. 즉 확실한 경로 설정이 필요합니다.
```java
http.authorizeRequests()
        .antMatchers("/hello/**").hasRole("ADMIN")
        .anyRequest().authenticated();        
```

**정규식 선택기**  
[온라인 정규식 선택기](https://regexr.com)  
MVC와 엔트 식으로 해결할 수 없는 경우에 이용하는 것이 좋습니다.
```java
http.authorizeRequests()
        .regexMatchers(".*/(kr|ca)+/(en).**").authenticated()
        .anyRequest().authenticated();        
```
잘못된 자격 증명으로 엔드포인트를 호출하면 권한이 있더라도 인증 필터에서 먼저 인증이 실패하는 경우 권한부여 필터까지 가지 않고 응답 상태 ```HTTP 401 Unauthorized```를 반환합니다.

스프링 시큐리티를 기본적으로 CSRF(사이트 간 요청 위조)에 대한 보호를 적용하는데 모든 엔드포인트 호출을 위해 비활성화할 수 있습니다.
```java
http.csrf().disable();
```

<br><br>


## 9장. 필터 구현
스프링 시큐리티가 제공하는 필터 BasicAuthenticationFilter 외의 맞춤형 필터 체인을 추가할 수 있습니다. 권한필터 전 이벤트나 로깅필터 구축이 가능해집니다.
스프링에서 제공하는 필터 간에는 순서가 이미 정해져 있습니다. ```CorsFilter``` → ```CsrfFilter``` → ```BasicAuthenticationFilter``` 순입니다.

**맞춤형 필터 구현**
```java
public class RequestVaildationFilter implements Filter {
	@Override
	public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  
		var httpRequest = (HttpServletRequest) request;
		var httpResponse = (HttpServletResponse) response;
		String requestId = httpRequest.getHeader("Request_id");

		if(requestId.isBlank){
			httpResponse.setStatus(HttpServletResponse.SC_BAD_REQUEST);
			return;
		}
		filterChain.doFilter(request, response);
	}
}
```

**맞춤형 필터 순서 지정**  
이렇게 ```addFilterBefore```를 사용하면, BasicAuthenticationFilter 전에 RequestVaildationFilter를 사용할 수 있 됩니다.   
인증 통과를 기록하는 필터를 만든다고 한다면 BasicAuthenticationFilter 뒤에 배치할 수 있도록 ```addFilterAfter```를 사용할 수 있습니다.
```java
http.addFilterBefore(
	new RequestVaildationFilter(),
	BasicAuthenticationFilter.class)
	.authorizeRequests().anyRequest().permitAll();
)
```

그렇다면 다른 필터 위치에 필터를 추가하려면 어떻게 해야할까?  
BasicAuthenticationFilter에 배치하려면 ```addFilterAt```를 사용할 수 있습니다. 이는 필터 대체가 아닌 추가적인 개념으로 봐야합니다.

이런 경우의 시나리오에 대해서 책에서는 다음 세 가지 사례를 들고 있습니다.
+ 인증은 위한 정적 헤더 값에 기반을 둔 식별
  + 암호화 서명이 아닌 단순한 문자열 제공으로 인증하는 경우
+ 대칭 키를 이용해 인증 요청 서명
  + 클라이언트와 서버가 키를 공유하는 경우
+ 인증 프로세스에 OTP 이용
  + 타가 인증 서버에서 OTP를 얻어서 애플리케이션 다단계 인증을 사용하는 경우

> 실제 시나리오에서 세부 정보를 저장할 때는 비밀 볼트를 이용해야 합니다.

UserDetailsService 구성을 비활성화 하기 위해서 ```exclude``` 특성을 이용할 수 있습니다.
```java
@SpringBootApplication(exclude={UserDetailsServiceAutoConfiguration.class})
```

**OncePerRequestFilter**  
스프링 시큐리트에서 제공하는 필터를 이용하는 것도 좋지만 최대한 간단하게 구현할 수 있는 방법이 있다면 그냥 확장하는 것을 피해야 합니다. 같은 요청에 한 번의 필터 호출을 위해 OncePerRequestFilter 클래스를 이용해 필터를 구현하는 것이 권장됩니다.

OncePerRequestFilter는 형식을 형변환하여 HttpServletReqeust 및 HttpServletResponse로 직접 요청을 수신합니다.  필터에서 요청 및 응답을 직접 조작할 수 있으므로 필요한 작업을 수행할 수 있습니다. 이러한 객체를 직접 사용하면 필터에서 외부 라이브러리나 서비스에 의존하지 않아도 되므로 독립성과 유연성을 높여줍니다.  
기본적으로 비동기 요청이나 오류 발송 요청에는 적용되지 않습니다. 하지만 메서드 재정의로 가능하게 할 수 있습니다. 필터가 적용될지 여부도 메서드 재정의로 정할 수 있습니다.

<br><br>

## 10장. CSRF 보호와 CORS 적용
**CSRF(사이트 간 요청 위조) 보호 적용**  
POST, PUT, DELETE를 비롯한 변경 호출 페이지는 응답으로 CSRF 토큰을 받고 다음 호출에 사용해야 합니다. 여기서 CSRF 공격은 사용자가 악성 스크립트가 있는 페이지를 열었을 때, 사용자 대신 작업을 수행하게 됩니다.

```CsrfFilter```를 통해 발급되는데, 이 필터는 GET, HEAD, TRACE, OPTIONS를 사용한 HTTP 방식의 요청을 모두 허용하고 CSRF 토큰을 담아서 응답합니다.
POST, PUT, DELETE 의 엔드포인트에서 개발을 하기 위해서는 CSRT 보호 활성화된 경우 토큰이 필요한데, ```_csrf``` 특성에 추가되어 있기 때문에 이를 응답 또는 thymeleaf를 이용해 출력합니다.

CSRF 보호는 브라우저에서 실행되는 웹 앱에 사용되며, 로그인과 같이 앱 내에서 수행하는 변경되는 작업이 없는 경우를 제외합니다.
그리고 같은 서버가 프런트와 백을 담당하는 아키텍처에서는 잘 작동하지만 솔루션이 독립적일 때는 그에 따른 보안 접근법(11~15장)을 고려해야 합니다.

Spring security는 기본적으로 CSRF 보호를 지원하는데, 서버에서 생성된 리소스를 이용하는 페이지가 같은 서버에서 생성된 경우에만 이용합니다.

그럼 모든 경로가 아니라 일부 경로에만 보호를 설정하려면 어떻게 해야 할까?
CSRF 보호를 아래 코드 처럼 ```disable``` 처리하는 것이 아니라 제공되는  handler와 matcher들 이용해서 보호 제외 경로를 지정할 수 있습니다.
```
http.httpBasic(HttpBasicConfigurer::disable)  
	.csrf(CsrfConfigurer::disable)
```

서버 쪽 세션에 csrf 토큰을 저장하는 것은 수평적 확장에서 적합하지 않습니다.
데이터베이스에서 토큰을 관리하는 방식으로 전환하기 위해서는 ```CsrfTokenRepository```를 새로 구현해서 세션 ID를 이용해 관리할 수 있습니다.
```CsrtTokenRepository``` 구현체를 만들면 override된 ```generateToken()```과 ```saveToken()```, ```loadToken()``` 메서드를 구현해서 사용하게 됩니다.
다른 대안으로는 토큰의 만료시간을 정해서 관리하는 방법도 있습니다.

**CORS(교차 출처 리소스 공유) 이용**  
CORS는 일부 조건에서 서로 다른 출처 간 요청을 허용합니다. 예를 들어 ```domain.com```에서 ```domain2.com```에서 ```domain.com```의 REST 엔드포인트를 호출한다고 할 때, 호출은 거부됩니다. CORS를 이용하면 이런 문제를 해결할 수 있습니다.

+ ```Access-Control-Allow-Origin``` 접근 가능한 외부 도메인 지정
+ ```Access-Control-Allow-Methods``` 다른 도메인에 대한 접근은 허용하지만 특정 엔드포인트만 허용
+ ```Access-Control-Allow-Headers``` 특정 요청에 이용할 수 있는 헤더에 제한을 추가

CSRF와 개념을 헷갈리지 말아야하는데 CORS는 교차 도메인 호출을 완화해주는 개념이고 브라우저에 관한 것이며 엔드포인트를 보호하는 방법은 아닙니다. CSRF 보흐는 공격을 막는 개념입니다.

HTTP OPTIONS 방식으로 사전 테스트 요청을 하는 경우가 있습니다. 이 요청이 실패하면 브라우저는 원래 요청을 수락하지 않습니다.

CSRF의 정책은 ```@CrossOrigin```으로 적용할 수 있습니다. 다음과 같이 메서드 위에 적용하면 해당 메서드는 localhost 출처에 대한 교차 출처 요청을 허용하게 됩니다. 이 때 ```*```로 넓은 범위의 출처를 허용하는 경우는 XXS(교차 사이트 스크립팅) 요청에 노출되어 DDoS 공격에 취약해질 수 있습니다.
```
@CrossOrigin("http://localhost:8080")
```

하지만 개별 관리를 이런 식으로 하게 되면, 관리가 어렵기 때문에 Security Config에 ```CorsConfigurationSource```를 정의해서 허용할 출처와 메서드를 지정해서 ```cors()```에 적용할 수 있습니다.

<br><br>

## 11장. 실전: 책임의 분리

<br><br>

****
+ [Spring Security in Action](https://github.com/spring-projects/spring-security/)
+ [XSS vs CSRF](https://www.wallarm.com/what/what-is-the-difference-between-csrf-and-xss)
+ [Guide to UserDetailsService in Spring Security](https://howtodoinjava.com/spring-security/inmemory-jdbc-userdetails-service/)

<div style="padding:3px; margin:200px 0;"></div>   




