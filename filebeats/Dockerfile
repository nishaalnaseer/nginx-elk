FROM debian:trixie-slim

WORKDIR /home/nginx

ARG FILEBEAT_VERSION=9.3.0
ARG FILEBEAT_PLATFORM=filebeat-${FILEBEAT_VERSION}-amd64.deb
ARG FILEBEAT_URL=https://artifacts.elastic.co/downloads/beats/filebeat/${FILEBEAT_PLATFORM}
ARG FILEBEAT_HASH_URL=${FILEBEAT_URL}.sha512
ARG FILEBEAT_FILENAME=filebeat.deb
ARG FILEBEAT_HASH_FILENAME=filebeat_hash.sha512

RUN apt-get update && apt-get install adduser curl -y \
    && adduser nginx --uid 6200 --disabled-login \
    && mkdir filebeat \
    && cd filebeat \
    && curl -L -o ${FILEBEAT_FILENAME} ${FILEBEAT_URL} \
    && curl -L -o ${FILEBEAT_HASH_FILENAME}  ${FILEBEAT_HASH_URL} \
    && sed -i "s|${FILEBEAT_PLATFORM}|${FILEBEAT_FILENAME}|" ${FILEBEAT_HASH_FILENAME} \
    && sha512sum -c ${FILEBEAT_HASH_FILENAME} \
    && apt-get purge curl adduser -y \
    && apt-get autoremove --purge -y \
    && dpkg -i ${FILEBEAT_FILENAME} \
    && cd .. \
    && rm -rf filebeat 

RUN mkdir /var/lib/filebeat && chown -R 6200:6200 /var/lib/filebeat

USER nginx

CMD ["filebeat", "-e", "-c", "/etc/filebeat/filebeat.yml"]