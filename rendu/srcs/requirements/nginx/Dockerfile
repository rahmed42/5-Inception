##Dockerfile for NGINX /w TLS v1.2 and TLS V1.3##
#Version OS
FROM alpine:3.16

#Meta Informations
LABEL maintainer="rahmed"

#Run installations
RUN	apk update && apk upgrade \
	&& apk add --no-cache nginx

#Listen Port HTTPS 443 on runtime
EXPOSE 443

#Exec at launch
CMD ["nginx", "-g", "daemon off;"]
