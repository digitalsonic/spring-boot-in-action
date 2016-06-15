# 附录C 配置属性

虽然Spring Boot在应用程序配置组件时处理了很多“粗活”，但你可能还是想对其中某些组件进行微调。这时就该配置属性登场了。

第3章介绍了`@ConfigurationProperties`注解，以及它如何暴露配置在代码外部的属性。正如你可以在自己创建的组件上使用`@ConfigurationProperties`注解，很多Spring Boot自动配置的组件也添加了`@ConfigurationProperties`注解，可以通过Spring Boot支持的各种属性源对其进行配置。

例如，要指定内嵌的Tomcat或Jetty服务器要监听的端口，可以设置`server.port`属性。这个属性可以设置于application.properties、application.yml、操作系统环境变量，或者是3.2节列出的其他地方。

本附录列出了Spring Boot组件提供的全部配置属性。请注意，这些属性是否生效取决于对应的组件是否声明为Spring应用程序上下文里的Bean（基本是自动配置的），设置为一个不生效的组件设置属性是没用的。

* `flyway.baseline-description`  ：执行基线时标记已有Schema的描述。
* `flyway.baseline-on-migrate`  ：在没有元数据表的情况下，针对非空Schema执行迁移时是否自动调用基线。（默认值：`false`。）
* `flyway.baseline-version` ：执行基线时用来标记已有Schema的版本。（默认值：`1`。）
* `flyway.check-location` ：检查迁移脚本所在的位置是否存在。（默认值：`false`。）
* `flyway.clean-on-validation-error`：在验证错误时，是否自动执行清理。（默认值：`false`。）
* `flyway.enabled`：开启Flyway。（默认值：`true`。）
* `flyway.encoding` ：设置SQL迁移文件的编码。（默认值：`UTF-8`。）
* `flyway.ignore-failed-future-migration`：
在读元数据表时，是否忽略失败的后续迁移。（默认值：`false`。）
* `flyway.init-sqls`：获取连接后立即执行初始化的SQL语句。
* `flyway.locations` ：迁移脚本的位置。（默认值：`db/migration`。）
* `flyway.out-of-order` ：是否允许乱序（out of order）迁移。（默认值：`false`。）
* `flyway.password`：待迁移数据库的登录密码。
* `flyway.placeholder-prefix`：设置每个占位符的前缀。（默认值：`${`。）
* `flyway.placeholder-replacement`：是否要替换占位符。（默认值：`true`。）
* `flyway.placeholder-suffix` ：设置占位符的后缀。（默认值：`}`。）
* `flyway.placeholders.[placeholder name]`：设置占位符的值。
* `flyway.schemas`：Flyway管理的Schema列表，大小写敏感。默认连接对应的默认Schema。
* `flyway.sql-migration-prefix`： SQL迁移的文件名前缀。（默认值：`V`。）
* `flyway.sql-migration-separator`：SQL迁移的文件名分隔符。（默认值：`__`）
* `flyway.sql-migration-suffix` ：SQL迁移的文件名后缀。（默认值：`.sql`）
* `flyway.table`：Flyway使用的Schema元数据表名称。（默认值：`schema_version`）
* `flyway.target`：Flyway要迁移到的目标版本号。（默认最新版本。）
* `flyway.url`：待迁移的数据库的JDBC URL。如果没有设置，就使用配置的主数据源。
* `flyway.user`：待迁移数据库的登录用户。
* `flyway.validate-on-migrate`：在运行迁移时是否要自动验证。（默认值：`true`。）
* `liquibase.change-log`：变更日志配置路径。（默认值：`classpath:/db/changelog/db.changelog-master.yaml`。）
* `liquibase.check-change-log-location`：检查变更日志位置是否存在。（默认值：`true`。）
* `liquibase.contexts`：要使用的运行时上下文列表，用逗号分隔。
* `liquibase.default-schema`：默认的数据库Schema。
* `liquibase.drop-first`：先删除数据库Schema。（默认值：`false`。）
* `liquibase.enabled`：开启Liquibase支持。（默认值：`true`。）
* `liquibase.password`：待迁移数据库的登录密码。
* `liquibase.url`：待迁移数据库的JDBC URL。如果没有设置，就使用配置的主数据源。
* `liquibase.user`：待迁移数据库的登录用户。
* `multipart.enabled`：开启分段（multi-part）上传支持。（默认值：`true`。）
* `multipart.file-size-threshold`：大于该阈值的文件会写到磁盘上。这里的值可以使用“MB”或“KB”后缀来表明是兆字节还是千字节。（默认值：`0`。）
* `multipart.location`：上传文件的中间存放位置。
* `multipart.max-file-size`：最大文件大小。这里的值可以使用“MB”或“KB”的后缀来表明是兆字节还是千字节。（默认值：`1MB`。）
* `multipart.max-request-size`：最大请求大小。这里的值可以使用“MB”或“KB”的后缀来表明是兆字节还是千字节。（默认值：`10MB`。）
* `security.basic.authorize-mode`：要运用的安全授权模式。
* `security.basic.enabled`：开启基本身份验证。（默认值：`true`。）
* `security.basic.path`：要保护的路径，用逗号分隔。（默认值：`[/**]`。）
* `security.basic.realm`：HTTP基本领域（realm）用户名。（默认值：`Spring`。）
* `security.enable-csrf`：开启跨站请求伪造（cross-site request forgery）支持。（默认值：`false`。）
* `security.filter-order`：安全过滤器链顺序。（默认值：`0`。）
* `security.headers.cache`：开启缓存控制HTTP头。（默认值：`false`。）
* `security.headers.content-type`： 开启`X-Content-Type-Options`头。（默认值：`false`。）
* `security.headers.frame`：开启`X-Frame-Options`头。（默认值：`false`。）
* `security.headers.hsts`：HTTP Strict Transport Security（HSTS）模式（可设置为`none`、`domain`、`all`）。
* `security.headers.xss`：开启跨站脚本（cross-site scripting）保护。（默认值：`false`。）
* `security.ignored`：要从默认保护路径中排除掉的路径列表，用逗号分隔。
* `security.oauth2.client.access-token-uri`：用于获取访问令牌的URI。
* `security.oauth2.client.access-token-validity-seconds`：在令牌过期前多长时间验证一次。
*`security.oauth2.client.additional-information.[key]`：设置额外的信息，令牌授予者会将其添加到令牌里。
* `security.oauth2.client.authentication-scheme`：传送持有人令牌（bearer token）的方法，包括`form`、`header`、`none`、`query`，可选其一。（默认值：`header`。）
* `security.oauth2.client.authorities`：要赋予经授权客户端的权限。
* `security.oauth2.client.authorized-grant-types`：客户端可用的授予类型。
* `security.oauth2.client.auto-approve-scopes`：客户端自动通过的范围。
* `security.oauth2.client.client-authentication-scheme`：在客户端身份认证时用于传输身份认证信息的方法，包括`form`、`header`、`none`、`query`，可选其一。（默认值：`header`。）
* `security.oauth2.client.client-id`：OAuth2客户端ID。
* `security.oauth2.client.client-secret`：OAuth2客户端密钥。默认随机生成。
* `security.oauth2.client.grant-type`：获得资源访问令牌的授予类型。
* `security.oauth2.client.id`：应用程序的客户端ID。
* `security.oauth2.client.pre-established-redirect-uri`：与服务器预先建立好的重定向URI。如果设置了该属性，用户授权请求中的重定向URI会被忽略，因为服务器不需要它。
* `security.oauth2.client.refresh-token-validity-seconds`：刷新令牌在过期前的有效时间。
* `security.oauth2.client.registered-redirect-uri`：客户端里注册的重定向URI，用逗号分隔。
* `security.oauth2.client.resource-ids`：与客户端关联的资源ID，用逗号分隔。
* `security.oauth2.client.scope`  
Scope assigned to the client.  
客户端分配的域。
* `security.oauth2.client.token-name`：令牌名称。
* `security.oauth2.client.use-current-uri`：请求里的当前URI（如果设置了的话）是否优先于预建立的重定向URI。（默认值：`true`。）
* `security.oauth2.client.user-authorization-uri`：用户要重定向以便授访问令牌的URI。
* `security.oauth2.resource.id`：资源的标识符。
* `security.oauth2.resource.jwt.key-uri`：JWT令牌的URI。如果没有配置`key-value`，使用的又是公钥，那么可以对这个属性进行设置。
* `security.oauth2.resource.jwt.key-value`：JWT令牌的验证密钥，可以是对称密钥，也可以是PEM编码的RSA公钥。如果没有配置这个属性，那么可以用`key-uri`代替。
* `security.oauth2.resource.prefer-token-info`：使用令牌的信息，设置为`false`则使用用户信息。（默认值：`true`。）
* `security.oauth2.resource.service-id`：服务ID。（默认值：`resource`。）
* `security.oauth2.resource.token-info-uri`：令牌解码端点URI。
* `security.oauth2.resource.token-type`：在使用`userInfoUri`时发送的令牌类型。
* `security.oauth2.resource.user-info-uri`：用户端点的URI。
* `security.oauth2.sso.filter-order`：在没有显式提供`WebSecurityConfigurerAdapter`时应用的过滤器顺序，在`WebSecurityConfigurerAdapter`里也可以指定顺序。
* `security.oauth2.sso.login-path`：登录页的路径——登录页是触发重定向到OAuth2授权服务器的页面。（默认值：`/login`。）
* `security.require-ssl`：对所有请求开启安全通道。（默认值：`false`。）
* `security.sessions`：创建会话使用的策略。（可选值包括：`always`、`never`、`if_required`、`stateless`。）
* `security.user.name`：默认的用户名。（默认值：`user`。）
* `security.user.password`：默认用户的密码。
* `security.user.role`：赋予默认用户的角色。
* `server.address`：服务器绑定的网络地址。
* `server.compression.enabled`：是否要开启压缩。（默认值：`false`。）
* `server.compression.excluded-user-agents`：用逗号分割的列表，标明哪些用户代理不该开启压缩。（可选值包括：`text/html`、`text/xml`、`text/plain`、`text/css`）
* `server.compression.mime-types`：要开启压缩的MIME类型列表，用逗号分割。
* `server.compression.min-response-size`：要执行压缩的最小响应大小（单位为字节）。（默认值：`2048`。）
* `server.context-parameters.[param name]`：设置一个Servlet上下文参数。
* `server.context-path`：应用程序的上下文路径。
* `server.display-name`：应用程序的显示名称。（默认值：`application`。）
* `server.jsp-servlet.class-name`：针对JSP使用的Servlet类名。（默认值：`org.apache.jasper.servlet.JspServlet`。）
* `server.jsp-servlet.init-parameters.[param name]`：设置JSP Servlet初始化参数。
* `server.jsp-servlet.registered`：JSP Servlet是否要注册到内嵌的Servlet容器里。（默认值：`true`。）
* `server.port`：服务器的HTTP端口。
* `server.servlet-path`：主分发器Servlet的路径。（默认值：`/`）
* `server.session.cookie.comment`：会话Cookie的注释。
* `server.session.cookie.domain`：
会话Cookie的域。
* `server.session.cookie.http-only`：会话Cookie的`HttpOnly`标记。
* `server.session.cookie.max-age`：会话Cookie的最大保存时间，单位为秒。
* `server.session.cookie.name`：会话Cookie名称。
* `server.session.cookie.path`：会话Cookie的路径。
* `server.session.cookie.secure`：会话Cookie的`Secure`标记。
* `server.session.persistent`：是否在两次重启间持久化会话数据。（默认值：`false`。）
* `server.session.timeout`：会话超时时间，单位为秒。
* `server.session.tracking-modes`：会话跟踪模式（包括：`cookie`、`url`和`ssl`，可选其一或若干）。
* `server.ssl.ciphers`：支持的SSL加密算法。
* `server.ssl.client-auth`：客户端授权是主动想（`want`）还是被动需要（`need`）。要有一个TrustStore。
* `server.ssl.enabled`：是否开启SSL。（默认值：`true`。）
* `server.ssl.key-alias`：在KeyStore里标识密钥的别名。
* `server.ssl.key-password`：在KeyStore里用于访问密钥的密码。
* `server.ssl.key-store`：持有SSL证书的KeyStore的路径（通常指向一个.jks文件）。
* `server.ssl.key-store-password`：访问KeyStore时使用的密钥。
* `server.ssl.key-store-provider` ：KeyStore的提供者。
* `server.ssl.key-store-type`：KeyStore的类型。
* `server.ssl.protocol`：要使用的SSL协议。（默认值：`TLS`。）
* `server.ssl.trust-store`：持有SSL证书的TrustStore。
* `server.ssl.trust-store-password`：用于访问TrustStore的密码。
* `server.ssl.trust-store-provider`：TrustStore的提供者。
* `server.ssl.trust-store-type`：TrustStore的类型。
* `server.tomcat.access-log-enabled`：是否开启访问日志。（默认值：`false`。）
* `server.tomcat.access-log-pattern`：访问日志的格式。（默认值：`common`。）
* `server.tomcat.accesslog.directory`：创建日志文件的目录。可以相对于Tomcat基础目录，也可以是绝对路径。（默认值：`logs`。）
* `server.tomcat.accesslog.enabled`：开启访问日志。（默认值：`false`。）
* `server.tomcat.accesslog.pattern`：访问日志的格式。（默认值：`common`。）
* `server.tomcat.accesslog.prefix`：日志文件名的前缀。（默认值：`access_log`。）
* `server.tomcat.accesslog.suffix`日志文件名的后缀。（默认值：`.log`。）
* `server.tomcat.background-processor-delay`：两次调用`backgroundProcess`方法之间的延迟时间，单位为秒。（默认值：`30`。）
* `server.tomcat.basedir`：Tomcat的基础目录。如果没有指定则使用一个临时目录。
* `server.tomcat.internal-proxies`  
匹配可信任的代理服务器的正则表达式。默认值：`“10\.\d{1,3}\.\d{1,3}\.\d{1,3}| 192\.168\.\d{1,3}\.\d{1,3}| 169\.254\.\d{1,3}\.\d{1,3}| 127\.\d{1,3}\.\d{1,3}\.\d{1,3}| 172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}| 172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}”`。
* `server.tomcat.max-http-header-size`：HTTP消息头的最大字节数。（默认值：`0`。）
* `server.tomcat.max-threads`：最大工作线程数。（默认值：`0`。）
* `server.tomcat.port-header`：用来覆盖原始端口值的HTTP头的名字。
* `server.tomcat.protocol-header`：持有流入协议的HTTP头，通常的名字是`X-Forwarded-Proto`。仅当设置了`remoteIpHeader`的时候，它会被配置为`RemoteIpValve`。
* `server.tomcat.protocol-header-https-value`： 协议头的值，表明流入请求使用了SSL。（默认值：`https`。）
* `server.tomcat.remote-ip-header`：表明从哪个HTTP头里可以提取到远端IP。仅当设置了`remoteIpHeader`的时候，它会被配置为`RemoteIpValve`。
* `server.tomcat.uri-encoding`：用来解码URI的字符编码。
* `server.undertow.access-log-dir`：Undertow的访问日志目录。（默认值：`logs`。）
* `server.undertow.access-log-enabled`：是否开启访问日志。（默认值：`false`。）
* `server.undertow.access-log-pattern`：访问日志的格式。（默认值：`common`。）
* `server.undertow.accesslog.dir`：Undertow访问日志目录。
* `server.undertow.accesslog.enabled`：开启访问日志。（默认值：`false`。）
* `server.undertow.accesslog.pattern`：访问日志的格式。（默认值：`common`。）
* `server.undertow.buffer-size`：每个缓冲的字节数。
* `server.undertow.buffers-per-region`每个区（region）的缓冲数。
* `server.undertow.direct-buffers`：在Java堆外分配缓冲。
* `server.undertow.io-threads`：要为工作线程创建的I/O线程数。
* `server.undertow.worker-threads`：工作线程数。
* `spring.activemq.broker-url`：ActiveMQ代理的URL。默认自动生成。
* `spring.activemq.in-memory`：标明默认代理URL是否应该在内存里。如果指定了一个显式的代理则忽略该属性。（默认值：`true`。）
* `spring.activemq.password`：代理的登录密码。
* `spring.activemq.pooled`：标明是否要创建一个`PooledConnectionFactory`来代替普通的`ConnectionFactory`。（默认值：`false`。）
* `spring.activemq.user`：代理的登录用户名。
* `spring.aop.auto`：添加`@EnableAspectJAutoProxy`。（默认值：`true`。）
* `spring.aop.proxy-target-class`：是否要创建基于子类（即Code Generation Library，CGLIB）的代理来代替基于Java接口的代理，前者为`true`，后者为`false`。（默认值：`false`。）
* `spring.application.admin.enabled`：开启应用程序的管理功能。（默认值：`false`。）
* `spring.application.admin.jmx-name`：应用程序管理MBean的JMX名称。（默认值：`org.springframework.boot:type=Admin,name=SpringApplication`。）
* `spring.artemis.embedded.cluster-password`：集群密码。默认在启动时随机生成。
* `spring.artemis.embedded.data-directory`：Journal文件目录。如果关闭了持久化则不需要该属性。
* `spring.artemis.embedded.enabled`：如果有Artemis服务器API则开启嵌入模式。（默认值：`true`。）
* `spring.artemis.embedded.persistent`：开启持久化存储。（默认值：`false`。）
* `spring.artemis.embedded.queues`：要在启动时创建的队列列表，用逗号分隔。（默认值：`[]`。）
* `spring.artemis.embedded.server-id`：服务器ID。默认情况下，使用一个自动递增的计数器。（默认值：`0`。）
* `spring.artemis.embedded.topics`：在启动时要创建的主题列表，用逗号分隔。（默认值：`[]`。）
* `spring.artemis.host`：Artemis代理主机。（默认值：`localhost`。）
* `spring.artemis.mode`：Artemis部署模式，默认自动检测。可以显式地设置为`native`或`embedded`。
* `spring.artemis.port`：Artemis代理端口。（默认值：`61616`。）
* `spring.autoconfigure.exclude`：要排除的自动配置类。
* `spring.batch.initializer.enabled`： 如果有必要的话，在启动时创建需要的批处理表。（默认值：`true`。）
* `spring.batch.job.enabled`：在启动时执行上下文里的所有Spring Batch任务。（默认值：`true`。）
* `spring.batch.job.names`：启动时要执行的任务名列表，用逗号分隔。默认在上下文里找到的所有任务都会执行。
* `spring.batch.schema`：指向初始化数据库Schema用的SQL文件的路径。（默认值：`classpath:org/springframework/batch/core/schema-@@platform@@.sql`。）
* `spring.batch.table-prefix`：所有批处理元数据表的表前缀。
* `spring.cache.cache-names`：如果底层缓存管理器支持缓存名的话，可以在这里指定要创建的缓存名列表，用逗号分隔。通常这会禁用运行时创建其他额外缓存的能力。
* `spring.cache.ehcache.config`：用来初始化EhCache的配置文件的位置。
* `spring.cache.guava.spec`：用来创建缓存的Spec。要获得有关Spec格式的详细情况，可以查看`CacheBuilderSpec`。
* `spring.cache.hazelcast.config`： 用来初始化Hazelcast的配置文件的位置。
* `spring.cache.infinispan.config`：用来初始化Infinispan的配置文件的位置。
* `spring.cache.jcache.config`：用来初始化缓存管理器的配置文件的位置。配置文件依赖于底层的缓存实现。
* `spring.cache.jcache.provider`：
`CachingProvider`实现的全限定类名，用来获取JSR-107兼容的缓存管理器，仅在Classpath里有不只一个JSR-107实现时才需要这个属性。
* `spring.cache.type`：缓存类型，默认根据环境自动检测。
* `spring.dao.exceptiontranslation.enabled`：  打开`PersistenceExceptionTranslationPostProcessor`。（默认值：`true`。）
* `spring.data.elasticsearch.cluster-name`：Elasticsearch集群名。（默认值：`elasticsearch`）
* `spring.data.elasticsearch.cluster-nodes`：集群节点地址列表，用逗号分隔。如果没有指定，就启动一个客户端节点。
* `spring.data.elasticsearch.properties`：用来配置客户端的额外属性。
* `spring.data.elasticsearch.repositories.enabled`：开启Elasticsearch仓库。（默认值：`true`。）
* `spring.data.jpa.repositories.enabled`：开启JPA仓库。（默认值：`true`。）
* `spring.data.mongodb.authentication-database`：身份认证数据库名。
* `spring.data.mongodb.database`：数据库名。
* `spring.data.mongodb.field-naming-strategy`：要使用的`FieldNamingStrategy`的全限定名。
* `spring.data.mongodb.grid-fs-database`：GridFS数据库名称。
* `spring.data.mongodb.host`：Mongo服务器主机地址。
* `spring.data.mongodb.password`：Mongo服务器的登录密码。
* `spring.data.mongodb.port`：Mongo服务器端口号。
* `spring.data.mongodb.repositories.enabled`：开启Mongo仓库。（默认值：`true`。）
* `spring.data.mongodb.uri`：Mongo数据库URI。设置了该属性后就主机和端口号会被忽略。（默认值：`mongodb://localhost/test`。）
* `spring.data.mongodb.username`：Mongo服务器的登录用户名。
* `spring.data.rest.base-path`：用于发布仓库资源的基本路径。
* `spring.data.rest.default-page-size`：分页数据的默认页大小。（默认值：`20`。）
* `spring.data.rest.limit-param-name`：用于标识一次返回多少记录的URL查询字符串参数名。（默认值：`size`。）
* `spring.data.rest.max-page-size`：最大分页大小。（默认值：`1000`。）
* `spring.data.rest.page-param-name`：URL查询字符串参数的名称，用来标识返回哪一页。（默认值：`page`。）
* `spring.data.rest.return-body-on-create`：在创建实体后是否返回一个响应体。（默认值：`false`。）
* `spring.data.rest.return-body-on-update`：在更新实体后是否返回一个响应体。（默认值：`false`。）
* `spring.data.rest.sort-param-name`：URL查询字符串参数的名称，用来标识结果排序的方向。（默认值：`sort`。）
* `spring.data.solr.host`：Solr的主机地址。如果设置了`zk-host`则忽略该属性。（默认值：`http://127.0.0.1:8983/solr`。）
* `spring.data.solr.repositories.enabled`：开启Solr仓库。（默认值：`true`。）
* `spring.data.solr.zk-host`：ZooKeeper主机地址，格式为“主机:端口”。
* `spring.datasource.abandon-when-percentage-full`：一个百分比形式的阈值，超过该阈值则关闭并报告被弃用（超时）的连接。
* `spring.datasource.allow-pool-suspension`： 是否允许池暂停（pool suspension）。在开启池暂停后会有性能会受到一定影响，除非你真的需要这个功能（例如在冗余的系统下），否则不要开启它。该属性只在使用Hikari数据库连接池时有用。（默认值：`false`。）
* `spring.datasource.alternate-username-allowed`：是否允许使用其他用户名。
* `spring.datasource.auto-commit`：更新操作是否自动提交。
* `spring.datasource.catalog`：默认的Catalog名称。
* `spring.datasource.commit-on-return`：在连接归还时，连接池是否要提交挂起的事务。
* `spring.datasource.connection-init-sql`：在所有新连接创建时都会执行的SQL语句，该语句会在连接加入连接池前执行。
* `spring.datasource.connection-init-sqls`：在物理连接第一次创建时执行的SQL语句列表。（用于DBCP连接池。）
* `spring.datasource.connection-properties.[key]`：设置创建连接时使用的属性。（用于DBCP连接池。）
* `spring.datasource.connection-test-query`：用于测试连接有效性的SQL查询。
* `spring.datasource.connection-timeout`：连接超时（单位为毫秒）。
* `spring.datasource.continue-on-error`：初始化数据库时发生错误不要终止。（默认值：`false`。）
* `spring.datasource.data`  
Data (DML) script resource reference.  
指向数据（数据库操纵语言，Data Manipulation Language，DML）脚本资源的引用。
* `spring.datasource.data-source-class-name`：用于获取连接的数据源的全限定类名。
* `spring.datasource.data-source-jndi`：用于获取连接的数据源的JNDI位置。
* `spring.datasource.data-source-properties.[key]`：设置创建数据源时使用的属性。（用于Hikari连接池。）
* `spring.datasource.db-properties`：设置创建数据源时使用的属性。（用于Tomcat连接池。）
* `spring.datasource.default-auto-commit`：连接上的操作是否自动提交。
* `spring.datasource.default-catalog`：连接的默认Catalog。
* `spring.datasource.default-read-only`：连接的默认只读状态。
* `spring.datasource.default-transaction-isolation`：连接的默认事务隔离级别。
* `spring.datasource.driver-class-name`：JDBC驱动的全限定类名。默认根据URL自动检测。
* `spring.datasource.fair-queue`：是否以FIFO方式返回连接。
* `spring.datasource.health-check-properties.[key]`：设置要纳入健康检查的属性。（用于Hikari连接池。）
* `spring.datasource.idle-timeout`：连接池中的连接能保持闲置状态的最长时间，单位为毫秒。（默认值：`10`。）
* `spring.datasource.ignore-exception-on-pre-load`：初始化数据库连接池时是否要忽略连接。
* `spring.datasource.init-sql`：在连接第一次创建时运行的自定义查询。
* `spring.datasource.initial-size`：在连接池启动时要建立的连接数。
* `spring.datasource.initialization-fail-fast`：在连接池创建时，如果达不到最小连接数是否要抛出异常。（默认值：`true`。）
* `spring.datasource.initialize`：使用data.sql初始化数据库。（默认值：`true`。）
* `spring.datasource.isolate-internal-queries`：是否要隔离内部请求。（默认值：`false`。）
* `spring.datasource.jdbc-interceptors`：一个分号分隔的类名列表，这些类都扩展了`JdbcInterceptor`类。这些拦截器会插入`java.sql.Connection`对象的操作链里。（用于Tomcat连接池。）
* `spring.datasource.jdbc-url`：用来创建连接的JDBC URL。
* `spring.datasource.jmx-enabled`：开启JMX支持（如果底层连接池提供该功能的话）。（默认值：`false`。）
*` spring.datasource.jndi-name`：数据源的JNDI位置。设置了该属性则忽略类、URL、用户名和密码属性。
* `spring.datasource.leak-detection-threshold`：用来检测Hikari连接池连接泄露的阈值，单位为毫秒。
* `spring.datasource.log-abandoned`：是否针对弃用语句或连接的应用程序代码记录下跟踪栈。用于DBCP连接池。（默认值：`false`。）
* `spring.datasource.log-validation-errors`：在使用Tomcat连接池时是否要记录验证错误。
* `spring.datasource.login-timeout`：连接数据库的超时时间（单位为秒）。
* `spring.datasource.max-active`：连接池中的最大活跃连接数。
* `spring.datasource.max-age`：连接池中连接的最长寿命。
* `spring.datasource.max-idle`：连接池中的最大空闲连接数。
* `spring.datasource.max-lifetime`：连接池中连接的最长寿命（单位为毫秒）。
* `spring.datasource.max-open-prepared-statements`：开启状态的`PreparedStatement`的数量上限。
* `spring.datasource.max-wait`：连接池在等待返回连接时，最长等待多少毫秒再抛出异常。
* `spring.datasource.maximum-pool-size`：连接池能达到的最大规模，包含空闲连接的数量和使用中的连接数量。
* `spring.datasource.min-evictable-idle-time-millis`：一个空闲连接被空闲连接释放器（如果存在的话）优雅地释放前，最短会在连接池里停留多少时间。
* `spring.datasource.min-idle`：连接池里始终应该保持的最小连接数。（用于DBCP和Tomcat连接池。）
* `spring.datasource.minimum-idle`HikariCP试图在连接池里维持的最小空闲连接数。
* `spring.datasource.name`：数据源的名称。
* `spring.datasource.num-tests-per-eviction-run`：空闲对象释放器线程（如果存在的话）每次运行时要检查的对象数。
* `spring.datasource.password`：数据库的登录密码。
* `spring.datasource.platform`：在Schema资源（schema-${platform}.sql）里要使用的平台。（默认值：`all`。）
* `spring.datasource.pool-name`：连接池名称。
* `spring.datasource.pool-prepared-statements`：是否要将`Statement`放在池里。
* `spring.datasource.propagate-interrupt-state`：对于等待连接的中断线程，是否要传播中断状态。
* `spring.datasource.read-only`在使用Hikari连接池时将数据源设置为只读。
* `spring.datasource.register-mbeans`：Hikari连接池是否要注册JMX MBean。
* `spring.datasource.remove-abandoned`：被弃用的连接在到达弃用超时后是否应该被移除。
* `spring.datasource.remove-abandoned-timeout`：连接在多少秒后应该考虑弃用。
* `spring.datasource.rollback-on-return`：在连接归还连接池时，是否要回滚挂起的事务。
* `spring.datasource.schema`：Schema（数据定义语言，Data Definition Language，DDL）脚本资源的引用。
* `spring.datasource.separator`：SQL初始化脚本里的语句分割符。（默认值：`;`。）
* `spring.datasource.sql-script-encoding`：SQL脚本的编码。
* `spring.datasource.suspect-timeout`：在记录一个疑似弃用连接前要等待多少秒。
* `spring.datasource.test-on-borrow`：从连接池中借用连接时是否要进行测试。
* `spring.datasource.test-on-connect`：在建立连接时是否要进行测试。
* `spring.datasource.test-on-return`：在将连接归还到连接池时是否要进行测试。
* `spring.datasource.test-while-idle`：在连接空闲时是否要进行测试。
* `spring.datasource.time-between-eviction-runs-millis`：在两次空闲连接验证、弃用连接清理和空闲池大小调整之间睡眠的毫秒数。
* `spring.datasource.transaction-isolation`：在使用Hikari连接池时设置默认事务隔离级别。
* `spring.datasource.url`：数据库的JDBC URL。
* `spring.datasource.use-disposable-connection-facade`：连接是否要用一个门面（facade）封装起来，在调用了`Connection.close()`后就不能再使用这个连接了。
* `spring.datasource.use-equals`：在比较方法名时是否使用`String.equals()`来代替`==`。
* `spring.datasource.use-lock`：在操作连接对象时是否要加锁。
* `spring.datasource.username`：数据库的登录用户名。
* `spring.datasource.validation-interval`：执行连接验证的间隔时间，单位为毫秒。
* `spring.datasource.validation-query`：在连接池里的连接返回给调用者或连接池时，要执行的验证SQL查询。
* `spring.datasource.validation-query-timeout`：在连接验证查询执行失败前等待的超时时间，单位为秒。
* `spring.datasource.validation-timeout`：在连接验证失败前等待的超时时间，单位为秒。（用于Hikari连接池。）
* `spring.datasource.validator-class-name`：可选验证器类的全限定类名，用于执行测试查询。
* `spring.datasource.xa.data-source-class-name`：XA数据源的全限定类名。
* `spring.datasource.xa.properties`：要传递给XA数据源的属性。
* `spring.freemarker.allow-request-override`：`HttpServletRequest`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.freemarker.allow-session-override`：`HttpSession`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.freemarker.cache`：开启模板缓存。
* `spring.freemarker.charset`：模板编码。
* `spring.freemarker.check-template-location`：检查模板位置是否存在。
* `spring.freemarker.content-type`：`Content-Type`的值。
* `spring.freemarker.enabled`：开启FreeMarker的MVC视图解析。
* `spring.freemarker.expose-request-attributes`：在模型合并到模板前，是否要把所有的请求属性添加到模型里。
* `spring.freemarker.expose-session-attributes`：在模型合并到模板前，是否要把所有的`HttpSession`属性添加到模型里。
* `spring.freemarker.expose-spring-macro-helpers`：是否发布供Spring宏程序库使用的`RequestContext`，并将命其名为`springMacroRequestContext`。
* `spring.freemarker.prefer-file-system-access`：加载模板时优先通过文件系统访问。文件系统访问能够实时检测到模板变更。（默认值：`true`。）
* `spring.freemarker.prefix`：在构建URL时添加到视图名称前的前缀。
* `spring.freemarker.request-context-attribute`：在所有视图里使用的`RequestContext`属性的名称。
* `spring.freemarker.settings`：要传递给FreeMarker配置的各种键。
* `spring.freemarker.suffix`：在构建URL时添加到视图名称后的后缀。
* `spring.freemarker.template-loader-path`：模板路径列表，用逗号分隔。（默认值：`["classpath:/templates/"]`。）
* `spring.freemarker.view-names`：可解析的视图名称的白名单。
* `spring.groovy.template.allow-request-override`：`HttpServletRequest`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.groovy.template.allow-session-override`：`HttpSession`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.groovy.template.cache`：开启模板缓存。
* `spring.groovy.template.charset`：模板编码。
* `spring.groovy.template.check-template-location`：检查模板位置是否存在。
* `spring.groovy.template.configuration.auto-escape`：模型变量在模板里呈现时是否要做转义。（默认值：`false`。）
* `spring.groovy.template.configuration.auto-indent`：模板是否要自动呈现缩进。（默认值：`false`。）
* `spring.groovy.template.configuration.auto-indent-string`：开启自动缩进时用于缩进的字符串，可以是`SPACES`，也可以是`TAB`。（默认值：`SPACES`。）
* `spring.groovy.template.configuration.auto-new-line`：模板里是否要呈现新的空行。（默认值：`false`。）
* `spring.groovy.template.configuration.base-template-class`：模板基类。
* `spring.groovy.template.configuration.cache-templates`：模板是否应该缓存。（默认值：`true`。）
* `spring.groovy.template.configuration.declaration-encoding`：用来写声明头的编码。
* `spring.groovy.template.configuration.expand-empty-elements`：没有正文的元素该用短形式（例如，`<br/>`）还是扩展形式（例如，`<br></br>`）来书写。（默认值：`false`。）
* `spring.groovy.template.configuration.locale`：设置模板地域。
* `spring.groovy.template.configuration.new-line-string`：在自动空行开启后用来呈现空行的字符串。（默认为系统的`line.separator`属性值。）
* `spring.groovy.template.configuration.resource-loader-path`：Groovy模板的路径。（默认值：`classpath:/templates/`。）
* `spring.groovy.template.configuration.use-double-quotes`：属性是该用双引号还是单引号。（默认值：`false`。）
* `spring.groovy.template.content-type`：`Content-Type`的值。
* `spring.groovy.template.enabled`：开启Groovy模板的MVC视图解析。
* `spring.groovy.template.expose-request-attributes`：在模型合并到模板前，是否要把所有的请求属性添加到模型里。
* `spring.groovy.template.expose-session-attributes`：在模型合并到模板前，是否要把所有的`HttpSession`属性添加到模型里。
* `spring.groovy.template.expose-spring-macro-helpers`：是否发布供Spring宏程序库使用的`RequestContext`，并将其命名为`springMacroRequestContext`。
* `spring.groovy.template.prefix`：在构建URL时，添加到视图名称前的前缀。
* `spring.groovy.template.request-context-attribute`：所有视图里使用的`RequestContext`属性的名称。
* `spring.groovy.template.resource-loader-path`：（默认值：`classpath:/templates/`。）
* `spring.groovy.template.suffix`：在构建URL时，添加到视图名称后的后缀。
* `spring.groovy.template.view-names`：可解析的视图名称白名单。
* `spring.h2.console.enabled`：开启控制台。（默认值：`false`。）
* `spring.h2.console.path`：可以找到控制台的路径。（默认值：`/h2-console`。）
* `spring.hateoas.apply-to-primary-object-mapper`：指定主`ObjectMapper`是否要应用HATEOAS支持。（默认值：`true`。）
* `spring.hornetq.embedded.cluster-password`：集群密码。默认在启动时随机生成。
* `spring.hornetq.embedded.data-directory`：日志文件目录。如果关闭了持久化功能则不需要该属性。
* `spring.hornetq.embedded.enabled`：如果有HornetQ服务器API，则开启嵌入模式。（默认值：`true`。）
* `spring.hornetq.embedded.persistent`：开启持久化存储。（默认值：`false`。）
* `spring.hornetq.embedded.queues`：启动时要创建的队列列表，用逗号分隔。（默认值：`[]`。）
* `spring.hornetq.embedded.server-id`：服务器ID。默认使用自增长计数器。（默认值：`0`。）
* `spring.hornetq.embedded.topics`：启动时要创建的主题列表，用逗号分隔。（默认值：`[]`。）
* `spring.hornetq.host`：HornetQ的主机。（默认值：`localhost`。）
* `spring.hornetq.mode`：HornetQ的部署模式，默认为自动检测。可以显式地设置为`native`或`embedded`。
* `spring.hornetq.port`：HornetQ的端口。（默认值：`5445`。）
* `spring.http.converters.preferred-json-mapper`：HTTP消息转换时优先使用JSON映射器。
* `spring.http.encoding.charset`：HTTP请求和响应的字符集。如果没有显式地指定`Content-Type`头，则将该属性值作为这个头的值。（默认值：`UTF-8`。）
* `spring.http.encoding.enabled`：开启HTTP编码支持。（默认值：`true`）
* `spring.http.encoding.force`：强制将HTTP请求和响应编码为所配置的字符集。（默认值：`true`）
* `spring.jackson.date-format`：日期格式字符串（yyyy-MM-dd HH:mm:ss）或日期格式类的全限定类名。
* `spring.jackson.deserialization`：影响Java对象反序列化的Jackson on/off特性。
* `spring.jackson.generator`：用于生成器的Jackson on/off特性。
* `spring.jackson.joda-date-time-format`：Joda日期时间格式字符串（yyyy-MM-dd HH:mm:ss）。如果没有配置，而`date-format`又配置了一个格式字符串的话，会将它作为降级配置。
* `spring.jackson.locale`：用于格式化的地域值。
* `spring.jackson.mapper`：Jackson的通用on/off特性。
* `spring.jackson.parser`：用于解析器的Jackson on/off特性。
* `spring.jackson.property-naming-strategy`：Jackson的`PropertyNamingStrategy`中的一个常量（`CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES`）。也可以设置`PropertyNamingStrategy`的子类的全限定类名。
* `spring.jackson.serialization`：影响Java对象序列化的Jackson on/off特性。
* `spring.jackson.serialization-inclusion`：控制序列化时要包含哪些属性。可选择Jackson的`JsonInclude.Include`枚举里的某个值。
* `spring.jackson.time-zone`：格式化日期时使用的时区。可以配置各种可识别的时区标识符，比如`America/Los_Angeles`或者`GMT+10`。
* `spring.jersey.filter.order`：Jersey过滤器链的顺序。（默认值：`0`）
* `spring.jersey.init`：通过Servlet或过滤器传递给Jersey的初始化参数。
* `spring.jersey.type`：Jersey集成类型。可以是`servlet`或者`filter`。
* `spring.jms.jndi-name`：连接工厂的JNDI名字。设置了该属性，则优先于其他自动配置的连接工厂。
* `spring.jms.listener.acknowledge-mode`：容器的应答模式（acknowledgment mode）。默认情况下，监听器使用自动应答。
* `spring.jms.listener.auto-startup`：启动时自动启动容器。（默认值：`true`。）
* `spring.jms.listener.concurrency`：并发消费者的数量下限。
* `spring.jms.listener.max-concurrency`：并发消费者的数量上限。
* `spring.jms.pub-sub-domain`：如果是主题而非队列，指明默认的目的地类型是否支持Pub/Sub。（默认值：`false`。）
* `spring.jmx.default-domain`：JMX域名。
* `spring.jmx.enabled`：将管理Bean发布到JMX域里。（默认值：`true`。）
* `spring.jmx.server`：`MBeanServer`的Bean名称。（默认值：`mbeanServer`。）
* `spring.jooq.sql-dialect`：在与配置的数据源通信时，JOOQ使用的`SQLDialect`，比如`POSTGRES`。
* `spring.jpa.database`：要操作的目标数据库，默认自动检测。也可以通过`databasePlatform`属性进行设置。
* `spring.jpa.database-platform`：要操作的目标数据库，默认自动检测。也可以通过`Database`枚举来设置。
* `spring.jpa.generate-ddl`：启动时要初始化Schema。（默认值：`false`）
* `spring.jpa.hibernate.ddl-auto`：DDL模式（`none`、`validate`、`update`、`create`和`create-drop`）。这是`hibernate.hbm2ddl.auto`属性的一个快捷方式。在使用嵌入式数据库时，默认为`create-drop`；其他情况下默认为`none`。
* `spring.jpa.hibernate.naming-strategy`：Hibernate命名策略的全限定类名。
* `spring.jpa.open-in-view`：注册`OpenEntityManagerInViewInterceptor`，在请求的整个处理过程中，将一个JPA `EntityManager`绑定到线程上。（默认值：`true`。）
* `spring.jpa.properties`：JPA提供方要设置的额外原生属性。
* `spring.jpa.show-sql`：在使用Bitronix Transaction Manager时打开SQL语句日志。（默认值：`false`。）
* `spring.jta.allow-multiple-lrc`：在使用Bitronix Transaction Manager时，事务管理器是否应该允许一个事务涉及多个LRC资源。（默认值：`false`。）
* `spring.jta.asynchronous2-pc`：在使用Bitronix Transaction Manager时，是否异步执行两阶段提交。（默认值：`false`。）
* `spring.jta.background-recovery-interval`：在使用Bitronix Transaction Manager时，多久运行一次恢复过程，单位为分钟。（默认值：`1`。）
* `spring.jta.background-recovery-interval-seconds`：在使用Bitronix Transaction Manager时，多久运行一次恢复过程，单位为秒。（默认值：`60`。）
* `spring.jta.current-node-only-recovery`：在使用Bitronix Transaction Manager时，恢复是否要滤除不包含本JVM唯一ID的XID。（默认值：`true`。）
* `spring.jta.debug-zero-resource-transaction`：在使用Bitronix Transaction Manager时，对于没有涉及任何资源的事务，是否要跟踪并记录它们的创建和提交调用栈。（默认值：`false`。）
* `spring.jta.default-transaction-timeout`：在使用Bitronix Transaction Manager时，默认的事务超时时间，单位为秒。（默认值：`60`。）
* `spring.jta.disable-jmx`：在使用Bitronix Transaction Manager时，是否要禁止注册JMX MBean。（默认值：`false`。）
* `spring.jta.enabled`：开启JTA支持。（默认值：`true`。）
* `spring.jta.exception-analyzer`：在使用Bitronix Transaction Manager时用到的异常分析器。设置为`null`时使用默认异常分析器，也可以设置自定义异常分析器的全限定类名。
* `spring.jta.filter-log-status`：在使用Bitronix Transaction Manager时，是否只记录必要的日志。开启该参数时能减少分段（fragment）空间用量，但调试更复杂了。（默认值：`false`。）
* `spring.jta.force-batching-enabled`：在使用Bitronix Transaction Manager时，是否批量输出至磁盘。禁用批处理会严重降低事务管理器的吞吐量。（默认值：`true`。）
* `spring.jta.forced-write-enabled`：在使用Bitronix Transaction Manager时，日志是否强制写到磁盘上。在生产环境里不要设置为`false`，因为不强制写到磁盘上无法保证完整性。（默认值：`true`）
* `spring.jta.graceful-shutdown-interval`：在使用Bitronix Transaction Manager时，要关闭的话，事务管理器在放弃事务前最多等它多少秒。（默认值：`60`。）
* `spring.jta.jndi-transaction-synchronization-registry-name`：在使用Bitronix Transaction Manager时，事务同步注册表应该绑定到哪个JNDI下。（默认值：`java:comp/TransactionSynchronizationRegistry`。）
* `spring.jta.jndi-user-transaction-name`：在使用Bitronix Transaction Manager时，用户事务应该绑定到哪个JNDI下。（默认值：`java:comp/UserTransaction`。）
* `spring.jta.journal`：在使用Bitronix Transaction Manager时，要用的日志名。可以是`disk`、`null`或者全限定类名。（默认值：`disk`。）
* `spring.jta.log-dir`：事务日志目录。
* `spring.jta.log-part1-filename`：日志分段文件1的名称。（默认值：`btm1.tlog`）
* `spring.jta.log-part2-filename`：日志分段文件2的名称。（默认值：`btm2.tlog`。）
* `spring.jta.max-log-size-in-mb`：在使用Bitronix Transaction Manager时，日志分段文件的最大兆数。日志越大，事务就被允许在未终结状态停留越长时间。但是，如果文件大小限制得太小，事务管理器在分段满了的时候就会暂停更长时间。（默认值：`2`）
* `spring.jta.resource-configuration-filename`：Bitronix Transaction Manager的配置文件名。
* `spring.jta.server-id`：唯一标识Bitronix Transaction Manager实例的ID。
* `spring.jta.skip-corrupted-logs`：是否跳过损坏的日志文件。（默认值：`false`。）
* `spring.jta.transaction-manager-id`：事务管理器的唯一标识符。
* `spring.jta.warn-about-zero-resource-transaction`：在使用Bitronix Transaction Manager时，是否要对执行时没有涉及任何资源的事务作出告警。（默认值：`true`。）
* `spring.mail.default-encoding`：默认的`MimeMessage`编码。（默认值：`UTF-8`。）
* `spring.mail.host`：SMTP服务器主机地址。
* `spring.mail.jndi-name`： 会话的JNDI名称。设置之后，该属性的优先级要高于其他邮件设置。
* `spring.mail.password`：SMTP服务器的登录密码。
* `spring.mail.port`：SMTP服务器的端口号。
* `spring.mail.properties`：附加的JavaMail会话属性。
* `spring.mail.protocol`：SMTP服务器用到的协议。（默认值：`smtp`。）
* `spring.mail.test-connection`：在启动时测试邮件服务器是否可用。（默认值：`false`。）
* `spring.mail.username`：SMTP服务器的登录用户名。
* `spring.messages.basename`：逗号分隔的基本名称列表，都遵循`ResourceBundle`的惯例。本质上这就是一个全限定的Classpath位置，如果不包含包限定符（比如`org.mypackage`），就会从Classpath的根部开始解析。（默认值：`messages`。）
* `spring.messages.cache-seconds`：加载的资源包文件的缓存失效时间，单位为秒。在设置为`-1`时，包会永远缓存。（默认值：`-1`。）
* `spring.messages.encoding`：消息包的编码。（默认值：`UTF-8`。）
* `spring.mobile.devicedelegatingviewresolver.enable-fallback`：开启降级解析支持。（默认值：`false`。）
* `spring.mobile.devicedelegatingviewresolver.enabled`：开启设备视图解析器。（默认值：`false`。）
* `spring.mobile.devicedelegatingviewresolver.mobile-prefix`：添加到移动设备视图名前的前缀。（默认值：`mobile/`。）
* `spring.mobile.devicedelegatingviewresolver.mobile-suffix`：添加到移动设备视图名后的后缀。
* `spring.mobile.devicedelegatingviewresolver.normal-prefix`：添加到普通设备视图名前的前缀。
* `spring.mobile.devicedelegatingviewresolver.normal-suffix`：添加到普通设备视图名后的后缀。
* `spring.mobile.devicedelegatingviewresolver.tablet-prefix`：添加到平板设备视图名前的前缀。（默认值：`tablet/`。）
* `spring.mobile.devicedelegatingviewresolver.tablet-suffix`：添加到平板设备视图名后的后缀。
* `spring.mobile.sitepreference.enabled`：开启`SitePreferenceHandler`。（默认值：`true`。）
* `spring.mongodb.embedded.features`：要开启的特性列表，用逗号分隔。
* `spring.mongodb.embedded.version`：要使用的Mongo版本。（默认值：`2.6.10`。）
* `spring.mustache.cache`：开启模板缓存。
* `spring.mustache.charset`：模板编码。
* `spring.mustache.check-template-location`：检查模板位置是否存在。
* `spring.mustache.content-type`：`Content-Type`的值。
* `spring.mustache.enabled`：开启Mustache的MVC视图解析。
* `spring.mustache.prefix`：添加到模板名前的前缀。（默认值：`classpath:/templates/`。）
* `spring.mustache.suffix`：添加到模板名后的后缀。（默认值：`.html`。）
* `spring.mustache.view-names`：可解析的视图名称的白名单。
* `spring.mvc.async.request-timeout`：异步请求处理超时前的等待时间（单位为毫秒）。如果没有设置该属性，则使用底层实现的默认超时时间，比如，Tomcat上使用Servlet 3时超时时间为10秒。
* `spring.mvc.date-format`：要使用的日期格式（比如`dd/MM/yyyy`）。
* `spring.mvc.favicon.enabled`：开启favicon.ico的解析。（默认值：`true`。）
* `spring.mvc.ignore-default-model-on-redirect`：在重定向的场景下，是否要忽略“默认”模型对象的内容。（默认值：`true`。）
* `spring.mvc.locale`：要使用的地域配置。
* `spring.mvc.message-codes-resolver-format`：（`PREFIX_ERROR_CODE`、`POSTFIX_ERROR_CODE`）。
* `spring.mvc.view.prefix`：Spring MVC视图前缀。
* `spring.mvc.view.suffix`：Spring MVC视图后缀。
* `spring.rabbitmq.addresses`：客户端应该连接的地址列表，用逗号分隔。
* `spring.rabbitmq.dynamic`：创建一个`AmqpAdmin` Bean。（默认值：`true`。）
* `spring.rabbitmq.host`：RabbitMQ主机地址。（默认值：`localhost`。）
* `spring.rabbitmq.listener.acknowledge-mode`：容器的应答模式。
* `spring.rabbitmq.listener.auto-startup`：启动时自动开启容器。（默认值：`true`。）
* `spring.rabbitmq.listener.concurrency`：消费者的数量下限。
* `spring.rabbitmq.listener.max-concurrency`：消费者的数量上限。
* `spring.rabbitmq.listener.prefetch`：单个请求里要处理的消息数。该数值不应小于事务数（如果用到的话）。
* `spring.rabbitmq.listener.transaction-size`：一个事务里要处理的消息数。为了保证效果，应该不大于预先获取的数量。
* `spring.rabbitmq.password`：进行身份验证的密码。
* `spring.rabbitmq.port`：RabbitMQ端口。（默认值：`5672`。）
* `spring.rabbitmq.requested-heartbeat`：请求心跳超时，单位为秒；`0`表示不启用心跳。
* `spring.rabbitmq.ssl.enabled`：开启SSL支持。（默认值：`false`。）
* `spring.rabbitmq.ssl.key-store`：持有SSL证书的KeyStore路径。
* `spring.rabbitmq.ssl.key-store-password`：访问KeyStore的密码。
* `spring.rabbitmq.ssl.trust-store`：持有SSL证书的TrustStore。
* `spring.rabbitmq.ssl.trust-store-password`：访问TrustStore的密码。
* `spring.rabbitmq.username`：进行身份验证的用户名。
* `spring.rabbitmq.virtual-host`：在连接RabbitMQ时的虚拟主机。
* `spring.redis.database`：连接工厂使用的数据库索引。（默认值：`0`。）
* `spring.redis.host`：Redis服务器主机地址。（默认值：`localhost`。）
* `spring.redis.password`：Redis服务器的登录密码。
* `spring.redis.pool.max-active`：连接池在指定时间里能分配的最大连接数。负数表示无限制。（默认值：`8`。）
* `spring.redis.pool.max-idle`：连接池里的最大空闲连接数。负数表示空闲连接数可以是无限大。（默认值：`8`。）
* `spring.redis.pool.max-wait`：当连接池被耗尽时，分配连接的请求应该在抛出异常前被阻塞多长时间（单位为秒）。负数表示一直阻塞。（默认值：`-1`。）
* `spring.redis.pool.min-idle`：连接池里要维持的最小空闲连接数。该属性只有在设置为正数时才有效。（默认值：`0`。。）
* `spring.redis.port`：Redis服务器端口。（默认值：`6379`。）
* `spring.redis.sentinel.master`：Redis服务器的名字。
* `spring.redis.sentinel.nodes`：形如“主机:端口”配对的列表，用逗号分隔。
* `spring.redis.timeout`：连接超时时间，单位为秒。（默认值：`0`。）
* `spring.resources.add-mappings`：开启默认资源处理。（默认值：`true`。）
* `spring.resources.cache-period`：资源处理器对资源的缓存周期，单位为秒。
* `spring.resources.chain.cache`：对资源链开启缓存。（默认值：`true`。）
* `spring.resources.chain.enabled`：开启Spring资源处理链。（默认关闭的，除非至少开启了一个策略。）
* `spring.resources.chain.html-application-cache`：开启HTML5应用程序缓存证明重写。（默认值：`false`。）
* `spring.resources.chain.strategy.content.enabled`：开启内容版本策略。（默认值：`false`。）
* `spring.resources.chain.strategy.content.paths`：要运用于版本策略的模式列表，用逗号分隔。（默认值：`[/**]`。）
* `spring.resources.chain.strategy.fixed.enabled`：开启固定版本策略。（默认值：`false`。）
* `spring.resources.chain.strategy.fixed.paths`：要运用于固定版本策略的模式列表，用逗号分隔。
* `spring.resources.chain.strategy.fixed.version`：用于固定版本策略的版本字符串。
* `spring.resources.static-locations`：静态资源位置。默认为`classpath:[/META-INF/resources/, /resources/, /static/, /public/]`加上context:/（Servlet上下文的根目录）。
* `spring.sendgrid.password`：SendGrid密码。
* `spring.sendgrid.proxy.host`：SendGrid代理主机地址。
* `spring.sendgrid.proxy.port`：SendGrid代理端口。
* `spring.sendgrid.username`：SendGrid用户名。
* `spring.social.auto-connection-views`：针对所支持的提供方开启连接状态视图。（默认值：`false`。）
* `spring.social.facebook.app-id`：应用程序ID。
* `spring.social.facebook.app-secret`：应用程序的密钥。
* `spring.social.linkedin.app-id`：应用程序ID。
* `spring.social.linkedin.app-secret`：应用程序的密钥。
* `spring.social.twitter.app-id`：应用程序ID。
* `spring.social.twitter.app-secret`：应用程序的密钥。
* `spring.thymeleaf.cache`：开启模板缓存。（默认值：`true`。）
* `spring.thymeleaf.check-template-location`：检查模板位置是否存在。（默认值：`true`。）
* `spring.thymeleaf.content-type`：`Content-Type`的值。（默认值：`text/html`。）
* `spring.thymeleaf.enabled`：开启MVC Thymeleaf视图解析。（默认值：`true`。）
* `spring.thymeleaf.encoding`：模板编码。（默认值：`UTF-8`。）
* `spring.thymeleaf.excluded-view-names`：要被排除在解析之外的视图名称列表，用逗号分隔。
* `spring.thymeleaf.mode`：要运用于模板之上的模板模式。另见`StandardTemplateModeHandlers`。（默认值：`HTML5`。）
* `spring.thymeleaf.prefix`：在构建URL时添加到视图名称前的前缀。（默认值：`classpath:/templates/`。）
* `spring.thymeleaf.suffix`：在构建URL时添加到视图名称后的后缀。（默认值：`.html`。）
* `spring.thymeleaf.template-resolver-order`：Thymeleaf模板解析器在解析器链中的顺序。默认情况下，它排在第一位。顺序从1开始，只有在定义了额外的`TemplateResolver` Bean时才需要设置这个属性。
* `spring.thymeleaf.view-names`：可解析的视图名称列表，用逗号分隔。
* `spring.velocity.allow-request-override`：`HttpServletRequest`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.velocity.allow-session-override`：`HttpSession`的属性是否允许覆盖（隐藏）控制器生成的同名模型属性。
* `spring.velocity.cache`：开启模板缓存。
* `spring.velocity.charset`：模板编码。
* `spring.velocity.check-template-location`：检查模板位置是否存在。
* `spring.velocity.content-type`：`Content-Type`的值。
* `spring.velocity.date-tool-attribute`：`DateTool`辅助对象在视图的Velocity上下文里呈现的名字。
* `spring.velocity.enabled`：开启Velocity的MVC视图解析。
* `spring.velocity.expose-request-attributes`：在模型合并到模板前，是否要把所有的请求属性添加到模型里。
* `spring.velocity.expose-session-attributes`：在模型合并到模板前，是否要把所有的`HttpSession`属性添加到模型里。
* `spring.velocity.expose-spring-macro-helpers`：是否发布供Spring宏程序库使用的`RequestContext`，并将其名命为`springMacroRequestContext`。
* `spring.velocity.number-tool-attribute`：`NumberTool`辅助对象在视图的Velocity上下文里呈现的名字。
* `spring.velocity.prefer-file-system-access`：加载模板时优先通过文件系统访问。文件系统访问能够实时检测到模板变更。（默认值：`true`。）
* `spring.velocity.prefix`：在构建URL时添加到视图名称前的前缀。
* `spring.velocity.properties`：额外的Velocity属性。
* `spring.velocity.request-context-attribute`：所有视图里使用的`RequestContext`属性的名称。
* `spring.velocity.resource-loader-path`：模板路径。（默认值：`classpath:/templates/`。）
* `spring.velocity.suffix`：在构建URL时添加到视图名称后的后缀。
* `spring.velocity.toolbox-config-location`：Velocity Toolbox的配置位置，比如/WEB-INF/toolbox.xml。自动加载Velocity Tools工具定义文件，将所定义的全部工具发布到指定的作用域内。
* `spring.velocity.view-names`：可解析的视图名称白名单。
* `spring.view.prefix`：Spring MVC视图前缀。
* `spring.view.suffix`：Spring MVC视图后缀。
