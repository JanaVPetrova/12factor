Введение
============

В современном мире программное обеспечение обычно предоставлется как сервис и называется *web-приложение* или
*software-as-a-service*. Приложение двенадцати факторов - это методология построения приложений "software-as-a-service", которые:

* Используют декларативный формат для автоматизации конфигурации, чтобы сократить время и стоимость внедрения в проект новых
разработчиков;
<!-- FIXME
  * Have a **clean contract** with the underlying operating system, offering **maximum portability** between execution environments;
-->
* Можно деплоить на современные **облачные платформы**, таким образом устраняя потребность в серверах и системном администрировании;

* **Minimize divergence** between development and production, enabling **continuous deployment** for maximum agility;
* And can **scale up** without significant changes to tooling, architecture, or development practices.

The twelve-factor methodology can be applied to apps written in any programming language, and which use any combination of backing services (database, queue, memory cache, etc).
