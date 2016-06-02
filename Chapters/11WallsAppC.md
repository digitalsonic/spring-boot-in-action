# appendix C Configuration properties
# 附录C 配置属性

Although Spring Boot handles a lot of the grunt work when it comes to configuring the components in your application, you may want to fine-tune some of those components. That’s where configuration properties come in handy.  
虽然Spring Boot在应用程序配置组件时处理了很多“粗活”，但你可能还是想对其中某些组件进行微调。这时就该配置属性登场了。
Chapter 3 describes the @ConfigurationProperties annotation and how it can be used to expose properties that you can configure external to application code. Just as you can use @ConfigurationProperties in components that you create, many of Spring Boot’s auto-configured components are also annotated with @ConfigurationProperties, making it possible to configure them via any supported property source.  
第3章介绍了`@ConfigurationProperties`注解，以及它是如何将配置在代码外部的属性暴露出来的。正如你可以在你创建的组件上使用`@ConfigurationProperties`注解那样，很多Spring Boot自动配置的组件也添加了`@ConfigurationProperties`注解，可以通过Spring Boot支持的各种属性源对其进行配置。For example, to specify the port that an embedded Tomcat or Jetty server should listen for requests on, you can set the server.port property. This can be set as a property in application.properties, in application.yml, in an operating system environment variable, or any of the other options listed in section 3.2.  
例如，要指定内嵌的Tomcat或Jetty服务器要监听的端口，就可以设置`server.port`属性。这个属性可以设置在application.properties、application.yml、操作系统环境变量，或者是3.2节列出的其他地方里。This appendix lists all of the configuration properties offered by Spring Boot components. Note that the applicability of these properties is dependent upon the component being declared as a bean in the Spring application context (most likely by way of auto-configuration). Setting a property for an inactive component will have no effect.  
本附录列出了Spring Boot组件提供的全部配置属性。请注意，这些属性的是否生效要取决于对应的组件是否被声明为Spring应用程序上下文里的Bean（基本是自动配置的），设置一个不生效组件的属性是没用的。

* `flyway.baseline-description`  The description to tag an existing schema with when executing baseline.  
执行基线时用来标记已有Schema的描述。* `flyway.baseline-on-migrate`  Whether to automatically call baseline when migrate is executed against a non-empty schema with no metadata table. (Default value: false)  在没有元数据表的情况下，针对非空Schema执行迁移时是否自动调用基线。（默认值：`false`）* `flyway.baseline-version`  Sets the version to tag an existing schema with when executing baseline. (Default value: 1)  
执行基线时用来标记已有Schema的版本。（默认值：`1`）
* `flyway.check-location`  
Check that migration scripts location exists. (Default value: false)  
检查迁移脚本所在的位置是否存在。（默认值：`false`）
* `flyway.clean-on-validation-error`  
Whether to automatically call clean or not when a validation error occurs. (Default value: false)  
在验证错误时，是否自动执行清理。（默认值：`false`）
* `flyway.enabled`  
Enable flyway. (Default value: true)  
开启Flyway。（默认值：`true`）
* `flyway.encoding`  
Sets the SQL migration encoding. (Default value: UTF-8)  
设置SQL迁移文件的编码。（默认值：`UTF-8`）
* `flyway.ignore-failed-future-migration`  
Whether to ignore failed future migrations when reading the metadata table. (Default value: false)  
在读元数据表时，是否忽略失败的后续迁移。（默认值：`false`）
* `flyway.init-sqls`  
SQL statements to execute to initialize a connection immediately after obtaining it.  
获取连接后立即执行做初始化的SQL语句。
* `flyway.locations`  
Locations of migrations scripts. (Default value: db/migration)  
迁移脚本的位置。（默认值：`db/migration`）
* `flyway.out-of-order`  
Whether or not “out of order” migrations are allowed. (Default value: false)  
是否允许“乱序”（out of order）迁移。（默认值：`false`）
* `flyway.password`  
Login password of the database to migrate.  
待迁移数据库的登录密码。
* `flyway.placeholder-prefix`  
Sets the prefix of every placeholder. (Default value: ${)  
设置每个占位符的前缀。（默认值：`${`）
* `flyway.placeholder-replacement`  
Whether placeholders should be replaced. (Default value: true)  
是否要替换占位符。（默认值：`true`）
* `flyway.placeholder-suffix`  
Sets the prefix of every placeholder. (Default value: ${)  
设置占位符的后缀。（默认值：`}`）
* `flyway.placeholders.[placeholder name]`  
Sets a placeholder value.  
设置占位符的值。
* `flyway.schemas`  
A case-sensitive list of schemes managed by Flyway. Defaults to the default schema of the connection.  
Flyway管理的Schema列表，大小写敏感。默认是连接对应的默认Schema。
* `flyway.sql-migration-prefix`  
The filename prefix for SQL migrations. (Default value: V)  
SQL迁移的文件名前缀。（默认值：`V`）
* `flyway.sql-migration-separator`  
The filename separator for SQL migrations. (Default value: \__)  
SQL迁移的文件名分隔符。（默认值：`__`）
* `flyway.sql-migration-suffix`  
The filename suffix for SQL migrations. (Default value: .sql)  
SQL迁移的文件名后缀。（默认值：`.sql`）
* `flyway.table`  
The name of the schema metadata table to be used by Flyway. (Default value: schema_version)  
Flyway使用的Schema元数据表名称。（默认值：`schema_version`）
* `flyway.target`  
The target version up to which Flyway should consider migrations. (Defaults to the latest version)  
Flyway要迁移到的目标版本号。（默认是最新版本）
* `flyway.url`  
JDBC URL of the database to migrate. If not set, the primary configured data source is used.  
待迁移的数据库的JDBC URL。如果没有设置，就使用配置的主数据源。
* `flyway.user`  
Login user of the database to migrate.  
待迁移数据库的登录用户。
* `flyway.validate-on-migrate`  
Whether to automatically validate when running migrate. (Default value: true)  
在运行迁移时是否要自动验证。（默认值：`true`）
* `liquibase.change-log`  
Change log configuration path. (Default value: classpath:/db/changelog/db.changelog-master.yaml)  
变更日志配置路径。（默认值：`classpath:/db/changelog/db.changelog-master.yaml`）
* `liquibase.check-change-log-location`  
Check that the change log location exists. (Default value: true)  
检查变更日志位置是否存在。（默认值：`true`）
* `liquibase.contexts`  
Comma-separated list of runtime contexts to use.  
要使用的运行时上下文列表，用逗号分隔。
* `liquibase.default-schema`  
Default database schema.  
默认的数据库Schema。
* `liquibase.drop-first`  
Drop the database schema first. (Default value: false)  
先删除数据库Schema。（默认值：`false`）
* `liquibase.enabled`  
Enable Liquibase support. (Default value: true)  
开启Liquibase支持。（默认值：`true`）
* `liquibase.password`  
Login password of the database to migrate.  
待迁移数据库的登录密码。
* `liquibase.url`  
JDBC URL of the database to migrate. If not set, the primary configured data source is used.  
待迁移数据库的JDBC URL。如果没有设置，就使用配置的主数据源。
* `liquibase.user`  
Login user of the database to migrate.
待迁移数据库的登录用户。
* `multipart.enabled`  
Enable support of multi-part uploads. (Default value: true)  
开启分段（multi-part）上传支持。（默认值：`true`）
* `multipart.file-size-threshold`  
Threshold after which files will be written to disk. Values can use the suffixes “MB” or “KB” to indicate a megabyte or kilobyte size. (Default value: 0)  
大于该阈值的文件会被写到磁盘上。这里的值可以使用“MB”或“KB”的后缀来表明是兆字节还是千字节。（默认值：`0`）
* `multipart.location`  
Intermediate location of uploaded files.  
上传文件的中间存放位置。
* `multipart.max-file-size`  
Max file size. Values can use the suffixes “MB” or “KB” to indicate a megabyte or kilobyte size. (Default value: 1MB)  
最大文件大小。这里的值可以使用“MB”或“KB”的后缀来表明是兆字节还是千字节。（默认值：`1MB`）
* `multipart.max-request-size`  
Max request size. Values can use the suffixes “MB” or “KB” to indicate a megabyte or kilobyte size. (Default value: 10MB)  
最大请求大小。这里的值可以使用“MB”或“KB”的后缀来表明是兆字节还是千字节。（默认值：`10MB`）
* `security.basic.authorize-mode`  
Security authorize mode to apply.
要运用的安全授权模式。
* `security.basic.enabled`  
Enable basic authentication. (Default value: true)  
开启基本身份验证。（默认值：`true`）
* `security.basic.path`  
Comma-separated list of paths to secure. (Default value: [/\**])  
要保护的路径，用逗号分隔。（默认值：`[/**]`）
* `security.basic.realm`  
HTTP basic realm name. (Default value: Spring)  
HTTP基本领域（realm）用户名。（默认值：`Spring`）
* `security.enable-csrf`  
Enable cross-site request forgery support. (Default value: false)  
开启跨站请求伪造（cross-site request forgery）支持。（默认值：`false`）
* `security.filter-order`  
Security filter chain order. (Default value: 0)  
安全过滤器链顺序。（默认值：`0`）
* `security.headers.cache`  
Enable cache control HTTP headers. (Default value: false)  
开启缓存控制HTTP头。（默认值：`false`）
* `security.headers.content-type`  
Enable X-Content-Type-Options header. (Default value: false)  
开启`X-Content-Type-Options`头。（默认值：`false`）
* `security.headers.frame`  
Enable X-Frame-Options header. (Default value: false)  
开启`X-Frame-Options`头。（默认值：`false`）
* `security.headers.hsts`  
HTTP Strict Transport Security (HSTS) mode (none, domain, all).  
HTTP Strict Transport Security（HSTS）模式（`none`、`domain`和`all`）。
* `security.headers.xss`  
Enable cross-site scripting (XSS) protection. (Default value: false)  
开启跨站脚本（cross-site scripting）保护。（默认值：`false`）
* `security.ignored`  
Comma-separated list of paths to exclude from the default secured paths.  
要从默认保护路径中排除掉的路径列表，用逗号分隔。
* `security.oauth2.client.access-token-uri`  
The URI used to fetch an access token.  
用于获取访问令牌的URI。
* `security.oauth2.client.access-token-validity-seconds`  
How long an access token is to be valid before expiring.  
在令牌过期前多长时间验证一次。
* `security.oauth2.client.additional-information.[key]`  
Set additional information that token granters would like to add to the token.  
设置额外的信息，令牌授予者会将其添加到令牌里。
* `security.oauth2.client.authentication-scheme`  
The method for transmitting the bearer token. One of form, header, none, or query. (Default value: header)  
传送持有人令牌（bearer token）的方法。`form`、`header`、`none`或`query`中的一种。（默认值：`header`）
* `security.oauth2.client.authorities`  
The authorities to be granted to an authenticated client.  
要赋予经授权客户端的权限。
* `security.oauth2.client.authorized-grant-types`  
The grant types allowed to the client.  
客户端可用的授予类型。
* `security.oauth2.client.auto-approve-scopes`  
The scope to automatically approve for a client.  
客户端自动通过的范围。
* `security.oauth2.client.client-authentication-scheme`  
The method for transmitting authentication credentials when authenticating the client. One of form, header, none, or query. (Default value: header)  
在客户端身份认证时用于传输身份认证信息的方法。`form`、`header`、`none`或`query`中的一种。（默认值：`header`）
* `security.oauth2.client.client-id`  
OAuth2 client ID.  
OAuth2客户端ID。
* `security.oauth2.client.client-secret`  
OAuth2 client secret. A random secret is generated by default.  
OAuth2客户端密钥。默认是生成的随机密钥。
* `security.oauth2.client.grant-type`  
The grant type for obtaining an access token for this resource.  
获得资源访问令牌的授予类型。
* `security.oauth2.client.id`  
The application’s client ID.  
应用程序的客户端ID。
* `security.oauth2.client.pre-established-redirect-uri`  
The redirect URI that has been pre-established with the server. If present, the redirect URI will be omitted from the user authorization request because the server doesn’t need to know it.  
与服务器预先建立好的重定向URI。如果设置了该属性，用户授权请求中的重定向URI会被忽略，因为服务器不需要它。
* `security.oauth2.client.refresh-token-validity-seconds`  
How long a refresh token will be valid before expiring.  
刷新令牌在过期前的有效时间。
* `security.oauth2.client.registered-redirect-uri`  
Comma-separated list of redirect URIs registered for the client.  
客户端里注册的重定向URI，用逗号分隔。
* `security.oauth2.client.resource-ids`  
Comma-separated list of resource IDs associated with the client.  
与客户端关联的资源ID，用逗号风格。
* `security.oauth2.client.scope`  
Scope assigned to the client.  
客户端分配的域。
* `security.oauth2.client.token-name`  
The token name.  
令牌名称。
* `security.oauth2.client.use-current-uri`  
Whether the current URI (if set) in the request should be used in preference to the pre-established redirect URI. (Default value: true)  
请求里的当前URI（如果设置了的话）是否优先于预建立的重定向URI。（默认值：`true`）
* `security.oauth2.client.user-authorization-uri`  
The URI to which the user is to be redirected to authorize an access token.  
用户要重定向以便授于访问令牌的URI。
* `security.oauth2.resource.id`  
Identifier of the resource.  
资源的标识符。
* `security.oauth2.resource.jwt.key-uri`  
The URI of the JWT token. Can be set if the value is not available and the key is public.  
JWT令牌的URI。如果没有配置`key-value`，使用的又是公钥，可以设置这个属性。
* `security.oauth2.resource.jwt.key-value`  
The verification key of the JWT token. Can either be a symmetric secret or PEMencoded RSA public key. If the value is not available, you can set the URI instead.  
JWT令牌的验证密钥。可以是对称密钥，也可以是PEM编码的RSA公钥。如果没有配置这个属性，可以用`key-uri`代替。
* `security.oauth2.resource.prefer-token-info`  
Use the token info; can be set to false to use the user info. (Default value: true)  
使用令牌的信息；设置为`false`就使用用户信息。（默认值：`true`）
* `security.oauth2.resource.service-id`  
The service ID. (Default value: resource)  
服务ID。（默认值：`resource`）
* `security.oauth2.resource.token-info-uri`  
URI of the token decoding endpoint.  
令牌解码端点URI。
* `security.oauth2.resource.token-type`  
The token type to send when using the userInfoUri.  
在使用`userInfoUri`时发送的令牌类型。
* `security.oauth2.resource.user-info-uri`  
URI of the user endpoint.  
用户端点的URI。
* `security.oauth2.sso.filter-order`  
Filter order to apply if not providing an explicit WebSecurityConfigurerAdapter (otherwise the order can be provided there instead).  
在没有显式提供`WebSecurityConfigurerAdapter`时应用的过滤器顺序，在`WebSecurityConfigurerAdapter`里也可以指定顺序。
* `security.oauth2.sso.login-path`  
Path to the login page—the page that triggers the redirect to the OAuth2 Authorization Server. (Default value: /login)  
登录页的路径——登录页是触发重定向到OAuth2授权服务器的页面。（默认值：`/login`）
* `security.require-ssl`  
Enable secure channel for all requests. (Default value: false)  
对所有请求开启安全通道。（默认值：`false`）
* `security.sessions`  
Session creation policy. (Default values: always, never, if_required, stateless).  
创建会话使用的策略。（可选值：`always`、`never`、`if_required`和`stateless`）
* `security.user.name`  
Default user name. (Default value: user)  
默认的用户名。（默认值：`user`）
* `security.user.password`  
Password for the default user name.  
默认用户的密码。
* `security.user.role`  
Granted roles for the default user name.  
赋予默认用户的角色。
* `server.address`  
Network address to which the server should bind.  
服务器绑定的网络地址。
* `server.compression.enabled`  
Whether or not compression should be enabled. (Default value: false)  
是否要开启压缩。（默认值：`false`）
* `server.compression.excluded-user-agents`  
Comma-separated list of user agents for which responses should not be compressed. (Default values: text/html,text/xml,text/plain,text/css)  
用逗号分割的列表，标明哪些用户代理不该开启压缩。（可选值：`text/html`、`text/xml`、`text/plain`和`text/css`）
* `server.compression.mime-types`  
Comma-separated list of MIME types that should be compressed.  
要开启压缩的MIME类型列表，用逗号分割。
* `server.compression.min-response-size`  
Minimum response size (in bytes) that is required for compression to be performed. (Default value: 2048)  
要执行压缩的最小响应大小（单位为字节）。（默认值：`2048`）
* `server.context-parameters.[param name]`  
Sets a servlet context parameter.  
设置一个Servlet上下文参数。
* `server.context-path`  
Context path of the application.  
应用程序的上下文路径。
* `server.display-name`  
Display name of the application. (Default value: application)  
应用程序的显示名称。（默认值：`application`）
* `server.jsp-servlet.class-name`  
The class name of the servlet to use for JSPs. (Default value: org.apache.jasper.servlet.JspServlet)  
针对JSP使用的Servlet类名。（默认值：`org.apache.jasper.servlet.JspServlet`）
* `server.jsp-servlet.init-parameters.[param name]`  
Sets a JSP servlet initialization parameter.  
设置JSP Servlet初始化参数。
* `server.jsp-servlet.registered`  
Whether or not the JSP servlet should be registered with the embedded servlet container. (Default value: true)  
JSP Servlet是否要注册到内嵌的Servlet容器里。（默认值：`true`）
* `server.port`  
Server HTTP port.  
服务器的HTTP端口。
* `server.servlet-path`  
Path of the main dispatcher servlet. (Default value: /)  
主分发器Servlet的路径。（默认值：`/`）
* `server.session.cookie.comment`  
Comment for the session cookie.  
会话Cookie的注释。
* `server.session.cookie.domain`  
Domain for the session cookie.  
会话Cookie的域。
* `server.session.cookie.http-only`  
HttpOnly flag for the session cookie.  
会话Cookie的`HttpOnly`标记。
* `server.session.cookie.max-age`  
Maximum age of the session cookie in seconds.  
会话Cookie的最大保存时间，单位为秒。
* `server.session.cookie.name`  
Session cookie name.  
会话Cookie名称。
* `server.session.cookie.path`  
Path of the session cookie.  
会话Cookie的路径。
* `server.session.cookie.secure`  
“Secure” flag for the session cookie.  
会话Cookie的`Secure`标记。
* `server.session.persistent`  
Persist session data between restarts. (Default value: false)  
是否在两次重启间持久化会话数据。（默认值：`false`）
* `server.session.timeout`  
Session timeout in seconds.  
会话超时时间，单位为秒。
* `server.session.tracking-modes`  
Session tracking modes (one or more of the following: cookie, url, ssl).  
会话跟踪模式（可选值是其中的一个或多个：`cookie`、`url`和`ssl`）。
* `server.ssl.ciphers`  
Supported SSL ciphers.  
支持的SSL加密算法。
* `server.ssl.client-auth`  
Whether client authentication is wanted (want) or needed (need). Requires a trust store.  
客户端授权是主动想（`want`）还是被动需要（`need`）。要有一个TrustStore。
* `server.ssl.enabled`  
Whether SSL is enabled or not. (Default value: true)  
是否开启SSL。（默认值：`true`）
* `server.ssl.key-alias`  
Alias that identifies the key in the key store.  
在KeyStore里标识密钥的别名。
* `server.ssl.key-password`  
Password used to access the key in the key store.  
在KeyStore里用于访问密钥的密码。
* `server.ssl.key-store`  
Path to the key store that holds the SSL certificate (typically a .jks file).  
持有SSL证书的KeyStore的路径（通常指向一个.jks文件）。
* `server.ssl.key-store-password`  
Password used to access the key store.  
访问KeyStore时使用的密钥。
* `server.ssl.key-store-provider`  
Provider for the key store.  
KeyStore的提供者。
* `server.ssl.key-store-type`  
Type of the key store.  
KeyStore的类型。
* `server.ssl.protocol`  
SSL protocol to use. (Default value: TLS)  
要使用的SSL协议。（默认值：`TLS`）
* `server.ssl.trust-store`  
Trust store that holds SSL certificates.  
持有SSL证书的TrustStore。
* `server.ssl.trust-store-password`  
Password used to access the trust store.  
用于访问TrustStore的密码。
* `server.ssl.trust-store-provider`  
Provider for the trust store.  
TrustStore的提供者。
* `server.ssl.trust-store-type`  
Type of the trust store.  
TrustStore的类型。
* `server.tomcat.access-log-enabled`  
Whether or not the access log is enabled. (Default value: false)  
是否开启访问日志。（默认值：`false`）
* `server.tomcat.access-log-pattern`  
Format pattern for access logs. (Default value: common)  
访问日志的格式。（默认值：`common`）
* `server.tomcat.accesslog.directory`  
Directory in which log files are created. Can be relative to the tomcat base dir or absolute. (Default value: logs)  
创建日志文件的目录。可以是相对于Tomcat基础目录的，也可以是绝对路径。（默认值：`logs`）
* `server.tomcat.accesslog.enabled`  
Enable access log. (Default value: false)  
开启访问日志。（默认值：`false`）
* `server.tomcat.accesslog.pattern`  
Format pattern for access logs. (Default value: common)  
访问日志的格式。（默认值：`common`）
* `server.tomcat.accesslog.prefix`  
Log filename prefix. (Default value: access_log)  
日志文件名的前缀。（默认值：`access_log`）
* `server.tomcat.accesslog.suffix`  
Log filename suffix. (Default value: .log)  
日志文件名的后缀。（默认值：`.log`）
* `server.tomcat.background-processor-delay`  
Delay in seconds between the invocation of backgroundProcess methods. (Default value: 30)  
两次调用`backgroundProcess`方法之间的延迟时间，单位为秒。（默认值：`30`）
* `server.tomcat.basedir`  
Tomcat base directory. If not specified, a temporary directory will be used.  
Tomcat的基础目录。如果没有指定则会使用一个临时目录。
* `server.tomcat.internal-proxies`  
Regular expression that matches proxies that are to be trusted. Default:“10\.\d{1,3}\.\d{1,3}\.\d{1,3}| 192\.168\.\d{1,3}\.\d{1,3}| 169\.254\.\d{1,3}\.\d{1,3}| 127\.\d{1,3}\.\d{1,3}\.\d{1,3}| 172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}| 172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}”  
用于匹配可信任的代理服务器的正则表达式。默认值：“10\.\d{1,3}\.\d{1,3}\.\d{1,3}| 192\.168\.\d{1,3}\.\d{1,3}| 169\.254\.\d{1,3}\.\d{1,3}| 127\.\d{1,3}\.\d{1,3}\.\d{1,3}| 172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}| 172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}”
* `server.tomcat.max-http-header-size`  
Maximum size in bytes of the HTTP message header. (Default value: 0)  
HTTP消息头的最大字节数。（默认值：`0`）
* `server.tomcat.max-threads`  
Maximum number of worker threads. (Default value: 0)  
最大工作线程数。（默认值：`0`）
* `server.tomcat.port-header`  
Name of the HTTP header used to override the original port value.  
用来覆盖原始端口值的HTTP头的名字。
* `server.tomcat.protocol-header`  
Header that holds the incoming protocol, usually named X-Forwarded-Proto. Configured as a RemoteIpValve only if remoteIpHeader is also set.  
持有流入协议的HTTP头，通常的名字是`X-Forwarded-Proto`。仅当设置了`remoteIpHeader`的时候，它会被配置为`RemoteIpValve`。
* `server.tomcat.protocol-header-https-value`  
Value of the protocol header that indicates that the incoming request uses SSL. (Default value: https)  
表明流入请求使用了SSL的协议头的值。（默认值：`https`）
* `server.tomcat.remote-ip-header`  
Name of the HTTP header from which the remote IP is extracted. Configured as a RemoteIpValve only if remoteIpHeader is also set.  
表明从哪个HTTP头里可以提取到远端IP。仅当设置了`remoteIpHeader`的时候，它会被配置为`RemoteIpValve`。
* `server.tomcat.uri-encoding`  
Character encoding to use to decode the URI.  
用来解码URI的字符编码。
* `server.undertow.access-log-dir`  
Undertow access log directory. (Default value: logs)  
Undertow的访问日志目录。（默认值：`logs`）
* `server.undertow.access-log-enabled`  
Whether or not the access log is enabled. (Default value: false)  
是否开启访问日志。（默认值：`false`）
* `server.undertow.access-log-pattern`  
Format pattern for access logs. (Default value: common)  
访问日志的格式。（默认值：`common`）
* `server.undertow.accesslog.dir`  
Undertow access log directory.  
Undertow访问日志目录。
* `server.undertow.accesslog.enabled`  
Enable access log. (Default value: false)  
开启访问日志。（默认值：`false`）
* `server.undertow.accesslog.pattern`  
Format pattern for access logs. (Default value: common)
访问日志的格式。（默认值：`common`）
* `server.undertow.buffer-size`  
Size of each buffer in bytes.  
每个缓冲的字节数。
* `server.undertow.buffers-per-region`  
Number of buffers per region.  
每个区（region）的缓冲数。
* `server.undertow.direct-buffers`  
Allocate buffers outside the Java heap.  
在Java堆外分配缓冲。
* `server.undertow.io-threads`  
Number of I/O threads to create for the worker.  
要为工作线程创建的I/O线程数。
* `server.undertow.worker-threads`  
Number of worker threads.  
工作线程数。
* `spring.activemq.broker-url`  
URL of the ActiveMQ broker. Auto-generated by default.  
ActiveMQ代理的URL。默认是自动生成的。
* `spring.activemq.in-memory`  
Specify if the default broker URL should be in memory. Ignored if an explicit broker has been specified. (Default value: true)  
标明默认代理URL是否应该在内存里。如果指定了一个显式的代理则忽略该属性。（默认值：`true`）
* `spring.activemq.password`  
Login password of the broker.  
代理的登录密码。
* `spring.activemq.pooled`  
Specify if a PooledConnectionFactory should be created instead of a regular ConnectionFactory. (Default value: false)  
标明是否要创建一个`PooledConnectionFactory`来代替普通的`ConnectionFactory`。（默认值：`false`）
* `spring.activemq.user`  
Login user of the broker.  
代理的登录用户名。
* `spring.aop.auto`  
Add @EnableAspectJAutoProxy. (Default value: true)  
添加`@EnableAspectJAutoProxy`。（默认值：`true`）
* `spring.aop.proxy-target-class`  
Whether subclass-based (CGLIB) proxies are to be created (true) as opposed to standard Java interface-based proxies (false). (Default value: false)  
是否要创建基于子类（CGLIB）的代理来代替基于Java接口的代理，前者为`true`，后者为`false`。（默认值：`false`）
* `spring.application.admin.enabled`  
Enable admin features for the application. (Default value: false)  
开启应用程序的管理功能。（默认值：`false`）
* `spring.application.admin.jmx-name`  
JMX name of the application admin MBean. (Default value: org.springframework.boot:type=Admin,name=SpringApplication)  
应用程序的管理MBean的JMX名称。（默认值：`org.springframework.boot:type=Admin,name=SpringApplication`）
* `spring.artemis.embedded.cluster-password`  
Cluster password. Randomly generated on startup by default.  
集群密码。默认在启动时随机生成。
* `spring.artemis.embedded.data-directory`  
Journal file directory. Not necessary if persistence is turned off.  
Journal文件目录。如果关闭了持久化则不需要该属性。
* `spring.artemis.embedded.enabled`  
Enable embedded mode if the Artemis server APIs are available. (Default value: true)  
如果有Artemis服务器API则开启嵌入模式。（默认值：`true`）
* `spring.artemis.embedded.persistent`  
Enable persistent store. (Default value: false)  
开启持久化存储。（默认值：`false`）
* `spring.artemis.embedded.queues`  
Comma-separated list of queues to create on startup. (Default value: [])  
要在启动时创建的队列列表，用逗号分隔。（默认值：`[]`）
* `spring.artemis.embedded.server-id`  
Server ID. By default, an auto-incremented counter is used. (Default value: 0)  
服务器ID。默认会使用一个自动递增的计数器。（默认值：`0`）
* `spring.artemis.embedded.topics`  
Comma-separated list of topics to create on startup. (Default value: [])  
在启动时要创建的主题列表，用逗号分隔。（默认值：`[]`）
* `spring.artemis.host`  
Artemis broker host. (Default value: localhost)  
Artemis代理主机。（默认值：`localhost`）
* `spring.artemis.mode`  
Artemis deployment mode, auto-detected by default. Can be explicitly set to native or embedded.  
Artemis部署模式，默认会自动检测。可以显式地设置为`native`或`embedded`。
* `spring.artemis.port`  
Artemis broker port. (Default value: 61616)  
Artemis代理端口。（默认值：`61616`）
* `spring.autoconfigure.exclude`  
Auto-configuration classes to exclude.  
要排除的自动配置类。
* `spring.batch.initializer.enabled`  
Create the required batch tables on startup if necessary. (Default value: true)  
如果需要的话，在启动时创建需要的批处理表。（默认值：`true`）
* `spring.batch.job.enabled`  
Execute all Spring Batch jobs in the context on startup. (Default value: true)  
在启动时执行上下文里的所有Spring Batch任务。（默认值：`true`）
* `spring.batch.job.names`  
Comma-separated list of job names to execute on startup. By default, all jobs found in the context are executed.  
启动时要执行的任务名列表，用逗号分隔。默认在上下文里找到的所有任务都会执行。
* `spring.batch.schema`  
Path to the SQL file to use to initialize the database schema. (Default value: classpath:org/springframework/batch/core/schema-@@platform@@.sql)  
指向初始化数据库Schema用的SQL文件的路径。（默认值：`classpath:org/springframework/batch/core/schema-@@platform@@.sql`）
* `spring.batch.table-prefix`  
Table prefix for all the batch metadata tables.  
所有批处理元数据表的表前缀。
* `spring.cache.cache-names`  
Comma-separated list of cache names to create if supported by the underlying cache manager. Usually this disables the ability to create additional caches on the fly.  
如果底层缓存管理器支持缓存名的话，可以在这里指定要创建的缓存名列表，用逗号分隔。通常这会禁用运行时创建其他额外缓存的能力。
* `spring.cache.ehcache.config`  
The location of the configuration file to use to initialize EhCache.  
用来初始化EhCache的配置文件的位置。
* `spring.cache.guava.spec`  
The spec to use to create caches. Check CacheBuilderSpec for more details on the spec format.  
用来创建缓存的Spec。有关Spec格式的详细情况，可以查看`CacheBuilderSpec`。
* `spring.cache.hazelcast.config`  
The location of the configuration file to use to initialize Hazelcast.  
用来初始化Hazelcast的配置文件的位置。
* `spring.cache.infinispan.config`  
The location of the configuration file to use to initialize Infinispan.  
用来初始化Infinispan的配置文件的位置。
* `spring.cache.jcache.config`  
The location of the configuration file to use to initialize the cache manager. The configuration file is dependent on the underlying cache implementation.  
用来初始化缓存管理器的配置文件的位置。配置文件依赖于底层的缓存实现。
* `spring.cache.jcache.provider`  
Fully qualified name of the CachingProvider implementation to use to retrieve the JSR-107 compliant cache manager. Only needed if more than one JSR-107 implementation is available on the classpath.  
`CachingProvider`实现的全限定类名，用来获取JSR-107兼容的缓存管理器，只在Classpath里有JSR-107实现时才需要这个属性。
* `spring.cache.type`  
Cache type, auto-detected according to the environment by default.  
缓存类型，默认会根据环境来做自动检测。
* `spring.dao.exceptiontranslation.enabled`  
Enable the PersistenceExceptionTranslationPostProcessor. (Default value: true)  
打开`PersistenceExceptionTranslationPostProcessor`。（默认值：`true`）
* `spring.data.elasticsearch.cluster-name`  
Elasticsearch cluster name. (Default value: elasticsearch)  
Elasticsearch集群名。（默认名：`elasticsearch`）
* `spring.data.elasticsearch.cluster-nodes`  
Comma-separated list of cluster node addresses. If not specified, starts a client node.  
集群节点地址列表，用逗号分隔。如果没有指定，就启动一个客户端节点。
* `spring.data.elasticsearch.properties`  
Additional properties used to configure the client.  
用来配置客户端的额外属性。
* `spring.data.elasticsearch.repositories.enabled`  
Enable Elasticsearch repositories. (Default value: true)  
开启Elasticsearch仓库。（默认值：`true`）
* `spring.data.jpa.repositories.enabled`  
Enable JPA repositories. (Default value: true)  
开启JPA仓库。（默认值：`true`）
* `spring.data.mongodb.authentication-database`  
Authentication database name.  
身份认证数据库名。
* `spring.data.mongodb.database`  
Database name.  
数据库名。
* `spring.data.mongodb.field-naming-strategy`  
Fully qualified name of the FieldNamingStrategy to use.  
要使用的`FieldNamingStrategy`的全限定名。
* `spring.data.mongodb.grid-fs-database`  
GridFS database name.  
GridFS数据库名称。
* `spring.data.mongodb.host`  
Mongo server host.  
Mongo服务器主机地址。
* `spring.data.mongodb.password`  
Login password of the Mongo server.  
Mongo服务器的登录密码。
* `spring.data.mongodb.port`  
Mongo server port.  
Mongo服务器的端口号。
* `spring.data.mongodb.repositories.enabled`  
Enable Mongo repositories. (Default value: true)  
开启Mongo仓库。（默认值：`true`）
* `spring.data.mongodb.uri`  
Mongo database URI. When set, the host and port are ignored. (Default value: mongodb://localhost/test)  
Mongo数据库URI。设置了该属性后就会忽略主机和端口号。（默认值：`mongodb://localhost/test`）
* `spring.data.mongodb.username`  
Login user of the Mongo server.  
Mongo服务器的登录用户名。
* `spring.data.rest.base-path`  
The base path to expose repository resources under.  
用于发布仓库资源的基本路径。
* `spring.data.rest.default-page-size`  
The default size of a page in paged data. (Default value: 20)  
分页数据的默认页大小。（默认值：`20`）
* `spring.data.rest.limit-param-name`  
The name of the URL query string parameter that indicates how many results to return at once. (Default value: size)  
用于标识一次返回多少记录的URL查询字符串参数名。（默认值：`size`）
* `spring.data.rest.max-page-size`  
The maximum size of pages. (Default value: 1000)  
最大分页大小。（默认值：`1000`）
* `spring.data.rest.page-param-name`  
The name of the URL query string parameter that indicates what page to return. (Default value: page)  
URL查询字符串参数的名称，用来标识返回哪一页。（默认值：`page`）
* `spring.data.rest.return-body-on-create`  
Whether to return a response body after creating an entity. (Default value: false)  
在创建实体后是否要返回一个响应体。（默认值：`false`）
* `spring.data.rest.return-body-on-update`  
Whether to return a response body after updating an entity. (Default value: false)  
在更新实体后是否要返回一个响应体。（默认值：`false`）
* `spring.data.rest.sort-param-name`  
The name of the URL query string parameter that indicates what direction to sort results. (Default value: sort)  
URL查询字符串参数的名称，用来标识结果排序的方向。（默认值：`sort`）
* `spring.data.solr.host`  
Solr host. Ignored if zk-host is set. (Default value: http://127.0.0.1:8983/solr)  
Solr的主机地址。如果设置了`zk-host`则忽略该属性。（默认值：`http://127.0.0.1:8983/solr`）
* `spring.data.solr.repositories.enabled`  
Enable Solr repositories. (Default value: true)  
开启Solr仓库。（默认值：`true`）
* `spring.data.solr.zk-host`  
ZooKeeper host address in the form HOST:PORT.  
ZooKeeper主机地址，格式为HOST:PORT。
* `spring.datasource.abandon-when-percentage-full`  
The percentage threshold above which connections that have been abandoned (timed out) will be closed and reported.  
一个百分比形式的阈值，超过该阈值后会关闭并报告被弃用（超时）的连接。
* `spring.datasource.allow-pool-suspension`  
Whether or not pool suspension is allowed. There is a performance impact when pool suspension is enabled. Unless you need it (for a redundancy system, for example) do not enable it. This property only applies when using the Hikari data pool. (Default value: false)  
是否允许池暂停（pool suspension）。在开启池暂停后会有一定性能影响，除非你真的需要这个功能（例如一个冗余的系统），否则不要开启它。该属性只在使用Hikari数据库连接池时有用。（默认值：`false`）
* `spring.datasource.alternate-username-allowed`  
Whether or not an alternate username is allowed.  
是否允许使用其他用户名。
* `spring.datasource.auto-commit`  
Whether or not updates are auto-committed.  
更新操作是否是自动提交的。
* `spring.datasource.catalog`  
The default catalog name.  
默认的Catalog名称。
* `spring.datasource.commit-on-return`  
Whether or not the connection pool should commit any pending transaction when a connection is returned.  
在连接归还时，连接池是否要提交挂起的事务。
* `spring.datasource.connection-init-sql`  
A SQL string that will be executed on all new connections when they are created, before they are added to the connection pool.  
在所有新连接创建时都会执行的SQL语句，该语句会在连接被加入连接池前执行。
* `spring.datasource.connection-init-sqls`  
A list of SQL statements to be executed when a physical connection is first created. (For use with the DBCP connection pool.)  
在物理连接第一次创建时执行的SQL语句列表。（用于DBCP连接池。）
* `spring.datasource.connection-properties.[key]`  
Sets a property to be used when creating a connection. (For the DBCP connection pool.)  
设置创建连接时使用的属性。（用于DBCP连接池。）
* `spring.datasource.connection-test-query`  
A SQL query to be executed to test the validity of connections.  
用于测试连接有效性的SQL查询。
* `spring.datasource.connection-timeout`  
The connection timeout (in milliseconds).  
连接超时（单位为毫秒）。
* `spring.datasource.continue-on-error`  
Do not stop if an error occurs while initializing the database. (Default value: false)  
初始化数据库时如果发生错误不要终止。（默认值：`false`）
* `spring.datasource.data`  
Data (DML) script resource reference.  
指向数据（DML）脚本资源的引用。
* `spring.datasource.data-source-class-name`  
The fully qualified class name of the data source to use to get connections.  
用于获取连接的数据源的全限定类名。
* `spring.datasource.data-source-jndi`  
The JNDI location of the data source to use to get connections.  
用于获取连接的数据源的JNDI位置。
* `spring.datasource.data-source-properties.[key]`  
Sets a property to be used when creating the data source. (For the Hikari connection pool.)  
设置创建数据源时使用的属性。（用于Hikari连接池。）
* `spring.datasource.db-properties`  
Sets a property to be used when creating the data source. (For the Tomcat connection pool.)  
设置创建数据源时使用的属性。（用于Tomcat连接池。）
* `spring.datasource.default-auto-commit`  
Whether or not to auto-commit on connections.  
连接上的操作是否自动提交。
* `spring.datasource.default-catalog`  
The default catalog for connections.  
连接的默认Catalog。
* `spring.datasource.default-read-only`  
The default read-only state for connections.  
连接的默认只读状态。
* `spring.datasource.default-transaction-isolation`  
The default transaction isolation for connections.  
连接的默认事务隔离级别。
* `spring.datasource.driver-class-name`  
Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.  
JDBC驱动的全限定类名。默认会根据URL自动检测。
* `spring.datasource.fair-queue`  
Whether or not to return connections in a FIFO fashion.  
是否以FIFO方式返回连接。
* `spring.datasource.health-check-properties.[key]`  
Sets a property to be included in the health check. (For the Hikari connection pool.)  
设置要纳入健康检查的属性。（用于Hikari连接池。）
* `spring.datasource.idle-timeout`  
The maximum amount of time (in milliseconds) that a connection is allowed to sit idle in the pool. (Default value: 10)  
连接池中的连接能保持闲置状态的最长时间，单位为毫秒。（默认值：`10`）
* `spring.datasource.ignore-exception-on-pre-load`  
Whether or not to ignore connections while initializing the datasource pool.  
初始化数据库连接池时是否要忽略连接。
* `spring.datasource.init-sql`  
A custom query to run when a connection is first created.  
在连接第一次创建时运行的自定义查询。
* `spring.datasource.initial-size`  
The number of connections that will be established when the connection pool is started.  
在连接池启动时要建立的连接数。
* `spring.datasource.initialization-fail-fast`  
Whether or not the construction of the pool should throw an exception if the minimum number of connections cannot be created. (Default value: true)  
在连接池创建时，如果达不到最小连接数是否要抛出异常。（默认值：`true`）
* `spring.datasource.initialize`  
Populate the database using data.sql. (Default value: true)  
使用data.sql来初始化数据库。（默认值：`true`）
* `spring.datasource.isolate-internal-queries`  
Whether internal queries should be isolated. (Default value: false)  
是否要隔离内部请求。（默认值：`false`）
* `spring.datasource.jdbc-interceptors`  
A semicolon-separated list of classnames extending the JdbcInterceptor class. These interceptors will be inserted as an interceptor into the chain of operations on a java.sql.Connection object. (For the Tomcat connection pool.)  
一个分号分隔的类名列表，这些类都扩展了`JdbcInterceptor`类。这些拦截器会被插入`java.sql.Connection`对象的操作链里。（用于Tomcat连接池。）
* `spring.datasource.jdbc-url`  
The JDBC URL to create connections with.  
用来创建连接的JDBC URL。
* `spring.datasource.jmx-enabled`  
Enable JMX support (if provided by the underlying pool). (Default value: false)  
开启JMX支持（如果底层连接池提供该功能的话）。（默认值：`false`）
* `spring.datasource.jndi-name`  
JNDI location of the datasource. Class, URL, username, and password are ignored when set.  
数据源的JNDI位置。设置了该属性后就会忽略类、URL、用户名和密码属性。
* `spring.datasource.leak-detection-threshold`  
The threshold, in milliseconds, for detecting connection leaks with the Hikari connection pool.  
用来检测Hikari连接池连接泄露的阈值，单位为毫秒。
* `spring.datasource.log-abandoned`  
Whether to log stack traces for application code that abandoned a statement or connection. For use with the DBCP connection pool. (Default value: false)  
是否针对弃用语句或连接的应用程序代码记录下跟踪栈。用于DBCP连接池。（默认值：`false`）
* `spring.datasource.log-validation-errors`  
Whether validation errors should be logged when using the Tomcat connection pool.  
在使用Tomcat连接池时是否要记录验证错误。
* `spring.datasource.login-timeout`  
The timeout (in seconds) for connecting to the database.  
连接数据库时的超时时间（单位为秒）。
* `spring.datasource.max-active`  
The maximum number of active connections in the connection pool.  
连接池中的最大活跃连接数。
* `spring.datasource.max-age`  
The maximum age of a connection in the connection pool.  
连接池中连接的最长寿命。
* `spring.datasource.max-idle`  
The maximum number of idle connections in the connection pool.  
连接池中的最大空闲连接数。
* `spring.datasource.max-lifetime`  
The maximum lifetime (in milliseconds) of a connection in the connection pool.  
连接池中连接的最长寿命（单位为毫秒）。
* `spring.datasource.max-open-prepared-statements`  
The maximum number of open prepared statements.  
开启状态的`PreparedStatement`的最大数量。
* `spring.datasource.max-wait`  
The maximum number of milliseconds that the pool will wait for a connection to be returned before throwing an exception.  
连接池在等待返回连接时，最多等待多少毫秒后抛出异常。
* `spring.datasource.maximum-pool-size`  
The maximum size that the pool is allowed to reach, including both idle and in-use connections.  
连接池能达到的最大连接数，包含空闲连接和使用中的连接。
* `spring.datasource.min-evictable-idle-time-millis`  
The minimum amount of time an object may sit idle in the pool before it is eligible for eviction by the idle object evictor (if any).  
一个空闲的连接在可被空闲连接释放器（如果存在的话）优雅的释放前，最少会在连接池里停留多少时间。
* `spring.datasource.min-idle`  
The minimum number of established connections that should be kept in the pool at all times. (For DBCP and Tomcat connection pools.)  
连接池里始终应该保持的最小连接数。（用于DBCP和Tomcat连接池。）
* `spring.datasource.minimum-idle`  
The minimum number of idle connections that HikariCP tries to maintain in the pool.  
HikariCP试图在连接池里维持的最小空闲连接数。
* `spring.datasource.name`  
The datasource name.  
数据源的名称。
* `spring.datasource.num-tests-per-eviction-run`  
The number of objects to examine during each run of the idle object evictor thread (if any).  
空闲对象释放器线程（如果存在的话）每次运行时要检查的对象数。
* `spring.datasource.password`  
Login password of the database.  
数据库的登录密码。
* `spring.datasource.platform`  
Platform to use in the schema resource (schema-${platform}.sql). (Default value: all)  
在Schema资源（schema-${platform}.sql）里要使用的平台。（默认值：`all`）
* `spring.datasource.pool-name`  
The connection pool name.  
连接池名称。
* `spring.datasource.pool-prepared-statements`  
Whether to pool statements or not.  
是否要将`Statement`放在池里。
* `spring.datasource.propagate-interrupt-state`  
Whether to propagate interrupt state for interrupted threads waiting for a connection.  
对于在等待连接的中断线程，是否要传播中断状态。
* `spring.datasource.read-only`  
Set a datasource as read-only when using the Hikari connection pool.  
在使用Hikari连接池时将数据源设置为只读的。
* `spring.datasource.register-mbeans`  
Whether or not the Hikari connection pool should register JMX MBeans.  
Hikari连接池是否要注册JMX MBean。
* `spring.datasource.remove-abandoned`  
Whether abandoned connections should be removed if they exceed the abandoned timeout.  
被弃用的连接在到达弃用超时后是否应该被移除。
* `spring.datasource.remove-abandoned-timeout`  
The time in seconds before a connection can be considered abandoned.  
连接在多少秒后应该考虑弃用。
* `spring.datasource.rollback-on-return`  
Whether any pending transactions should be rolled back when a connection is returned to the pool.  
在连接归还连接池时，是否要回滚挂起的事务。
* `spring.datasource.schema`  
Schema (DDL) script resource reference.  
Schema（DDL）脚本资源的引用。
* `spring.datasource.separator`  
Statement separator in SQL initialization scripts. (Default value: ;)  
SQL初始化脚本里的语句分割符。（默认值：`;`）
* `spring.datasource.sql-script-encoding`  
SQL scripts encoding.  
SQL脚本的编码。
* `spring.datasource.suspect-timeout`  
How long in seconds before logging a suspected abandoned connection.  
在记录下一个疑似弃用连接前要等待多少秒。
* `spring.datasource.test-on-borrow`  
Whether a connection should be tested upon being borrowed from the connection pool.  
在从连接池中借用连接时是否要进行测试。
* `spring.datasource.test-on-connect`  
Whether a connection should be tested upon creation.  
在建立连接时是否要进行测试。
* `spring.datasource.test-on-return`  
Whether a connection should be tested upon return to the connection pool.  
在将连接归还到连接池时是否要进行测试。
* `spring.datasource.test-while-idle`  
Whether a connection should be tested while idle.  
在连接空闲时是否要进行测试。
* `spring.datasource.time-between-eviction-runs-millis`  
The number of milliseconds to sleep between runs of the idle connection validation, abandoned cleaner, and idle pool resizing.  
在两次空闲连接验证、弃用连接清理和空闲池大小调整之间睡眠的毫秒数。
* `spring.datasource.transaction-isolation`  
Set the default transaction isolation level when using the Hikari connection pool.  
在使用Hikari连接池时设置的默认事务隔离级别。
* `spring.datasource.url`  
JDBC URL of the database.  
数据库的JDBC URL。
* `spring.datasource.use-disposable-connection-facade`  
Whether the connection will be wrapped with a facade that will disallow the connection to be used after Connection.close() is called.  
连接是否要用一个门面（facade）封装起来，在调用了`Connection.close()`后就不能再使用这个连接了。
* `spring.datasource.use-equals`  
Whether to use String.equals() instead of == when comparing method names.  
在比较方法名时是否使用`String.equals()`来代替`==`。
* `spring.datasource.use-lock`  
Whether a lock should be used when operations are performed on the connection object.  
在操作连接对象时是否要加锁。
* `spring.datasource.username`  
Login user of the database.  
数据库的登录用户名。
* `spring.datasource.validation-interval`  
How often, in milliseconds, to run connection validation.  
执行连接验证的间隔时间，单位为毫秒。
* `spring.datasource.validation-query`  
The SQL query that will be used to validate connections from this pool before returning them to the caller or pool.  
在连接池里的连接被返回给调用者或连接池时要执行的验证SQL查询。
* `spring.datasource.validation-query-timeout`  
The timeout in seconds before a connection validation query fails.  
在连接验证查询执行失败前等待的超时时间，单位为秒。
* `spring.datasource.validation-timeout`  
The timeout in seconds before a connection validation fails. (For use with the Hikari connection pool.)  
在连接验证失败前等待的超时时间，单位为秒。（用于Hikari连接池。）
* `spring.datasource.validator-class-name`  
The fully qualified class name for an optional validator class that will be used in place of test queries.  
可选的验证器类的全限定类名，它会执行测试查询。
* `spring.datasource.xa.data-source-class-name`  
XA datasource fully qualified name.  
XA数据源的全限定类名。
* `spring.datasource.xa.properties`  
Properties to pass to the XA data source.  
要传递给XA数据源的属性。
* `spring.freemarker.allow-request-override`  
Set whether HttpServletRequest attributes are allowed to override (hide) controller-generated model attributes of the same name.  
`HttpServletRequest`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.freemarker.allow-session-override`  Set whether HttpSession attributes are allowed to override (hide) controller-generated model attributes of the same name.  `HttpSession`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。* `spring.freemarker.cache`  
Enable template caching.  开启模板缓存。* `spring.freemarker.charset`  
Template encoding.  模板编码。* `spring.freemarker.check-template-location`  
Check that the templates location exists.  检查模板位置是否存在。* `spring.freemarker.content-type`  
Content-Type value.  `Content-Type`的值。* `spring.freemarker.enabled`  Enable MVC view resolution for this technology.  开启FreeMarker的MVC视图解析。* `spring.freemarker.expose-request-attributes`  Set whether all request attributes should be added to the model prior to merging with the template.  在模型合并到模板前，是否要把所有的请求属性添加到模型里。* `spring.freemarker.expose-session-attributes`  Set whether all HttpSession attributes should be added to the model prior to merging with the template.  在模型合并到模板前，是否要把所有的`HttpSession`属性添加到模型里。* `spring.freemarker.expose-spring-macro-helpers`  Set whether to expose a RequestContext for use by Spring’s macro library, under the name springMacroRequestContext.  是否要用Spring的宏发布一个`RequestContext`，名为`springMacroRequestContext`。* `spring.freemarker.prefer-file-system-access`  Prefer filesystem access for template loading. Filesystem access enables hot detection of template changes. (Default value: true)  加载模板时优先通过文件系统访问。文件系统访问能够实时检测到模板变更。（默认值：`true`）* `spring.freemarker.prefix`  Prefix that gets prepended to view names when building a URL.  在构建URL时添加到视图名称前的前缀。* `spring.freemarker.request-context-attribute`  
Name of the RequestContext attribute for all views.  所有视图里使用的`RequestContext`属性的名称。* `spring.freemarker.settings`  Well-known FreeMarker keys that will be passed to FreeMarker’s configuration.  要传递给FreeMarker配置的各种键。* `spring.freemarker.suffix`  Suffix that gets appended to view names when building a URL.  在构建URL时添加到视图名称后的后缀。* `spring.freemarker.template-loader-path`  Comma-separated list of template paths. (Default value: ["classpath:/templates/"])  
模板路径列表，用逗号分隔。（默认值：`["classpath:/templates/"]`）
* `spring.freemarker.view-names`  Whitelist of view names that can be resolved.  可解析的视图名称的白名单。* `spring.groovy.template.allow-request-override`  Set whether HttpServletRequest attributes are allowed to override (hide) controller-generated model attributes of the same name.  `HttpServletRequest`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。* `spring.groovy.template.allow-session-override`  Set whether HttpSession attributes are allowed to override (hide) controller-generated model attributes of the same name.  `HttpSession`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。* `spring.groovy.template.cache`  Enable template caching.  开启模板缓存。* `spring.groovy.template.charset`  Template encoding.  模板编码。* `spring.groovy.template.check-template-location`  Check that the templates location exists.  检查模板位置是否存在。* `spring.groovy.template.configuration.auto-escape`  Whether or not model variables are escaped when rendered in the template. (Default value: false)  模型变量在模板里呈现时是否要做转义。（默认值：`false`）* `spring.groovy.template.configuration.auto-indent`  Whether or not the template renders indentation automatically. (Default value: false)  模板是否要自动呈现缩进。（默认值：`false`）* `spring.groovy.template.configuration.auto-indent-string`  The string used for indentation when auto-indentation is enabled. Either SPACES or TAB. (Default value: SPACES)  开启自动缩进时用于缩进的字符串。可以是`SPACES`，也可以是`TAB`。（默认值：`SPACES`）* `spring.groovy.template.configuration.auto-new-line`  Whether or not new lines should be rendered by the template. (Default value: false)  模板里是否要呈现新的空行。（默认值：`false`）* `spring.groovy.template.configuration.base-template-class`  The template base class.  模板基类。* `spring.groovy.template.configuration.cache-templates`  
Whether or not templates should be cached. (Default value: true)  模板是否应该被缓存。（默认值：`true`）* `spring.groovy.template.configuration.declaration-encoding`  The encoding used to write the declaration header.  用来写声明头的编码。* `spring.groovy.template.configuration.expand-empty-elements`  Whether elements without a body should be written in the short form (e.g., <br/>) or expanded form (e.g., <br></br>). (Default value: false)  
没有正文的元素是该用短形式（例如，`<br/>`）还是扩展形式（例如，`<br></br>`）来书写。
* `spring.groovy.template.configuration.locale`  Set the template locale.  设置模板地域。* `spring.groovy.template.configuration.new-line-string`  The string to render for a new line when auto-newlines are enabled. (Default is the value of the system’s line.separator property)  
在自动空行开启后用来呈现空行的字符串。（默认是系统的`line.separator`属性值）* `spring.groovy.template.configuration.resource-loader-path`  The path to the Groovy templates. (Default value: classpath:/templates/)  
Groovy模板的路径。（默认值：`classpath:/templates/`）* `spring.groovy.template.configuration.use-double-quotes`  Whether attributes should use double quotes or single quotes. (Default value: false)  
属性是该用双引号还是单引号。（默认值：`false`）* `spring.groovy.template.content-type`  
Content-Type value.
`Content-Type`的值。* `spring.groovy.template.enabled`  Enable MVC view resolution for this technology.  
开启Groovy模板的MVC视图解析。* `spring.groovy.template.expose-request-attributes`  Set whether all request attributes should be added to the model prior to merging with the template.  
在模型合并到模板前，是否要把所有的请求属性添加到模型里。* `spring.groovy.template.expose-session-attributes`  Set whether all HttpSession attributes should be added to the model prior to merging with the template.  
在模型合并到模板前，是否要把所有的`HttpSession`属性添加到模型里。* `spring.groovy.template.expose-spring-macro-helpers`  Set whether to expose a RequestContext for use by Spring’s macro library, under the name springMacroRequestContext.  
是否要用Spring的宏发布一个`RequestContext`，名为`springMacroRequestContext`。* `spring.groovy.template.prefix`  Prefix that gets prepended to view names when building a URL.  
在构建URL时添加到视图名称前的前缀。* `spring.groovy.template.request-context-attribute`  
Name of the RequestContext attribute for all views.  
所有视图里使用的`RequestContext`属性的名称。* `spring.groovy.template.resource-loader-path`  
Template path. (Default value: classpath:/templates/)  
模板路径。（默认值：`classpath:/templates/`）* `spring.groovy.template.suffix`  Suffix that gets appended to view names when building a URL.  
在构建URL时添加到视图名称后的后缀。* `spring.groovy.template.view-names`  
Whitelist of view names that can be resolved.  
可解析的视图名称白名单。* `spring.h2.console.enabled`  Enable the console. (Default value: false)  
开启控制台。（默认值：`false`）
* `spring.h2.console.path`  Path at which the console will be available. (Default value: /h2-console)  
可以找到控制台的路径。（默认值：`/h2-console`）* `spring.hateoas.apply-to-primary-object-mapper`  Specify if HATEOAS support should be applied to the primary ObjectMapper. (Default value: true)  
指定主`ObjectMapper`是否要应用HATEOAS支持。（默认值：`true`）* `spring.hornetq.embedded.cluster-password`  Cluster password. Randomly generated on startup by default.  
集群密码。默认在启动时随机生成。* `spring.hornetq.embedded.data-directory`  Journal file directory. Not necessary if persistence is turned off.  
日志文件目录。如果关闭了持久化功能则不需要该属性。* `spring.hornetq.embedded.enabled`  Enable embedded mode if the HornetQ server APIs are available. (Default value: true)  
如果有HornetQ服务器API的话，开启嵌入模式。（默认值：`true`）* `spring.hornetq.embedded.persistent`  
Enable persistent store. (Default value: false)  
开启持久化存储。（默认值：`false`）* `spring.hornetq.embedded.queues`  Comma-separated list of queues to create on startup. (Default value: [])  
启动时要创建的队列列表，用逗号分隔。（默认值：`[]`）* `spring.hornetq.embedded.server-id`  Server ID. By default, an auto-incremented counter is used. (Default value: 0)  
服务器ID。默认会使用自增长计数器。（默认值：`0`）* `spring.hornetq.embedded.topics`  Comma-separated list of topics to create on startup. (Default value: [])  
启动时要创建的主题列表，用逗号分隔。（默认值：`[]`）* `spring.hornetq.host`  HornetQ broker host. (Default value: localhost)  
HornetQ的主机。（默认值：`localhost`）* `spring.hornetq.mode`  HornetQ deployment mode, auto-detected by default. Can be explicitly set to native or embedded.  
HornetQ的部署模式，默认为自动检测。可以显式地设置为`native`或`embedded`。* `spring.hornetq.port`  HornetQ broker port. (Default value: 5445)  
HornetQ的端口。（默认值：`5445`）* `spring.http.converters.preferred-json-mapper`  Preferred JSON mapper to use for HTTP message conversion.  
HTTP消息转换时优先使用JSON映射器。* `spring.http.encoding.charset`  Charset of HTTP requests and responses. Added to the Content-Type header if not set explicitly. (Default value: UTF-8)  
HTTP请求和响应的字符集。如果没有显式地指定`Content-Type`头，则会将该属性值作为这个头的值。（默认值：`UTF-8`）* `spring.http.encoding.enabled`  Enable HTTP encoding support. (Default value: true)  
开启HTTP编码支持。（默认值：`true`）
* `spring.http.encoding.force`  Force the encoding to the configured charset on HTTP requests and responses. (Default value: true)  
强制将HTTP请求和响应编码为所配置的字符集。（默认值：`true`）* `spring.jackson.date-format`  Date format string (yyyy-MM-dd HH:mm:ss) or a fully qualified date format class name.  
日期格式字符串（yyyy-MM-dd HH:mm:ss）或日期格式类的全限定类名。* `spring.jackson.deserialization`  Jackson on/off features that affect the way Java objects are deserialized.  
影响Java对象反序列化的Jackson on/off特性。* `spring.jackson.generator`  Jackson on/off features for generators.  
用于生成器的Jackson on/off特性。* `spring.jackson.joda-date-time-format`  Joda date/time format string (yyyy-MM-dd HH:mm:ss). If not configured, date-format will be used as a fallback if it’s configured with a format string.  
Joda日期时间格式字符串（yyyy-MM-dd HH:mm:ss）。如果没有配置，而`date-format`又配置了一个格式字符串的话，会将它作为降级配置。* `spring.jackson.locale`  
Locale used for formatting.  
用于格式化的地域值。* `spring.jackson.mapper`  Jackson general purpose on/off features.  
Jackson的通用目的on/off特性。* `spring.jackson.parser`  Jackson on/off features for parsers.  
用于解析器的Jackson on/off特性。* `spring.jackson.property-naming-strategy`  One of the constants on Jackson’s PropertyNamingStrategy (CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES). Can also be a fully qualified class name of a `PropertyNamingStrategy` subclass.  
Jackson的`PropertyNamingStrategy`中的一个常量（`CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES`）。也可以设置`PropertyNamingStrategy`的子类的全限定类名。* `spring.jackson.serialization`  Jackson on/off features that affect the way Java objects are serialized.  影响Java对象序列化的Jackson on/off特性。* `spring.jackson.serialization-inclusion`  Controls the inclusion of properties during serialization. Configured with one of the values in Jackson’s JsonInclude.Include enumeration.  
控制序列化时要包含哪些属性。可选值是Jackson的`JsonInclude.Include`枚举里的某个值。* `spring.jackson.time-zone`  Time zone used when formatting dates. Configured using any recognized time zone identifier, such as America/Los_Angeles or GMT+10.  
格式化日期时使用的时区。可以配置各种可识别的时区标识符，比如`America/Los_Angeles`或者`GMT+10`。* `spring.jersey.filter.order`  Jersey filter chain order. (Default value: 0)  
Jersey过滤器链的顺序。（默认值：`0`）* `spring.jersey.init`  Init parameters to pass to Jersey via the servlet or filter.  
通过Servlet或过滤器传递给Jersey的初始化参数。* `spring.jersey.type`  Jersey integration type. Can be either servlet or filter.  
Jersey集成类型。可以是`servlet`或者`filter`。* `spring.jms.jndi-name`  Connection factory JNDI name. When set, takes precedence to others’ connection factory auto-configurations.  
连接工厂的JNDI名字。设置了该属性后，它会优先于其他自动配置的连接工厂。* `spring.jms.listener.acknowledge-mode`  Acknowledge mode of the container. By default, the listener is transacted with automatic acknowledgment.  
容器的应答模式（acknowledgment mode）。默认情况下，监听器使用自动应答。* `spring.jms.listener.auto-startup`  Start the container automatically on startup. (Default value: true)  
启动时自动启动容器。* `spring.jms.listener.concurrency`  Minimum number of concurrent consumers.  
并发消费者的最小数量。* `spring.jms.listener.max-concurrency`  Maximum number of concurrent consumers.  
并发消费者的最大数量。* `spring.jms.pub-sub-domain`  Specify if the default destination type supports publish/subscribe (if it is a topic as opposed to a queue). (Default value: false)  
指明默认的目的地类型是否支持public/subscribe（如果它是主题而非队列的话）。（默认值：`false`）* `spring.jmx.default-domain`  JMX domain name.  
JMX域名。* `spring.jmx.enabled`  Expose management beans to the JMX domain. (Default value: true)  
将管理Bean发布到JMX域里。（默认值：`true`）* `spring.jmx.server`  MBeanServer bean name. (Default value: mbeanServer)  
`MBeanServer`的Bean名称。（默认值：`mbeanServer`）* `spring.jooq.sql-dialect`  SQLDialect JOOQ used when communicating with the configured datasource, such as POSTGRES.  
在与配置的数据源通信时，JOOQ使用的`SQLDialect`，比如`POSTGRES`。* `spring.jpa.database`  Target database to operate on, auto-detected by default. Can be alternatively set using the databasePlatform property.  
要操作的目标数据库，默认会自动检测。另外，也可以通过`databasePlatform`属性进行设置。* `spring.jpa.database-platform`  Name of the target database to operate on, auto-detected by default. Can be alternatively set using the Database enum.  
要操作的目标数据库，默认会自动检测。也可以通过`Database`枚举来设置。* `spring.jpa.generate-ddl`  Initialize the schema on startup. (Default value: false)  
启动时要初始化Schema。（默认值：`false`）* `spring.jpa.hibernate.ddl-auto`  DDL mode (none, validate, update, create, create-drop). This is actually a shortcut for the hibernate.hbm2ddl.auto property. Default to create-drop when using an embedded database; none otherwise.  
DDL模式（`none`、`validate`、`update`、`create`和`create-drop`）。这是`hibernate.hbm2ddl.auto`属性的一个快捷方式。在使用嵌入式数据库时，默认为`create-drop`；其他情况下默认为`none`。
* `spring.jpa.hibernate.naming-strategy`  The fully qualified class name of a Hibernate naming strategy.  
Hibernate命名策略的全限定类名。* `spring.jpa.open-in-view`  Register OpenEntityManagerInViewInterceptor. Binds a JPA EntityManager to the thread for the entire processing of the request. (Default value: true)  
注册`OpenEntityManagerInViewInterceptor`，在请求的整个处理过程中，将一个JPA `EntityManager`绑定到线程上。（默认值：`true`）* `spring.jpa.properties`  Additional native properties to set on the JPA provider.  
JPA提供方上要设置的额外原生属性。* `spring.jpa.show-sql`  Enable logging of SQL statements when using the Bitronix Transaction Manager. (Default value: false)  
在使用Bitronix Transaction Manager时打开SQL语句日志。（默认值：`false`）* `spring.jta.allow-multiple-lrc`  Whether the transaction manager should allow enlistment of multiple LRC resources in a single transaction when using the Bitronix Transaction Manager. (Default value: false)  
在使用Bitronix Transaction Manager时，事务管理器是否应该允许一个事务里涉及多个LRC资源。（默认值：`false`）* `spring.jta.asynchronous2-pc`  Whether two-phase commit should be executed asynchronously when using the Bitronix Transaction Manager. (Default value: false)  
在使用Bitronix Transaction Manager时，是否异步执行两阶段提交。（默认值：`false`）* `spring.jta.background-recovery-interval`  How often, in minutes, to run the recovery process when using the Bitronix Transaction Manager. (Default value: 1)  
在使用Bitronix Transaction Manager时，多久运行一次恢复过程，单位为分钟。（默认值：`1`）* `spring.jta.background-recovery-interval-seconds`  How often, in seconds, to run the recovery process when using the Bitronix Transaction Manager. (Default value: 60)  
在使用Bitronix Transaction Manager时，多久运行一次恢复过程，单位为秒。（默认值：`60`）* `spring.jta.current-node-only-recovery`  Whether recovery should filter out recovered XIDs that don’t contain this JVM’s unique ID when using the Bitronix Transaction Manager. (Default value: true)  
在使用Bitronix Transaction Manager时，恢复是否要过滤掉不包含本JVM唯一ID的XID。（默认值：`true`）* `spring.jta.debug-zero-resource-transaction`  Whether creation and commit call stacks of transactions executed without a single enlisted resource should be tracked and logged when using the Bitronix Transaction Manager. (Default value: false)  
在使用Bitronix Transaction Manager时，对于没有涉及任何资源的事务，是否要跟踪并记录它们的创建和提交调用栈。（默认值：`false`）* `spring.jta.default-transaction-timeout`  The default transaction timeout, in seconds, when using the Bitronix Transaction Manager. (Default value: 60)  
在使用Bitronix Transaction Manager时，默认的事务超时时间，单位为秒。（默认值：`60`）* `spring.jta.disable-jmx`  Whether the registration of JMX MBeans should be disabled when using the Bitronix Transaction Manager. (Default value: false)  
在使用Bitronix Transaction Manager时，是否要禁止注册JMX MBean。（默认值：`false`）
* `spring.jta.enabled`  Enable JTA support. (Default value: true)  
开启JTA支持。（默认值：`true`）* `spring.jta.exception-analyzer`  The exception analyzer to use when using the Bitronix Transaction Manager. Can be null for the default exception analyzer or the fully qualified class name of a custom exception analyzer.  在使用Bitronix Transaction Manager时用到的异常分析器。设置为`null`时使用默认异常分析器，也可以设置自定义异常分析器的全限定类名。* `spring.jta.filter-log-status`  Whether mandatory logs should be written when using the Bitronix Transaction Manager. Enabling this parameter lowers space usage of the fragments but makes debugging more complex. (Default value: false)  在使用Bitronix Transaction Manager时，是否只打必要的日志。开启该参数时能减少分段（fragment）空间用量，但让调试更复杂了。（默认值：`false`）* `spring.jta.force-batching-enabled`  Whether disk forces are batched when using the Bitronix Transaction Manager. Disabling batching can seriously lower the transaction manager’s throughput. (Default value: true)  在使用Bitronix Transaction Manager时，是否批量输出到磁盘上。禁用批处理会严重降低事务管理器的吞吐量。（默认值：`true`）* `spring.jta.forced-write-enabled`  Whether logs are forced to disk when using the Bitronix Transaction Manager. Do not set to false in production because without disk force, integrity is not guaranteed. (Default value: true)  在使用Bitronix Transaction Manager时，日志是否强制写到磁盘上。在生产环境里不要设置为`false`，因为不强制写到磁盘上无法保证完整性。（默认值：`true`）* `spring.jta.graceful-shutdown-interval`  Maximum number of seconds the transaction manager will wait for transactions to be done before aborting them at shutdown time when using the Bitronix Transaction Manager. (Default value: 60)  在使用Bitronix Transaction Manager时，要关闭的话，事务管理器在放弃事务前最多等它多少秒。（默认值：`60`）* `spring.jta.jndi-transaction-synchronization-registry-name`  The name that the transaction synchronization registry should be bound under in JNDI when using the Bitronix Transaction Manager. (Default value: java:comp/TransactionSynchronizationRegistry)  在使用Bitronix Transaction Manager时，事务同步注册表应该绑定到哪个JNDI下。（默认值：`java:comp/TransactionSynchronizationRegistry`）* `spring.jta.jndi-user-transaction-name`  The name the user transaction should be bound under in JNDI when using the Bitronix Transaction Manager. (Default value: java:comp/UserTransaction)  在使用Bitronix Transaction Manager时，用户事务应该绑定到哪个JNDI下。（默认值：`java:comp/UserTransaction`）* `spring.jta.journal`  The journal name, when using the Bitronix Transaction Manager. Can be disk, null, or a fully qualified class name. (Default value: disk)  在使用Bitronix Transaction Manager时，要用的日志名。可以是`disk`、`null`或者全限定类名。（默认值：`disk`）* `spring.jta.log-dir`  Transaction logs directory.  事务日志目录。* `spring.jta.log-part1-filename`  The journal fragment file 1 name. (Default value: btm1.tlog)  
日志分段文件1的名称。（默认值：`btm1.tlog`）
* `spring.jta.log-part2-filename`  The journal fragment file 2 name. (Default value: btm2.tlog)  日志分段文件2的名称。（默认值：`btm2.tlog`）* `spring.jta.max-log-size-in-mb`  The maximum size in megabytes of the journal fragments. Larger logs allow transactions to stay longer in-doubt. If, however, the size is too small, the transaction manager will pause longer when a fragment is full. For use with the Bitronix Trans- action Manager. (Default value: 2)  在使用Bitronix Transaction Manager时，日志分段文件的最大兆数。日志越大，事务在未终结状态的时间就能越长。但是，如果文件大小限制的太小，事务管理器在分段满了的时候就会暂停更长时间。（默认值：`2`）* `spring.jta.resource-configuration-filename`  The Bitronix Transaction Manager configuration filename.  Bitronix Transaction Manager的配置文件名。* `spring.jta.server-id`  The ID that uniquely identifies the Bitronix Transaction Manager instance.  唯一标识Bitronix Transaction Manager实例的ID。* `spring.jta.skip-corrupted-logs`  Whether corrupted log files should be skipped. (Default value: false)  是否要跳过损坏的日志文件。（默认值：`false`）* `spring.jta.transaction-manager-id`  
Transaction manager unique identifier.  事务管理器的唯一标识符。* `spring.jta.warn-about-zero-resource-transaction`  Whether to warn about transactions executed without a single enlisted resource when using the Bitronix Transaction Manager. (Default value: true)  在使用Bitronix Transaction Manager时，是否要对执行时没有涉及任何资源的事务作出告警。（默认值：`true`）* `spring.mail.default-encoding`  Default MimeMessage encoding. (Default value: UTF-8)  默认的`MimeMessage`编码。（默认值：`UTF-8`）* `spring.mail.host`  
SMTP server host.  SMTP服务器主机地址。* `spring.mail.jndi-name`  Session JNDI name. When set, takes precedence over any other mail settings.  会话的JNDI名称。该属性的优先级要高于其他邮件设置。* `spring.mail.password`  Login password of the SMTP server.  SMTP服务器的登录密码。* `spring.mail.port`  
SMTP server port.  SMTP服务器的端口号。* `spring.mail.properties`  Additional JavaMail session properties.  附加的JavaMail会话属性。* `spring.mail.protocol`  Protocol used by the SMTP server. (Default value: smtp)  SMTP服务器用到的协议。（默认值：`smtp`）* `spring.mail.test-connection`  Test that the mail server is available on startup. (Default value: false)  在启动时测试邮件服务器是否可用。（默认值：`false`）* `spring.mail.username`  Login user of the SMTP server.  SMTP服务器的登录用户名。* `spring.messages.basename`  Comma-separated list of basenames, each following the ResourceBundle convention. Essentially a fully qualified classpath location. If it doesn’t contain a package qualifier (such as org.mypackage), it will be resolved from the classpath root. (Default value: messages)  逗号分隔的基本名称列表，都遵循`ResourceBundle`的惯例。本质上这就是一个全限定的Classpath位置，如果不包含包限定符（比如`org.mypackage`），就会从Classpath的根部开始解析。（默认值：`messages`）* `spring.messages.cache-seconds`  Loaded resource bundle files cache expiration, in seconds. When set to -1, bun- dles are cached forever. (Default value: -1)  加载的资源包文件的缓存失效时间，单位为秒。在设置为`-1`时，包会永远缓存。（默认值：`-1`）* `spring.messages.encoding`  Message bundles encoding. (Default value: UTF-8)  消息包的编码。（默认值：`UTF-8`）* `spring.mobile.devicedelegatingviewresolver.enable-fallback`  
Enable support for fallback resolution. (Default value: false)  开启降级解析支持。（默认值：`false`）* `spring.mobile.devicedelegatingviewresolver.enabled`  
Enable device view resolver. (Default value: false)  开启设备视图解析器。（默认值：`false`）* `spring.mobile.devicedelegatingviewresolver.mobile-prefix`  Prefix that gets prepended to view names for mobile devices. (Default value: mobile/)  添加到移动设备视图名前的前缀。（默认值：`mobile/`）* `spring.mobile.devicedelegatingviewresolver.mobile-suffix`  Suffix that gets appended to view names for mobile devices.  添加到移动设备视图名后的后缀。* `spring.mobile.devicedelegatingviewresolver.normal-prefix`  Prefix that gets prepended to view names for normal devices.  添加到普通设备视图名前的前缀。* `spring.mobile.devicedelegatingviewresolver.normal-suffix`  Suffix that gets appended to view names for normal devices.  添加到普通设备视图名后的后缀。* `spring.mobile.devicedelegatingviewresolver.tablet-prefix`  Prefix that gets prepended to view names for tablet devices. (Default value: tablet/)  添加到平板设备视图名前的前缀。（默认值：`tablet/`）* `spring.mobile.devicedelegatingviewresolver.tablet-suffix`  Suffix that gets appended to view names for tablet devices.  添加到平板设备视图名后的后缀。* `spring.mobile.sitepreference.enabled`  Enable SitePreferenceHandler. (Default value: true)  开启`SitePreferenceHandler`。（默认值：`true`）* `spring.mongodb.embedded.features`  Comma-separated list of features to enable.  要开启的特性列表，用逗号分隔。* `spring.mongodb.embedded.version`  Version of Mongo to use. (Default value: 2.6.10)  
要使用的Mongo版本。（默认值：`2.6.10`）
