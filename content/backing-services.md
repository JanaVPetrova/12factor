## IV. Резервные сервисы
### Относитесь к резервным сервисам так же, как и к связанным

*Резервный сервис* - это любой сервис, ресурсы которого приложение использует во время своей обычной
работы. Примерами являются хранилища данных (такие как [MySQL](http://dev.mysql.com/) или
[CouchDB](http://couchdb.apache.org/)), системы обмена сообщениями (такие как [RabbitMQ](http://www.rabbitmq.com/)
или [Beanstalkd](http://kr.github.com/beanstalkd/)), SMTP сервисы для исходящих сообщений (например
[Postfix](http://www.postfix.org/)) и системы кеширования (такие как [Memcached](http://memcached.org/))

Backing services like the database are traditionally managed by the same systems administrators as the app's runtime deploy.  In addition to these locally-managed services, the app may also have services provided and managed by third parties.  Examples include SMTP services (such as [Postmark](http://postmarkapp.com/)), metrics-gathering services (such as [New Relic](http://newrelic.com/) or [Loggly](http://www.loggly.com/)), binary asset services (such as [Amazon S3](http://aws.amazon.com/s3/)), and even API-accessible consumer services (such as [Twitter](http://dev.twitter.com/), [Google Maps](http://code.google.com/apis/maps/index.html), or [Last.fm](http://www.last.fm/api)).

**The code for a twelve-factor app makes no distinction between local and third party services.**  To the app, both are attached resources, accessed via a URL or other locator/credentials stored in the [config](/config).  A [deploy](/codebase) of the twelve-factor app should be able to swap out a local MySQL database with one managed by a third party (such as [Amazon RDS](http://aws.amazon.com/rds/)) without any changes to the app's code.  Likewise, a local SMTP server could be swapped with a third-party SMTP service (such as Postmark) without code changes.  In both cases, only the resource handle in the config needs to change.

Each distinct backing service is a *resource*.  For example, a MySQL database is a resource; two MySQL databases (used for sharding at the application layer) qualify as two distinct resources.  The twelve-factor app treats these databases as *attached resources*, which indicates their loose coupling to the deploy they are attached to.

<img src="/images/attached-resources.png" class="full" alt="A production deploy attached to four backing services." />

Resources can be attached and detached to deploys at will.  For example, if the app's database is misbehaving due to a hardware issue, the app's administrator might spin up a new database server restored from a recent backup.  The current production database could be detached, and the new database attached -- all without any code changes.

