## I. Кодовая база
### Одна кодовая база всегда находится в системе контроля версий и используется для множества деплоев

Приложение двенадцати факторов всегда находится в системе контроля версий, таких как
[Git](http://git-scm.com/),[Mercurial](http://mercurial.selenic.com/),
или [Subversion](http://subversion.apache.org/). Копия ревизии отслеживающеся базы данных
называется *кодовым репозиторием*, или "кодовым репо", или просто "репо".

*Кодовая база* - это любой отдельный репозиторий (в централизованных системам контроля версий, таких как
Subversion) или любой набор репозиториев, у которых есть общий корневой коммит (в децентрализованных
системах контроля версий, таких как Git)

![Одна кодовая база используется для множества деплоев](/images/codebase-deploys.png)

Всегда есть связь "один к одному" между кодовай базой и приложением:

* Если есть много кодобых баз, то это не приложение -- это распределенная система. Каждый компонент
в распределенной системе - приложение, каждое из которых в отдельности соответствует двенадцати факторам.
* Множество приложений, делящих между собой одну кодовую базу нарушают двенадцать факторов.
Решение для такой ситуации - включить общий код в библиотеки, которые можно подключить с помощью
[менеджера зависимостей](/dependencies).

There is only one codebase per app, but there will be many deploys of the app.  A *deploy* is a running instance of the app.  This is typically a production site, and one or more staging sites.  Additionally, every developer has a copy of the app running in their local development environment, each of which also qualifies as a deploy.

The codebase is the same across all deploys, although different versions may be active in each deploy.  For example, a developer has some commits not yet deployed to staging; staging has some commits not yet deployed to production.  But they all share the same codebase, thus making them identifiable as different deploys of the same app.

