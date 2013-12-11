## II. Зависимости
### Явно указывайте и изолируйте зависимости

Большинство языков программирования предлагают пакетные менеджеры для распространения поддерживаемых
библиотек, такие как [CPAN](http://www.cpan.org/) для Perl или [Rubygems](http://rubygems.org/)
для Ruby. Библиотеки, установленные с помощью пакетного менеджера, можно установить как для
всей системы (в "site packages"), так и в отдельную директорию ("vendoring" или "bundling"),
содержащую приложение.

**Приложение двенадцати факторов никогда не полагается на скрытое существование в системе
какого-либо пакета.** Оно объявляет все зависимости точно и целиком с помощью манифеста
*объявления зависимостей* (*dependency declaration* manifest). Кроме того, оно использует инструмент
*изоляции зависимостей* во время исполнения, чтобы убедиться, что нет скрытых зависимостей, утечка
которых произошла из окружающей системы. Полная и явная спецификация зависимостей применяется и в
продакшн-среде, и в среде разработки.

Например, [Gem Bundler](http://gembundler.com/) для Ruby предлагает манифест в формате `Gemfile`
для объявления зависимостей и `bundle exec` для изоляции зависимостей. В Python есть два отдельных
инструмента для этих шагов -- [Pip](http://www.pip-installer.org/en/latest/) для объявления и
[Virtualenv](http://www.virtualenv.org/en/latest/) для изоляции. Даже в C есть
[Autoconf](http://www.gnu.org/s/autoconf/) для декларации, а статическая линковка (static linking)
обеспечивает изоляцию зависимостей. Независимо от инструментария, объявление зависимостей и их изоляция
должны использовать вместе -- только одно или другое не достаточно, чтобы удовлетворить двенадцати
факторам.

One benefit of explicit dependency declaration is that it simplifies setup for developers new to the app.  The new developer can check out the app's codebase onto their development machine, requiring only the language runtime and dependency manager installed as prerequisites.  They will be able to set up everything needed to run the app's code with a deterministic *build command*.  For example, the build command for Ruby/Bundler is `bundle install`, while for Clojure/[Leiningen](https://github.com/technomancy/leiningen#readme) it is `lein deps`.

Twelve-factor apps also do not rely on the implicit existence of any system tools.  Examples include shelling out to ImageMagick or `curl`.  While these tools may exist on many or even most systems, there is no guarantee that they will exist on all systems where the app may run in the future, or whether the version found on a future system will be compatible with the app.  If the app needs to shell out to a system tool, that tool should be vendored into the app.
