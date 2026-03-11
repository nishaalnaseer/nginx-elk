FROM nginxinc/nginx-unprivileged:1.28-alpine-perl


USER root

RUN apk add --no-cache shadow \
    && groupmod -g 6200 nginx \
    && usermod -u 6200 -g 6200 nginx \
    && chown -R 6200:6200 /var/cache/nginx /var/run /etc/nginx /var/log/nginx

USER nginx

CMD ["nginx", "-g", "daemon off;"]
 