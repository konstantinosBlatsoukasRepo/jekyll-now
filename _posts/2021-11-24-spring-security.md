# Spring Security

## dependencies

```groovy
    implementation 'org.springframework.boot:spring-boot-starter-security:2.5.6'
    implementation 'org.springframework.ldap:spring-ldap-core:2.3.4.RELEASE'
    implementation 'org.springframework.security:spring-security-ldap:5.5.1'
    implementation 'com.unboundid:unboundid-ldapsdk:6.0.2'
    implementation 'io.jsonwebtoken:jjwt:0.9.1'
```

## configuration

--> authentication (who)

```java
@Configuration
@EnableWebSecurity
class MyClass extends WebSecurityConfigureAdapter {


  // AuthenticationManagerBuilder auth, build authentication rules
  @Override
  protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    auth.inMemoryAuthentication() // ldap, jdbc ...
    .withUser("myUser")
    .password("myPass")
    .roles("ROLE")
    .and()
    .withUser("maangerUser")
    .password("myPass")
    .roles("ADMIN")
  }

  @Bean
  public PasswordEncoder getPasswordEncoder() {
    return NoOpPAsswordEncoder.getInstance();
  }


}
```

--> authorization (what) + authorities === permissions

```java
@Configuration
@EnableWebSecurity
class MyClass extends WebSecurityConfigureAdapter {

.....

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
    .antMatchers("/projects/new").hasRole("ADMIN")
    .antMatchers("/").authenticated.and().formLogin();
  }

}
```
