# shiro（留坑）

Apache Shiro 是一个强大且易于使用的开源安全框架，用于身份验证、授权、加密和会话管理等安全操作。它广泛用于Java应用程序中，提供了全面的安全管理功能，可以轻松集成到现有的应用程序中，保护应用程序的安全性和保密性。

### 主要特点和功能：

1. **认证（Authentication）**：
   - Shiro 提供了对用户身份验证的支持，支持多种认证方式，如基于用户名/密码的认证、基于LDAP的认证、基于OAuth的认证等。
   - 可以轻松地编写自定义的身份验证器，并与各种认证数据源集成。

2. **授权（Authorization）**：
   - Shiro 允许定义和管理应用程序中的权限和角色。
   - 支持基于角色的访问控制（RBAC）和细粒度的权限控制，如方法级别的权限控制和资源级别的访问控制。
   - 可以通过注解或编程方式进行授权检查，保护应用程序中的安全资源。

3. **会话管理（Session Management）**：
   - Shiro 提供了灵活的会话管理功能，支持本地会话和分布式会话管理。
   - 可以管理会话的超时、存储位置、Cookie设置等。

4. **密码加密（Cryptography）**：
   - Shiro 提供了对密码的加密和解密支持，包括常见的哈希算法（如MD5、SHA等）和加密算法（如AES、DES等）。

5. **集成性**：
   - Shiro 可以与常见的Web框架（如Spring MVC、Spring Boot）、IoC容器（如Spring）、持久化框架（如Hibernate）等无缝集成。
   - 也可以与各种身份存储（如数据库、LDAP、文件等）和第三方身份提供者（如OAuth提供者）集成。

6. **易于扩展和定制**：
   - Shiro 的设计允许开发者轻松地扩展和定制各种安全功能，如自定义Realm、自定义过滤器等。

### 核心概念：

- **Subject**：表示正在与应用程序交互的用户，可以是一个人、设备或者其他系统。
- **SecurityManager**：管理所有Subject，并协调身份验证（Authentication）和授权（Authorization）操作。
- **Realm**：用于进行实际的身份验证和授权，连接到应用程序特定的安全数据源，如数据库、LDAP等。
- **SessionManager**：管理Subject的会话，负责会话的创建、删除和管理。

### 示例代码：

#### 1. 配置Shiro

在Spring Boot项目中，可以通过配置类来配置Shiro，例如：

```java
@Configuration
public class ShiroConfig {

    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(SecurityManager securityManager) {
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        shiroFilterFactoryBean.setSecurityManager(securityManager);
        
        // 定义拦截器链
        Map<String, String> filterChainDefinitionMap = new LinkedHashMap<>();
        filterChainDefinitionMap.put("/admin/**", "authc, roles[admin]");
        filterChainDefinitionMap.put("/user/**", "authc, roles[user]");
        
        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterChainDefinitionMap);
        
        return shiroFilterFactoryBean;
    }

    @Bean
    public SecurityManager securityManager(Realm realm) {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        securityManager.setRealm(realm);
        return securityManager;
    }

    @Bean
    public Realm realm() {
        // 实现自定义的Realm
        return new MyRealm();
    }
}
```

#### 2. 编写自定义Realm

```java
public class MyRealm extends AuthorizingRealm {

    // 实现身份验证逻辑
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        // 获取用户名密码
        UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
        String username = token.getUsername();
        // 从数据库或其他数据源获取用户信息
        // 如果用户不存在，可以抛出UnknownAccountException
        // 如果密码错误，可以抛出IncorrectCredentialsException
        // 否则，返回认证信息
        return new SimpleAuthenticationInfo(username, "password", getName());
    }

    // 实现授权逻辑
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        // 获取当前用户的角色和权限信息
        SimpleAuthorizationInfo authorizationInfo = new SimpleAuthorizationInfo();
        authorizationInfo.addRole("admin");
        authorizationInfo.addStringPermission("user:create");
        return authorizationInfo;
    }
}
```

#### 3. 使用Shiro进行安全控制

在控制器中使用Shiro注解进行安全控制：

```java
@RestController
@RequestMapping("/admin")
@RequiresRoles("admin")
public class AdminController {

    @GetMapping("/dashboard")
    public String adminDashboard() {
        return "Admin Dashboard";
    }
}
```

### 总结

Apache Shiro 是一个功能强大且易于集成的安全框架，提供了身份验证、授权、加密和会话管理等核心功能。通过Shiro，开发者可以轻松地为Java应用程序添加安全性，保护应用程序中的资源和数据。Shiro 的灵活性和易用性使其成为Java开发中首选的安全框架之一。