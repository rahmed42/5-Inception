##Dockerfile for WORDPRESS /w PHP-fhm ##
#Version OS
FROM alpine:3.16

#Passing Env variables in container
ARG	DB_NAME \
	DB_USER \
	DB_PASSWORD

#Meta Informations
LABEL maintainer="rahmed"

#Run installations
RUN	apk update && apk upgrade && apk add --no-cache \
	php8-fpm php8-mysqli unzip wget && \
	rm -f /var/cache/apk/*

#Install WordPress
WORKDIR /var/www
RUN wget https://wordpress.org/latest.zip && \
    unzip latest.zip && \
    cp -rf wordpress/* . && \
    rm -rf wordpress latest.zip

#Copy local files to container
COPY ./requirements/wordpress/conf/wp-config-create.sh .
RUN sh wp-config-create.sh && rm wp-config-create.sh && \
    chmod -R 0777 wp-content/ && mkdir -p wp-content/uploads/2023/01/
COPY ./requirements/wordpress/tools/42Nice.png wp-content/uploads/2023/01/.
COPY ./requirements/wordpress/conf/www.conf /etc/php8/php-fpm.d/.

#Exec at launch
CMD ["/usr/sbin/php-fpm8", "-F"]
