## VIII. Параллелизм
### Расширяйтесь с помощью модели процессов

Любая запущенная компьютерная программа представляет собой один или несколько процессов. Веб приложения запускаются по-разному.
Например, PHP процессы порождаются от Apache и стартуют по запросу. Процессы Java используют
противоположный подход: во время старта JVM запускает один сверхпроцесс, который резервирует большой кусок системных ресурсов
(ЦПУ и память), а параллелизм достигается засчет внутренних тредов. В обоих случаях разработчики приложения очень мало работают
с самими процессами.

![Масштабирование выражается в запущенных процессах, различные нагрузки - в типах процессов](/images/process-types.png)

**В приложении двенадцати факторов процессы имеют высокий приоритет.** Процессы в приложении двенадцати факторов принимают
строгие сигналы от [unix модели процессов для запуска сервисных демонов](http://adam.heroku.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/).
Используя эту модель, разработчик может проектировать свое приложение так, чтобы оно могло справится с различной нагрузкой,
назначая каждый тип работы определенному *типу процессов*. Например, запросы HTTP могут обрабатываться web-процессом, а долгие
фоновые задачи могут обрабаываться рабочим процессом.

Но отдельные процессы также могут иметь свое внутреннее мультиплексирование с помощью тредов внутри виртуалной машины, или
асинхронную модель событий, которые есть в таких инструментах как [EventMachine](http://rubyeventmachine.com/),
[Twisted](http://twistedmatrix.com/trac/), и [Node.js](http://nodejs.org/). Но отдельная виртуальная машина может только
расти (масштабироваться вертикально), поэтому приложение должно уметь работать с несколькими процессами, которые запущены на разных
физических машинах.

This does not exclude individual processes from handling their own internal multiplexing, via threads inside the runtime VM, or the async/evented model found in tools such as [EventMachine](http://rubyeventmachine.com/), [Twisted](http://twistedmatrix.com/trac/), or [Node.js](http://nodejs.org/).  But an individual VM can only grow so large (vertical scale), so the application must also be able to span multiple processes running on multiple physical machines.

Модель процессов показывает себя с лучшей стороны тогда, когда приходить время масштабирования.

The process model truly shines when it comes time to scale out.  The [share-nothing, horizontally partitionable nature of twelve-factor app processes](/processes) means that adding more concurrency is a simple and reliable operation.  The array of process types and number of processes of each type is known as the *process formation*.

Процессы приложения двенадцати факторов [никогда не демонизируются](http://dustin.github.com/2010/02/28/running-processes.html)
и никогда не пишут PID файлы. Вместо этого полагайтесь на менеждер процессов операционной системы
(например [Upstart](http://upstart.ubuntu.com/), распределенный менеджер процессов на облачной платформе, или такой инструмент
как [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) во время разработки), для управления
[потоками вывода](/logs), реагируйте на падение процессов и обрабатывайте перезапуски и выключения, инициированные пользователем.
