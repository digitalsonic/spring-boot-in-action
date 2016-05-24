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
* `flyway.clean-on-validation-error`  
Whether to automatically call clean or not when a validation error occurs. (Default value: false)
* `flyway.enabled`  
Enable flyway. (Default value: true)
* `flyway.encoding`  
Sets the SQL migration encoding. (Default value: UTF-8)
* `flyway.ignore-failed-future-migration`  
Whether to ignore failed future migrations when reading the metadata table. (Default value: false)
* `flyway.init-sqls`  
SQL statements to execute to initialize a connection immediately after obtaining it.
* `flyway.locations`  
Locations of migrations scripts. (Default value: db/migration)
* `flyway.out-of-order`  
Whether or not “out of order” migrations are allowed. (Default value: false)
* `flyway.password`  
Login password of the database to migrate.
* `flyway.placeholder-prefix`  
Sets the prefix of every placeholder. (Default value: ${)
* `flyway.placeholder-replacement`  
Whether placeholders should be replaced. (Default value: true)
* `flyway.placeholder-suffix`  
Sets the prefix of every placeholder. (Default value: ${)
* `flyway.placeholders.[placeholder name]`  
Sets a placeholder value.
* `flyway.schemas`  
A case-sensitive list of schemes managed by Flyway. Defaults to the default schema of the connection.
* `flyway.sql-migration-prefix`  
The filename prefix for SQL migrations. (Default value: V)
* `flyway.sql-migration-separator`  
The filename separator for SQL migrations. (Default value: \__)
* `flyway.sql-migration-suffix`  
The filename suffix for SQL migrations. (Default value: .sql)
* `flyway.table`  
The name of the schema metadata table to be used by Flyway. (Default value: schema_version)
* `flyway.target`  
The target version up to which Flyway should consider migrations. (Defaults to the latest version)
* `flyway.url`  
JDBC URL of the database to migrate. If not set, the primary configured data source is used.
* `flyway.user`  
Login user of the database to migrate.
* `flyway.validate-on-migrate`  
Whether to automatically validate when running migrate. (Default value: true)
* `liquibase.change-log`  
Change log configuration path. (Default value: classpath:/db/changelog/db.changelog-master.yaml)
* `liquibase.check-change-log-location`  
Check that the change log location exists. (Default value: true)
* `liquibase.contexts`  
Comma-separated list of runtime contexts to use.
* `liquibase.default-schema`  
Default database schema.
* `liquibase.drop-first`  
Drop the database schema first. (Default value: false)
* `liquibase.enabled`  
Enable Liquibase support. (Default value: true)
* `liquibase.password`  
Login password of the database to migrate.
* `liquibase.url`  
JDBC URL of the database to migrate. If not set, the primary configured data source is used.
* `liquibase.user`  
Login user of the database to migrate.
* `multipart.enabled`  
Enable support of multi-part uploads. (Default value: true)
* `multipart.file-size-threshold`  
Threshold after which files will be written to disk. Values can use the suffixes “MB” or “KB” to indicate a megabyte or kilobyte size. (Default value: 0)
* `multipart.location`  
Intermediate location of uploaded files.
* `multipart.max-file-size`  
Max file size. Values can use the suffixes “MB” or “KB” to indicate a megabyte or kilobyte size. (Default value: 1MB)
* `multipart.max-request-size`  
Max request size. Values can use the suffixes “MB” or “KB” to indicate a megabyte or kilobyte size. (Default value: 10MB)
* `security.basic.authorize-mode`  
Security authorize mode to apply.
* `security.basic.enabled`  
Enable basic authentication. (Default value: true)
* `security.basic.path`  
Comma-separated list of paths to secure. (Default value: [/\**])
* `security.basic.realm`  
HTTP basic realm name. (Default value: Spring)
* `security.enable-csrf`  
Enable cross-site request forgery support. (Default value: false)
* `security.filter-order`  
Security filter chain order. (Default value: 0)
* `security.headers.cache`  
Enable cache control HTTP headers. (Default value: false)
* `security.headers.content-type`  
Enable X-Content-Type-Options header. (Default value: false)
* `security.headers.frame`  
Enable X-Frame-Options header. (Default value: false)
* `security.headers.hsts`  
HTTP Strict Transport Security (HSTS) mode (none, domain, all).
* `security.headers.xss`  
Enable cross-site scripting (XSS) protection. (Default value: false)
* `security.ignored`  
Comma-separated list of paths to exclude from the default secured paths.
* `security.oauth2.client.access-token-uri`  
The URI used to fetch an access token.
* `security.oauth2.client.access-token-validity-seconds`  
How long an access token is to be valid before expiring.
* `security.oauth2.client.additional-information.[key]`  
Set additional information that token granters would like to add to the token.
* `security.oauth2.client.authentication-scheme`  
The method for transmitting the bearer token. One of form, header, none, or query. (Default value: header)
* `security.oauth2.client.authorities`  
The authorities to be granted to an authenticated client.
* `security.oauth2.client.authorized-grant-types`  
The grant types allowed to the client.
* `security.oauth2.client.auto-approve-scopes`  
The scope to automatically approve for a client.
* `security.oauth2.client.client-authentication-scheme`  
The method for transmitting authentication credentials when authenticating the client. One of form, header, none, or query. (Default value: header)
* `security.oauth2.client.client-id`  
OAuth2 client ID.
* `security.oauth2.client.client-secret`  
OAuth2 client secret. A random secret is generated by default.
* `security.oauth2.client.grant-type`  
The grant type for obtaining an access token for this resource.
* `security.oauth2.client.id`  
The application’s client ID.
* `security.oauth2.client.pre-established-redirect-uri`  
The redirect URI that has been pre-established with the server. If present, the redirect URI will be omitted from the user authorization request because the server
doesn’t need to know it.
* `security.oauth2.client.refresh-token-validity-seconds`  
How long a refresh token will be valid before expiring.
* `security.oauth2.client.registered-redirect-uri`  
Comma-separated list of redirect URIs registered for the client.
* `security.oauth2.client.resource-ids`  
Comma-separated list of resource IDs associated with the client.
* `security.oauth2.client.scope`  
Scope assigned to the client.
* `security.oauth2.client.token-name`  
The token name.
* `security.oauth2.client.use-current-uri`  
Whether the current URI (if set) in the request should be used in preference to the pre-established redirect URI. (Default value: true)
* `security.oauth2.client.user-authorization-uri`  
The URI to which the user is to be redirected to authorize an access token.
* `security.oauth2.resource.id`  
Identifier of the resource.
* `security.oauth2.resource.jwt.key-uri`  
The URI of the JWT token. Can be set if the value is not available and the key is public.
* `security.oauth2.resource.jwt.key-value`  
The verification key of the JWT token. Can either be a symmetric secret or PEMencoded RSA public key. If the value is not available, you can set the URI instead.
* `security.oauth2.resource.prefer-token-info`  
Use the token info; can be set to false to use the user info. (Default value: true)
* `security.oauth2.resource.service-id`  
The service ID. (Default value: resource)
* `security.oauth2.resource.token-info-uri`  
URI of the token decoding endpoint.
* `security.oauth2.resource.token-type`  
The token type to send when using the userInfoUri.
* `security.oauth2.resource.user-info-uri`  
URI of the user endpoint.
* `security.oauth2.sso.filter-order`  
Filter order to apply if not providing an explicit WebSecurityConfigurerAdapter (otherwise the order can be provided there instead).
* `security.oauth2.sso.login-path`  
Path to the login page—the page that triggers the redirect to the OAuth2 Authorization Server. (Default value: /login)
* `security.require-ssl`  
Enable secure channel for all requests. (Default value: false)
* `security.sessions`  
Session creation policy. (Default values: always, never, if_required, stateless).
* `security.user.name`  
Default user name. (Default value: user)
* `security.user.password`  
Password for the default user name.
* `security.user.role`  
Granted roles for the default user name.
* `server.address`  
Network address to which the server should bind.
* `server.compression.enabled`  
Whether or not compression should be enabled. (Default value: false)
* `server.compression.excluded-user-agents`  
Comma-separated list of user agents for which responses should not be compressed. (Default values: text/html,text/xml,text/plain,text/css)
* `server.compression.mime-types`  
Comma-separated list of MIME types that should be compressed.
* `server.compression.min-response-size`  
Minimum response size (in bytes) that is required for compression to be performed. (Default value: 2048)
* `server.context-parameters.[param name]`  
Sets a servlet context parameter.
* `server.context-path`  
Context path of the application.
* `server.display-name`  
Display name of the application. (Default value: application)
* `server.jsp-servlet.class-name`  
The class name of the servlet to use for JSPs. (Default value: org.apache.jasper.servlet.JspServlet)
* `server.jsp-servlet.init-parameters.[param name]`  
Sets a JSP servlet initialization parameter.
* `server.jsp-servlet.registered`  
Whether or not the JSP servlet should be registered with the embedded servlet container. (Default value: true)
* `server.port`  
Server HTTP port.
* `server.servlet-path`  
Path of the main dispatcher servlet. (Default value: /)
* `server.session.cookie.comment`  
Comment for the session cookie.
* `server.session.cookie.domain`  
Domain for the session cookie.
* `server.session.cookie.http-only`  
HttpOnly flag for the session cookie.
* `server.session.cookie.max-age`  
Maximum age of the session cookie in seconds.
* `server.session.cookie.name`  
Session cookie name.
* `server.session.cookie.path`  
Path of the session cookie.
* `server.session.cookie.secure`  
“Secure” flag for the session cookie.
* `server.session.persistent`  
Persist session data between restarts. (Default value: false)
* `server.session.timeout`  
Session timeout in seconds.
* `server.session.tracking-modes`  
Session tracking modes (one or more of the following: cookie, url, ssl).
* `server.ssl.ciphers`  
Supported SSL ciphers.
* `server.ssl.client-auth`  
Whether client authentication is wanted (want) or needed (need). Requires a trust store.
* `server.ssl.enabled`  
Whether SSL is enabled or not. (Default value: true)
* `server.ssl.key-alias`  
Alias that identifies the key in the key store.
* `server.ssl.key-password`  
Password used to access the key in the key store.
* `server.ssl.key-store`  
Path to the key store that holds the SSL certificate (typically a .jks file).
* `server.ssl.key-store-password`  
Password used to access the key store.
* `server.ssl.key-store-provider`  
Provider for the key store.
* `server.ssl.key-store-type`  
Type of the key store.
* `server.ssl.protocol`  
SSL protocol to use. (Default value: TLS)
* `server.ssl.trust-store`  
Trust store that holds SSL certificates.
* `server.ssl.trust-store-password`  
Password used to access the trust store.
* `server.ssl.trust-store-provider`  
Provider for the trust store.
* `server.ssl.trust-store-type`  
Type of the trust store.
* `server.tomcat.access-log-enabled`  
Whether or not the access log is enabled. (Default value: false)
* `server.tomcat.access-log-pattern`  
Format pattern for access logs. (Default value: common)
* `server.tomcat.accesslog.directory`  
Directory in which log files are created. Can be relative to the tomcat base dir or absolute. (Default value: logs)
* `server.tomcat.accesslog.enabled`  
Enable access log. (Default value: false)
* `server.tomcat.accesslog.pattern`  
Format pattern for access logs. (Default value: common)
* `server.tomcat.accesslog.prefix`  
Log filename prefix. (Default value: access_log)
* `server.tomcat.accesslog.suffix`  
Log filename suffix. (Default value: .log)
* `server.tomcat.background-processor-delay`  
Delay in seconds between the invocation of backgroundProcess methods. (Default value: 30)
* `server.tomcat.basedir`  
Tomcat base directory. If not specified, a temporary directory will be used.
* `server.tomcat.internal-proxies`  
Regular expression that matches proxies that are to be trusted. Default:“10\.\d{1,3}\.\d{1,3}\.\d{1,3}| 192\.168\.\d{1,3}\.\d{1,3}| 169\.254\.\d{1,3}\.\d{1,3}| 127\.\d{1,3}\.\d{1,3}\.\d{1,3}| 172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}| 172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}”
* `server.tomcat.max-http-header-size`  
Maximum size in bytes of the HTTP message header. (Default value: 0)
* `server.tomcat.max-threads`  
Maximum number of worker threads. (Default value: 0)
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
