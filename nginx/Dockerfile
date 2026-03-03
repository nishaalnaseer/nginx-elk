FROM nginxinc/nginx-unprivileged:1.28-alpine-perl


USER root

WORKDIR  /home/nginx/

ARG FILEBEAT_VERSION=9.3.0
ARG FILEBEAT_URL=https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz
ARG FILEBEAT_HASH_URL=${FILEBEAT_URL}.sha512
ARG FILEBEAT_FILENAME=filebeat.tar.gz
ARG FILEBEAT_HASH_FILENAME=filebeat_hash.sha512

RUN apk add --no-cache shadow supervisor \
    && groupmod -g 6100 nginx \
    && usermod -u 6100 -g 6100 nginx \
    && chown -R 6100:6100 /var/cache/nginx /var/run /etc/nginx /var/log/nginx  \
    && curl -L -o ${FILEBEAT_FILENAME} ${FILEBEAT_URL} \
    && curl -L -o ${FILEBEAT_HASH_FILENAME}  ${FILEBEAT_HASH_URL}\
    && sed -i "s|filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz|${FILEBEAT_FILENAME}|" ${FILEBEAT_HASH_FILENAME} \
    && sha512sum -c ${FILEBEAT_HASH_FILENAME} \
    && tar -xzf ${FILEBEAT_FILENAME} \
    && rm ${FILEBEAT_HASH_FILENAME} \
    && rm ${FILEBEAT_FILENAME}\
    && mkdir filebeat \
    && cp -r filebeat-${FILEBEAT_VERSION}-linux-x86_64/* filebeat/. \
    && rm -rf filebeat-${FILEBEAT_VERSION}-linux-x86_64

# USER nginx

# CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
CMD ["sh"]
 