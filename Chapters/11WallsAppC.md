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
* `server.tomcat.protocol-header`  
Header that holds the incoming protocol, usually named X-Forwarded-Proto. Configured as a RemoteIpValve only if remoteIpHeader is also set.
* `server.tomcat.protocol-header-https-value`  
Value of the protocol header that indicates that the incoming request uses SSL. (Default value: https)
* `server.tomcat.remote-ip-header`  
Name of the HTTP header from which the remote IP is extracted. Configured as a RemoteIpValve only if remoteIpHeader is also set.
* `server.tomcat.uri-encoding`  
Character encoding to use to decode the URI.
* `server.undertow.access-log-dir`  
Undertow access log directory. (Default value: logs)
* `server.undertow.access-log-enabled`  
Whether or not the access log is enabled. (Default value: false)
* `server.undertow.access-log-pattern`  
Format pattern for access logs. (Default value: common)
* `server.undertow.accesslog.dir`  
Undertow access log directory.
* `server.undertow.accesslog.enabled`  
Enable access log. (Default value: false)
* `server.undertow.accesslog.pattern`  
Format pattern for access logs. (Default value: common)
