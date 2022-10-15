FROM httpd:2.4-alpine
RUN apk update; \
    apk upgrade;


COPY server.crt /etc/apache2/ssl/server.crt
COPY server.key /etc/apache2/ssl/server.key
COPY server.pem /etc/apache2/ssl/server.pem
COPY server.crt /usr/local/apache2/conf/server.crt
COPY server.key /usr/local/apache2/conf/server.key
COPY server.pem /usr/local/apache2/conf/server.pem


# Copy apache vhost file to proxy php requests to php-fpm container
COPY demo.apache.conf /usr/local/apache2/conf/demo.apache.conf
RUN echo "Include /usr/local/apache2/conf/demo.apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf

RUN sed -i \
        -e 's/^#\(Include .*httpd-ssl.conf\)/\1/' \
        -e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
        -e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
        /usr/local/apache2/conf/httpd.conf
# Enable Apache mod_rewrite
RUN apk add apache2-utils
#RUN apk add apache2
#RUN a2enmod ssl && systemctl restart apache2