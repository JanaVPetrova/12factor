## V. Сборка, релиз, запуск
### Строго разделяйте этапы сборки и запуска

[Кодовая база](/codebase) трансформируется в (non-development) деплой в три этапа:

* *Этап сборки* - это преобразование репозитория в исполняемый пакет, который называют *сборка*.
Используя версию кода, определенную в процессе деплоя, на этапе сборки получают и упаковывают
[зависимости](/dependencies) и компилируют двоичные файлы и ассеты.
* На *этапе релиза* сборку, полученную на предыдущем этапе объединяют с текущей [конфигурацией](/config).
Получившийся *релиз*, содержащий в себе сборку и конфигурацию, готов к немедленному запуску в
среде выполнения.
* На *этапе запуска* (или "runtime") приложение запускают в среде выполнения, запуская некоторый
набор [процессов](/processes) для выбранного релиза.

![Code becomes a build, which is combined with config to create a release.](/images/release.png)

**The twelve-factor app uses strict separation between the build, release, and run stages.**  For example, it is impossible to make changes to the code at runtime, since there is no way to propagate those changes back to the build stage.

Deployment tools typically offer release management tools, most notably the ability to roll back to a previous release.  For example, the [Capistrano](https://github.com/capistrano/capistrano/wiki) deployment tool stores releases in a subdirectory named `releases`, where the current release is a symlink to the current release directory.  Its `rollback` command makes it easy to quickly roll back to a previous release.

Every release should always have a unique release ID, such as a timestamp of the release (such as `2011-04-06-20:32:17`) or an incrementing number (such as `v100`).  Releases are an append-only ledger and a release cannot be mutated once it is created.  Any change must create a new release.

Builds are initiated by the app's developers whenever new code is deployed.  Runtime execution, by contrast, can happen automatically in cases such as a server reboot, or a crashed process being restarted by the process manager.  Therefore, the run stage should be kept to as few moving parts as possible, since problems that prevent an app from running can cause it to break in the middle of the night when no developers are on hand.  The build stage can be more complex, since errors are always in the foreground for a developer who is driving the deploy.

