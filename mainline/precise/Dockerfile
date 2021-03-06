FROM ubuntu:precise

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.9.11-1~precise

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
	&& echo "deb http://nginx.org/packages/mainline/ubuntu/ precise nginx" >> /etc/apt/sources.list \
	&& echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
	&& wget -O- https://download.newrelic.com/548C16BF.gpg | apt-key add - \
	&& apt-get update \
	&& apt-get install -y ca-certificates nginx=${NGINX_VERSION} gettext-base newrelic-sysmond \
	&& rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

#New Relic Configuration
RUN nrsysmond-config --set license_key=YOUR_LICENSE_KEY \
	&& /etc/init.d/newrelic-sysmond start

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
