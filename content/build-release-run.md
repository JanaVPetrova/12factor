## V. Сборка, релиз, запуск
### Строго разделяйте этапы сборки и запуска

[Кодовая база](/codebase) трансформируется в (non-development) деплой в три этапа:

* *Этап сборки* - это преобразование репозитория в исполняемый пакет, который называют *сборка*.
Используя версию кода, определенную в процессе деплоя, на этапе сборки получают и упаковывают
[зависимости](/dependencies) и компилируют бинарные файлы и ассеты.
* На *этапе релиза* сборку, полученную на предыдущем этапе объединяют с текущей [конфигурацией](/config).
Получившийся *релиз*, содержащий в себе сборку и конфигурацию, готов к немедленному запуску в
среде выполнения.
* На *этапе запуска* (или "runtime") приложение запускают в среде выполнения, запуская некоторый
набор [процессов](/processes) для выбранного релиза.

![Code becomes a build, which is combined with config to create a release.](/images/release.png)

**Приложение двенадцати факторов строго разделяет этапы сборки, релиза и запуска.** Например, невозможно внести
изменения в код на этапе запуска, поскольку позже нельзя применить эти изменения на более ранней стадии сборки.

В инструменты деплоя как правило входят инструенты управления релизами, в частности возможность откатить
релиз на предыдущую версию. Например, [Capistrano](https://github.com/capistrano/capistrano/wiki)
хранит релизы в поддиректории `releases`; текущий релиз - это симлинк на директорию текущего релиза.
Команда `rollback` - это простой способ быстро откатиться на предыдущую версию релиза.

У каждого релиза должен быть уникальный ID, например таймстемп (такой как `2011-04-06-20:32:17`) или
инкрементируемый номер (`v100`). Релизы можно только добавлять и ни в коем случае их нельзя изменять.
Для любых изменений должен быть создан новый релиз.

Builds are initiated by the app's developers whenever new code is deployed.  Runtime execution, by contrast, can happen automatically in cases such as a server reboot, or a crashed process being restarted by the process manager.  Therefore, the run stage should be kept to as few moving parts as possible, since problems that prevent an app from running can cause it to break in the middle of the night when no developers are on hand.  The build stage can be more complex, since errors are always in the foreground for a developer who is driving the deploy.
