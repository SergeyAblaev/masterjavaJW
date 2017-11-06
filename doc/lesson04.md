# Онлайн проекта  <a href="https://github.com/JavaWebinar/masterjava">Masterjava</a>.

## [Материалы занятия (скачать все патчи можно через Download папки patch)](https://drive.google.com/drive/u/0/folders/0B9Ye2auQ_NsFVDNRNHpGU2NmcEk) 

### Fix: переименовал модуль `import(export)` в `upload`
#### 4_0_rename_upload.patch


## ![hw](https://cloud.githubusercontent.com/assets/13649199/13672719/09593080-e6e7-11e5-81d1-5cb629c438ca.png) Разбор домашнего задания HW3

### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 1. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFOFFNYzB6VGdma2c">Структура многомодульного проекта</a>
#### 4_1_HW3_pom_structure.patch
> Вместо включения всех модулей в главный аггрегатор, сделал еще 2 аггрегатных модуля 2го уровня: `web` и `services`

**ВНИМАНИЕ! после патча**
  - проверьте правильность структуры проекта
  - сделайте `mvn clean install`
  - удалите пустые корневые каталоги `src` и `upload` (git не работает с каталогами)
  - переименуйте модули idea (`Shift+F6`), если отличаются от имени модуля Maven

![structure](https://user-images.githubusercontent.com/11200258/32416643-928c4e34-c25d-11e7-832d-0f72dc1f8760.png)

#### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) [Вопрос: как разбивать приложение на модули](https://drive.google.com/open?id=0B9Ye2auQ_NsFTm5FX1ZZeTFpYU0)
### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 2. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFMHNBcVZ3eHlqblk">Реализация модуля `upload`: Thymeleaf и Upload</a>
**ВНИМАНИЕ! перед накаткой патча создайте в папке `web` проекта каталог `\common-web\src`, иначе патч промахивается.**
#### 4_2_HW3_thymeleaf_upload.patch
- <a href="http://www.thymeleaf.org/">Thymeleaf</a>:  natural templates
   - <a href="http://www.thymeleaf.org/doc/articles/thymeleaf3migration.html">Thymeleaf 3 migration guide</a>
   - <a href="http://www.thymeleaf.org/doc/articles/layouts.html">th:include и th:replace</a>
   - <a href="http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#conditional-expressions">Tutorial</a>
   - <a href="http://www.thymeleaf.org/doc/articles/standardurlsyntax.html">URL Syntax</a>
   - <a href="http://www.concretepage.com/thymeleaf/java-thymeleaf-example-getting-started-with-thymeleaf">Java Thymeleaf Example</a>
- [Thread-safe TemplateEngine.process](http://stackoverflow.com/a/23007118/548473)

#### 4_3_HW3_upload_servlet3.patch
> - Реализовал парсинг через StAX+JAXB
> - Добавил проверку на отсутствие выбранного файла

- [File upload using Servlet 3.0 API](http://www.ewebtutorials.com/file-upload-using-servlet-3-0-api-in-java.html)

----------------
## ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 3. [Maven. Поиск и разрешение конфликтов зависимостей](https://drive.google.com/file/d/0B9Ye2auQ_NsFbFFpWWFzRWE3ekU)
- Maven options: -pl upload -am install, -h, -X
- <a href="https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html">Maven transitive dependencies</a>
- <a href="https://maven.apache.org/components/plugins/maven-project-info-reports-plugin/">Project Info Reports Plugin</a>
- <a href="http://stackoverflow.com/questions/28098566/maven-site-warning-the-repository-url-https-maven-repository-dev-java-net-no/40040093#40040093">Disable blacklisted repository warnings</a>
```
mvn dependency:tree
mvn project-info-reports:dependencies
```
#### 4_4_dependencies.patch

- <a href="https://habrahabr.ru/company/jugru/blog/191246/">Разрешение конфликтов в транзитивных зависимостях</a>
- <a href="https://ru.wikipedia.org/wiki/Dependency_hell">Dependency hell</a>
```
mvn project-info-reports:dependency-convergence
```
#### 4_5_fix_convergence.patch
#### 4_6_enforcer.patch
- <a href="http://maven.apache.org/enforcer/maven-enforcer-plugin/">Maven Enforcer Plugin</a>
```
mvn clean install
mvn -DincludeScope=runtime dependency:copy-dependencies
```
- <a href="https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/javaway/jar-hell.zip">jar-hell.zip</a>
```
java -jar jar-hell.jar .
```
- <a href="http://jhades.github.io/">jHades</a>, [Downloads WAR Duplicate Classes](http://jhades.github.io/downloads.html)
```
java -jar jhades-standalone-report.jar path/to/war/webapp.war
```
```
mvn dependency:analyze
```
- <a href="https://maven.apache.org/plugins/maven-dependency-plugin/analyze-mojo.html">Analyze used and declared; used and undeclared; unused and declared</a>

## ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 4. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFSTR0cTl4NjE1OEE">Подключаем логирование с общими настройкам</a>
#### 4_7_logging.patch
> - Перенес подключение `logback-test.xml` из `parent-web` в `parent` (он используется в JUnit тестах, которые могут быть в любом модуле)
> - Добавил в корень проекта `config_templates` с копией конфигурации. 
Общие файлы конфигурации заданы в maven parent как в `<masterjava.config>/apps/masterjava/config/</masterjava.config>`.
**Нужно у себя в корне диска создать этот каталог и положить в него содержимое `config_templates`**

- <a href="http://www.slf4j.org/legacy.html">Добавление зависимостей логирования</a>
- <a href="https://www.mkyong.com/logging/logback-xml-example">logback.xml Example</a>
- Справочник:
  - <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFaTdYUnpLNFFUeXM">Topjava: логирование.</a>
  - <a href="https://www.youtube.com/watch?v=j-i3NQiKbcc">Владимир Красильщик: Что надо знать о логировании прагматичному Java‑программисту</a>  
  - <a href="http://habrahabr.ru/post/113145/">Java Logging: история кошмара</a>
  - <a href="http://skipy.ru/useful/logging.html">Ведение лога приложения</a>
  - <a href="http://logging.apache.org/log4j/2.x/index.html">Log4j</a>, <a href="http://logback.qos.ch/">Logback</a>

## ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 5.<a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFelc3S1RTWEx6VjA">Библиотеки и фреймворки для работы с JDBC.</a>

### Выбор lightweight JDBC helper library
- <a href="http://stackoverflow.com/questions/7137929/lightweight-jdbc-helper-library-alternative-to-apache-commons-dbutils">Lightweight JDBC helper library</a>
- <a href="https://habrahabr.ru/company/luxoft/blog/280784/#ii5">Библиотеки для работа с JDBC</a>
- <a href="http://www.mybatis.org/mybatis-3/">MyBatis</a>, <a href="http://sivalabs.in/2012/10/mybatis-tutorial-part-2-crud-operations-using-annotations/">MyBatis tutorial (CRUD)</a>
- <a href="https://commons.apache.org/proper/commons-dbutils/">Commons DbUtils</a>, <a href="https://habrahabr.ru/post/183204/">доработка commons-dbutils</a>
- <a href="http://stackoverflow.com/a/6258793/548473">jDBI sample</a>
   - <a href="http://jdbi.org/">JDBI</a>
   - <a href="http://zetcode.com/db/jdbi/">JDBI tutorial</a> (<a href="http://www.dropwizard.io/0.7.1/docs/manual/jdbi.html">Dropwizard JDBI</a>)
   - [Combining Spring Boot and JDBI](https://www.sitepoint.com/combining-spring-boot-and-jdbi)

### Tomcat Class Loader. Memory Leeks
#### 4_8_context.patch
> Cохранил конфигурацию Tomcat `context.xml`, в котором конфигурируется `jdbc/masterjava`. **Ее надо будет положить в ${TOMCAT_HOME}/conf**

- <a href="https://tomcat.apache.org/tomcat-8.0-doc/class-loader-howto.html">Class Loader HOW-TO</a>
- Библиотеки vs Frameworks и Tomcat Common Lib. <a href="https://habrahabr.ru/post/222443/">Memory Leaks</a>. 
- <a href="https://www.youtube.com/watch?v=sSmQ6W-ovZE">Никита Сальников-Тарновский — Утечки памяти</a>

### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 6. <a href="https://drive.google.com/file/d/0B9Ye2auQ_NsFa1JVQmRhQVdYdzA">Модуль `persist`</a>
**ВНИМАНИЕ! перед накаткой патча создайте в корне проекта каталоги `persist\src\main` и `persist\src\test`, иначе патч промахивается.**
#### 4_9_persist.patch
- <a href="http://stackoverflow.com/a/2322214/548473">Postgresql enum advantages/disadvantages</a>
- <a href="http://stackoverflow.com/a/7834949/548473">ALTER Enum types since 9.1</a>
- <a href="https://gitlab.com/rbertoncelj/jdbi-entity-mapper">Simple EntityMapper with `@Column` support</a>

### Домашнее задание
- в модуле **upload** сохранить всех импортированных пользователей в базе (записи просто добавляются в таблицу `users`). Делать вставку группами (chunk) в [batch моде](http://jdbi.org/sql_object_api_batching/). Количестово пользователей в chunk принимать с UI как параметр
- сделать отображение первых 20 пользователей в модуле **webapp**
- добавить в таблицу `users` уникальный индекс на `email` и вставлять в базу только новых пользователей. Результат импорта: пользователи, уже присутствующие в базе ([POSTGRES: INSERT ON CONFLICT](https://habrahabr.ru/post/264281/))

#### Optional
- cделать сохранение пользователей чанками в несколько потоков (по мере чтения xml). Результат импорта: пользователи, уже присутствующие в базе и диапазоны (начальный-конечный `email`) в чанке,
если его обработка закончилась с ошибкой + причина отказа в импорте для каждого пользователя/диапазона. 
- дополнительный материал:
  - [Speed up insertion performance in PostgreSQL](http://stackoverflow.com/a/12207237/548473)
  - [COPY — копировать данные между файлом и таблицей](https://postgrespro.ru/docs/postgrespro/9.6/sql-copy)