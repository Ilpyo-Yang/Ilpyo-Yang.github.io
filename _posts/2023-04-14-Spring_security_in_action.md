---
title: 📖 Spring Security in Action (진행중)
author: Rosie Yang
date: 2023-04-14
category: backend
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
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#1장-오늘날의-보안">1장. 오늘날의 보안</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#2장-안녕-스프링-시큐리티">2장. 안녕! 스프링 시큐리티</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#3장-사용자-관리">3장. 사용자 관리</a></li>
                <li><a href="/backend/2023/04/14/Spring_security_in_action.html#4장-암호처리">4장. 암호처리</a></li>
            </ul>
        </td>
    </tr>
</table>

<br>

<br>

### 1장. 오늘날의 보안
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

### 2장. 안녕! 스프링 시큐리티

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

### 3장. 사용자 관리
3장은 ```User```, ```UserDetailService```, ```UserDetailManager```에 대한 내용을 다루고 있습니다. 사용자가 인증을 하는 과정에서 인증 논리에 따라 인증 제공자는 사용자를 인증하는 과정을 거치게 되는데 이 때, 메모리 User를 관리하는 체계에 대한 내용입니다.

<br>

**UserDetils와 구현**  
사용자 기술을 위해서 스프링 시큐리티는 ```UserDetils``` 인터페이스를 구현하고 준수합니다. ```UserDetails```를 직접 클래스로 구현해도 되고, 사용에 따라 ```UserDetail```를 빌드해서 사용할 수 있습니다.
```UserDetails```는 하나 이상의 권한, password, username을 조회하거나 계정의 활성화 및 비활성화를 관리하는 메서드들을 가지고 있습니다. 계정의 관리는 기본적으로 ```true``` 처리를 하지면 별도 내부 서비스 방침에 따라 커즈터마이징해 구현할 수 있습니다.
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

### 4장. 암호처리

****
+ [Spring Security in Action](https://github.com/spring-projects/spring-security/)
+ [XSS vs CSRF](https://www.wallarm.com/what/what-is-the-difference-between-csrf-and-xss)
+ [Guide to UserDetailsService in Spring Security](https://howtodoinjava.com/spring-security/inmemory-jdbc-userdetails-service/)

<div style="padding:3px; margin:200px 0;"></div>   




