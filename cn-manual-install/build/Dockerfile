FROM nextcloud/aio-nextcloud:latest
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories
RUN apk update && \
    apk upgrade 
ENV PHP_MEMORY_LIMIT=512M
ENV PHP_UPLOAD_LIMIT=10G
ENV PHP_MAX_TIME=3600
ENV NEXTCLOUD_VERSION=26.0.4
ENV AIO_TOKEN=123456
ENV AIO_URL=localhost

COPY --chmod=775 *.sh /

VOLUME /mnt/ncdata
VOLUME /var/www/html
USER root
ENTRYPOINT ["/start.sh"]
CMD ["/usr/bin/supervisord", "-c", "/supervisord.conf"]
HEALTHCHECK CMD sudo -E -u www-data bash /healthcheck.sh
LABEL com.centurylinklabs.watchtower.enable="false"
