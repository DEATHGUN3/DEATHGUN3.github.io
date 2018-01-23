
## 权限系统简介
- 几乎所有的权限系统都分为两个部分，一个是分配权限部分，一个是验证权限部分，为了理解它们，首先解释两个基本的名词:安全实体和权限。
> - **安全实体:**就是被权限系统保护的对象，比如工资数据。
> - **权限:**就是需要被校验的权限对象，比如查看，修改等。
- 分配权限和验证权限的含义:
> - **分配权限:**把对某些安全实体的某些权限分配给某些人员的过程。
> - **验证权限:**判断某个人员或程序对某个安全实体是否拥有某个或某些权限的过程。

在目前应用系统的开发中，多数是利用数据库来存放授权过程产生的数据，也就是说:分配权限是向数据库中添加数据、或是维护数据的过程，而验证权限过程就变成了从数据库中获取相应数据进行匹配的过程。

## Shiro简介
- Apache Shiro是Java的一个安全(权限)框架。
- Shiro可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE环境，也可以用在JavaEE环境。
- Shiro可以完成:认证、授权、加密、会话管理、与Web集成、缓存等。
- [Shiro官网](http://shiro.apache.org/)
- [Shiro的GitHub](https://github.com/apache/shiro)

## Shiro能干嘛
- **认证:**验证用户来核实他们的身份
- **授权:**对用户执行访问控制，如:判断用户是否被分配了一个确定的安全角色，判断用户是否被允许做某事
- **会话管理:**在任何环境下使用Session API，即使没有Web或EJB容器
- **加密:**以更简洁易用的方式使用加密的功能，保护或隐藏数据防止被偷窥
- **Realms:**聚集一个或多个用户安全数据的数据源，并作为一个单一的复合用户的"视图"
- 可以启动**单点登录(SSO)**功能
- 可以为没有关联到登录的用户启用"Remember Me"服务

## Shiro有什么
基本功能点如下图所示:
![](/img/shiro/shiro-architecture1.png)
Shiro的四大核心部分——身份认证，授权，会话管理和加密。
- `Authentication`:<font color=#0000CD>身份认证/登录</font>，验证用户是不是拥有相应的身份；
- <font color=#FF0000>Authorization</font>:<font color=#0000CD>授权，即权限认证</font>，验证某个已认证的用户是否拥有某个权限；即判断用户是否能进行什么操作，如:验证某个用户是否拥有某个角色。或者细粒度地验证某个用户对某个资源是否具有某个权限。
- <font color=#FF0000>Session Manager</font>:<font color=#0000CD>会话管理</font>，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；
<font color=#0000CD>会话可以是普通JavaSE环境，也可以是Web环境</font>；
- <font color=#FF0000>Cryptograhy</font>:<font color=#0000CD>加密</font>，保护数据的安全性，如密码加密存储到数据库，而不是明文存储。

除了以上四大核心功能，shiro还提供很多扩展:
- <font color=#FF0000>Web Support</font>:<font color=#0000CD>Web支持</font>，可以非常容易地集成到Web环境；
- <font color=#FF0000>Caching</font>:<font color=#0000CD>缓存</font>，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率；
- <font color=#000000>Concurrency</font>:Shiro支持<font color=#0000CD>多线程应用的并发认证</font>，即如在一个线程中开启另外一个线程，能把权限自动传播下去
- <font color=#000000>Testing</font>:提供<font color=#0000CD>测试</font>支持；
- <font color=#FF0000>Run As</font>:<font color=#0000CD>允许一个用户假装为另一个用户</font>(如果他们允许)<font color=#0000CD>的身份进行访问</font>；
- <font color=#FF0000>Remember Me</font>:<font color=#0000CD>记住我</font>，这个是非常常见的功能，即一次登录后，下次再来的话就不用登录了。





