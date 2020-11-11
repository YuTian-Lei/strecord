# Spring security

## 总体架构

- 集成
- config配置：配置用户认证和权限控制
  - 用户认证和权限控制（默认实现）
  - 用户认证和权限控制（自定义实现）

- 自定义实现用户认证和权限控制
- PreAuthorize注解使用

## 权限认证方法

- HTTP BASIC authentication headers (一个基于IEFT  RFC 的标准)
- HTTP Digest authentication headers (一个基于IEFT  RFC 的标准)
- HTTP X.509 client certificate exchange  (一个基于IEFT RFC 的标准)
- [LDAP](http://www.oschina.net/project/tag/180/ldap) (一个非常常见的跨平台认证需要做法，特别是在大环境)
- FormLogin (提供简单用户接口的需求)

### 自定义用户认证和权限控制

#### 用户认证

- **UserDetails** 
- **UserDetailsService** 
- **AuthenticationProvider** 

#### 权限控制

- **FilterInvocationSecurityMetadataSource**：自定义权限数据源
- **AccessDecisionManager** ：自定义鉴权管理器
- **AbstractSecurityInterceptor** ：资源访问过滤器



