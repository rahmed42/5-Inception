##Dockerfile for MariaDB##
#Version OS
FROM alpine:3.16

#Passing Env variables in container
ARG	DB_NAME \
	DB_USER \
	DB_PASSWORD \
	DB_ROOT_PASSWORD

#Meta Informations
LABEL maintainer="rahmed"

#Run installations
RUN	apk update && apk upgrade \
	&& apk add --no-cache mariadb mariadb-client 

#Configure WordPress Database
RUN mkdir /var/run/mysqld; \
    chmod 777 /var/run/mysqld
COPY requirements/mariadb/conf/* /etc/my.cnf.d/

RUN mysql_install_db --user=mysql --datadir=/var/lib/mysql

#Listen Ports on runtime
EXPOSE 3306

#Exec at launch
COPY requirements/mariadb/tools/create_db.sh .
RUN sh create_db.sh && rm create_db.sh

USER mysql
CMD ["/usr/bin/mysqld", "--skip-log-error"]
