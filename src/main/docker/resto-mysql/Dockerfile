FROM mysql:5.7

LABEL Author="Alex Panov", version=0.1

# Поднимаем консоль в контейнере
# ENTRYPOINT service mysql start && /bin/sh

# объединили несколько команд в одном слое. Если mysql стартуем в секции ENTRYPOINT, здесь это уже не нужно
# Эти команды не нужны, если мы юзаем группу команд [A]
#RUN # service mysql start && \
# mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "CREATE DATABASE resto" && \
# mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "CREATE USER 'restouser'@'%' identified by 'restopass';" && \
# mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "GRANT ALL ON resto.* TO 'restouser'@'%' IDENTIFIED BY 'restopass'; FLUSH PRIVILEGES"

# порт 3306 уже открыт в базовом образе mysql

# Можно указать пароль в скрипте сборки (ниже), а можно позволить пользователю указать пароль самому при сборке (в командной строке?)
# ENV MYSQL_ROOT_PASSWORD=root

# позволяет указать логин/пароль пользователя, которые будут созданы при старте контейнера и которым будут даны полномочия SUPERUSER по доступу к созданной базе
# ENV MYSQL_USER=restouser
# ENV MYSQL_PASSWORD=restopass

# Задать базу данных, которая будет создана при первом старте контейнера через перерменную окружения mysql
# ENV MYSQL_DATABASE=resto

# Либо так [A]:
ENV MYSQL_ROOT_PASSWORD=root \
       MYSQL_USER=restouser \
       MYSQL_PASSWORD=restopass1 \
       MYSQL_DATABASE=resto

# Скопировать init.sql в папку /docker-entrypoint-initdb.sh
# В соответсвии с https://hub.docker.com/_/mysql/ все файлы в этой папке будут выполнены
# [!] Файлы *.sql д.б. исключены из игнора в .dockerignore
#  Copy the database schema to the /data directory
# [!] Видимо, послк этого надо использовать команду RUN /docker-entrypoint-initdb.sh/init.sql
COPY ./dump/init.sql /docker-entrypoint-initdb.d/init.sql
# RUN mysqld --explicit_defaults_for_timestamp /docker-entrypoint-initdb.sh/init.sql
# [!] Правильно так:
# init_db.sh /tmp/init_db.sh
# RUN /tmp/init_db.sh
# Другой способ - подмонтировать папку с файлом init_db (sql или sh?) прямо в *.yml файле
# Третий способ - в *.yml файле использовать секцию command





# cd src/main/docker/sample
# Команда сборки образа (сначала перейти в папку, где лежит Dockerfile): docker build -t babagay/mysql .

# Команда запуска собранного контейнера:
# docker run -i -t -p 3306:3306 babagay/mysql
# или так: docker run -it babagay/mysql

# Вариант команды запуска контейнера с указанием его имени и пароля mysql, а также с маппингом портов
# docker run -it -e MYSQL_ROOT_PASSWORD=secret -p 3307:3306 --name=mysql_resto babagay/mysql
# docker run -it -e MYSQL_ROOT_PASSWORD=secret -p 3307:3306 --name=mysql_resto --memory=256M --cpus=1 babagay/mysql
# docker run -it -e MYSQL_ROOT_PASSWORD=secret -p 3306:3306 --name=mysql_resto --memory=256M --cpus=1 -v d:/restodata:/var/lib/mysql babagay/mysql
# docker run -it -e MYSQL_ROOT_PASSWORD=secret -p 3307:3306 -d --name=mysql_resto --memory=256M --cpus=1 -v ~/mysql_mnt:/var/lib/mysql babagay/mysql
# 3307 - это порт, на который надо стучатсья с локальной машины. А 3306 - порт внутри контейнера
# ключ -d позволяет запускать в режиме демона
# Команда входа в запущенный контейнер через консоль:
# docker exec -it mysql_resto /bin/sh
# mysql -urestouser -prestopass
# лимит памяти для контейнера 256М и одно ядро
# Ключ -v позволяет подмонтировать локальную папку к папке внутри образа
# docker rm -f mysql_resto - удалить контейнер mysql_resto
# docker ps -a - список контейнеров
# Запуск через композер:
# docker-compose -f mysql.yml up
# docker-compose -f resto-app.yml up
# Или docker-compose -f src/main/docker/app.yml up --build
# --build заставляет пересобирать образ





# Команда, чтоб посмотреть историю создания образа: docker history --no-trunc mysql

# docker stop mysql_resto

# docker start mysql_resto

# [issues]
# Файл grant.sql скопировался, но, видимо, не отработал, т.к. тестовая таблица не появилась

# [workflow]
# Я разрабатываю, используя базу, как обычный сервис винды, а , когда все готово, создаю докерфайл с командой типа
# COPY init.sql /docker-entrypoint-initdb.sh/
# В init.sql образ базы.
# Затем докер билд и пуш в докер репу.
# после чего на удаленном сервере просто: docker-compose up
