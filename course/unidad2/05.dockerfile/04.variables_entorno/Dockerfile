FROM mariadb:10.1.22

ENV MYSQL_ROOT_PASSWORD admin123
ENV MYSQL_DATABASE curso

ADD  ./loadData.sql /docker-entrypoint-initdb.d/loadData.sql
