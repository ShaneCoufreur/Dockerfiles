FROM alpine:3.23.4

ADD https://github.com/just-containers/s6-overlay/releases/download/v3.2.3.0/s6-overlay-x86_64.tar.xz /tmp/

RUN tar -C / -Jxpf /tmp/s6-overlay-x86_64.tar.xz \
    && apk add  --no-cache --purge cgit git \
      openssh-server curl nginx fcgiwrap ca-certificates \
      py-pygments \
    && adduser \
      -u 10000 \
      -h /home/git \
      --disabled-password \
      -D \
      -s /usr/bin/git-shell \
      git \
    && passwd -u git \
    && ssh-keygen -A \
    && mkdir -p /run/nginx \
    && rm -rf /var/cache/apk/* /tmp/*

COPY nginx/nginx.conf /etc/nginx/nginx.conf
COPY sshd/sshd_config /etc/ssh/sshd_config
COPY cgit/cgitrc /etc/cgitrc

COPY nginx/run /etc/services.d/nginx/run
COPY sshd/run /etc/services.d/sshd/run
COPY fcgiwrap/run /etc/services.d/fcgiwrap/run

EXPOSE 443 80 22

CMD ["/init"]
