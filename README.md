# PostgreSQL
PostgreSQL по умолчанию создаёт схему public — все запросы будут проходить в её рамках.

Создадим схему в БД
CREATE SCHEMA IF NOT EXISTS content;

Но постоянно писать название схемы быстро надоест, поэтому можно изменить search_path, по которому PostgreSQL ищет таблицы. Например, можно получить текущий search_path, выполнив запрос:
SHOW search_path;

Добавим нашу схему в использование по умолчанию:
SET search_path TO content,public;

Но у такого подхода есть недостаток. Команда SET изменяет параметр search_path на время сессии с базой данных. При переподключении вам придётся вновь выполнить SET. Однако есть возможность навсегда привязать необходимый search_path к роли:
ALTER ROLE <необходимая_роль> SET search_path TO content,public; 

Чтобы не создавать БД вручную, создается ddl файл с командами.
Пример запуска БД с командами:
psql -h 127.0.0.1 -U app -d movies_database -f movies_database.ddl 

# Docker
### Запуск postgres
docker run -d \
  --name postgres \
  -p 5432:5432 \
  -v $HOME/postgresql/data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=123qwe \
  -e POSTGRES_USER=app \
  -e POSTGRES_DB=movies_database  \
  postgres:13 
  
Если в ответ вы получили id контейнера, значит, запуск прошёл успешно. Проверьте это, выполнив docker ps — эта команда позволяет увидеть все запущенные в данный момент контейнеры. А если набрать docker ps -a — вы увидите абсолютно все контейнеры, в том числе и остановленные.

Теперь вы можете подключиться к своему серверу Postgres. Это можно сделать двумя способами:
1) Установить postgresql локально и воспользоваться утилитой psql, которая идёт в комплекте — psql -h 127.0.0.1 -U app -d movies_database. Таким способом вы сможете подключиться к любому доступному в вашей сети серверу PostgreSQL. Для установки нужно выполнить sudo apt install postgresql.
2) Вызвать psql внутри запущенного postgres контейнера — docker exec -it postgres psql -h 127.0.0.1 -U app -d movies_database. Параметры -it соединят ваш терминал с терминалом контейнера и вы сможете вводить команды.

Чтобы запустить остановленный контейнер, необходимо ввести docker start и далее указать идентификатор или имя контейнера. 
docker start 2c88170e5391

# Flake8
### Как проверить, что flake8 доволен/не доволен
- pre-commit run flake8 --all-files

# Как пользоваться Git- [Как сделать новый коммит](./commmit_help.md)


# Полезные команды
- git commit --amend <commit message> — изменить сообщение последнего коммита.
- git stash — спрятать текущие изменения и откатить их. Удобно, если не хочется удалять изменения, но нужно 

### Откатить их на время ради переключения в другую ветку.
- git pull — получить изменения с сервера.
- git log --graph --oneline --decorate — отразить историю коммитов в виде графа.
### Фильтрация истории коммитов:
#### По дате:
- git log --after="2020-4-1"
- git log --after="yesterday"
- git log --after="1 week ago"
- git log --after="2020-4-1" --before="2014-4-6"
#### По автору:
- git log --author="Alex" — найти коммиты Алекса.
- git log --author="Alex\|Ivan" — найти коммиты Алекса и Ивана.
#### По другим параметрам:
- git log --grep="ISSUE-777:" — по паттерну сообщения.
- git log -- [foo.py](http://foo.py/) [bar.py](http://bar.py/) — по файлам, которые были изменены.
- git log -S "git" — поиск по содержимому файлов. Например, когда в файлы была добавлена строчка git.
- git log --no-merges — убрать из выборки мёрж-коммиты.
- git log --merges — выбрать только мёрж-коммиты.

### Введение в основы Git
https://git-scm.com/book/ru/v2/Введение-Основы-Git
### Создание алиасов команд
https://git-scm.com/book/ru/v2/Основы-Git-Псевдонимы-в-Git
### Как просматривать историю изменений на английском
https://www.atlassian.com/git/tutorials/git-log
### Один из подходов к созданию веток
https://www.atlassian.com/ru/git/tutorials/comparing-workflows/feature-branch-workflow
### git rebase для начинающих
https://habr.com/ru/post/337302/
