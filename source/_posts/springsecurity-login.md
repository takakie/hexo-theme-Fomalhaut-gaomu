---
title: SpringSecurity登录接口实现
tags: test
abbrlink: dfeaaa20
---

# 1.配置mybatis-plus

## 1.1.配置数据库链接信息

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/auth_user?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

## 1.2.集成BaseMapper接口

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.gaomu.domain.User;

public interface UserMapper extends BaseMapper<User> {

}
```

## 1.3.实现 UserDetailsServiceImpl

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.gaomu.domain.LoginUser;
import com.gaomu.domain.User;
import com.gaomu.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

import java.util.Objects;

@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

        //查询用户信息
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<User>();
        queryWrapper.eq(User::getUserName, username);
        User user = userMapper.selectOne(queryWrapper);
        //如果没有查询到用户就行抛出异常
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
        //查询对应的权限信息
        //把数据封装成UserDetails返回
        return new LoginUser(user);
    }
}
```

## 1.4.实现UserDetails所有接口

**所有接口会被DAOAuthenticationProvider类调用进行用户校验**

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
@Data
@NoArgsConstructor
@AllArgsConstructor
public class LoginUser implements UserDetails {


    private User user;

    //账户授权信息
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUserName();
    }

    //账户是否过期
    @Override
    public boolean isAccountNonExpired() {
        return ture;
    }

    //账户是否被锁定
    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    //账户凭证是否过期
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    //账户是否可用
    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

## 1.5.数据库字段

![image-20231116124145225](C:\Users\ChenLei\Desktop\周报\springsecurity重写登录接口\image-20231116124145225.png)

若使用明文存储密钥需要在密码前加上{noop}标识符

![image-20231127102407425](C:\Users\ChenLei\Desktop\周报\springsecurity重写登录接口\image-20231127102407425.png)

## 1.6.登录校验测试

![image-20231127102626714](C:\Users\ChenLei\Desktop\周报\springsecurity重写登录接口\image-20231127102626714.png)

![image-20231127102703527](C:\Users\ChenLei\Desktop\周报\springsecurity重写登录接口\image-20231127102703527.png)