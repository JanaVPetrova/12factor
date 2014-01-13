## VI. Процессы
### Выполняйте приложение как один или несколько процессов, не зависящих от состояния

Приложение выполняется в среде выполнения как один или более *процессов*

В простейшем случае, код - это stand-alone скрипт, среда выполнения - это локальный компьютер разработчика
с установленной на нем языковой средой выполнения, а процесс запущен с помощью командной строки
(например, `python my_script.py`). С другий точки зрения, production-развертка сложного приложения
может использовать много [типов процессов, представленных нулем или более запущенных процессов](/concurrency)

**Процессы двенадцати факторов не зависят от состояния и являются
[share-nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture).** Любые данные, которые нужно
записать, хранятся в stateful [резервном сервисе](/backing-services), обычно в базе данных.

The memory space or filesystem of the process can be used as a brief, single-transaction cache.  For example, downloading a large file, operating on it, and storing the results of the operation in the database.  The twelve-factor app never assumes that anything cached in memory or on disk will be available on a future request or job -- with many processes of each type running, chances are high that a future request will be served by a different process.  Even when running only one process, a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.

Asset packagers (such as [Jammit](http://documentcloud.github.com/jammit/) or [django-assetpackager](http://code.google.com/p/django-assetpackager/)) use the filesystem as a cache for compiled assets.  A twelve-factor app prefers to do this compiling during the [build stage](/build-release-run), such as the [Rails asset pipeline](http://ryanbigg.com/guides/asset_pipeline.html), rather than at runtime.

Some web systems rely on ["sticky sessions"](http://en.wikipedia.org/wiki/Load_balancing_%28computing%29#Persistence) -- that is, caching user session data in memory of the app's process and expecting future requests from the same visitor to be routed to the same process.  Sticky sessions are a violation of twelve-factor and should never be used or relied upon.  Session state data is a good candidate for a datastore that offers time-expiration, such as [Memcached](http://memcached.org/) or [Redis](http://redis.io/).

