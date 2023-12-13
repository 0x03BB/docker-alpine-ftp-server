FROM alpine:latest AS upgrade

RUN \
  apk update && \
  apk upgrade


FROM upgrade AS pidproxy

RUN \
  apk add alpine-sdk && \
  git clone https://github.com/ZentriaMC/pidproxy.git && \
  cd pidproxy && \
  git checkout 193e5080e3e9b733a59e25d8f7ec84aee374b9bb && \
  sed -i 's/-mtune=generic/-mtune=native/g' Makefile && \
  make && \
  mv pidproxy /usr/bin/pidproxy && \
  cd .. && \
  rm -rf pidproxy && \
  apk del alpine-sdk && \
  rm -rf /var/cache/apk/*


FROM upgrade

COPY --from=pidproxy /usr/bin/pidproxy /usr/bin/pidproxy
RUN \
  apk add vsftpd && \
  rm -rf /var/cache/apk/*

COPY --chmod=744 start_vsftpd.sh /bin/start_vsftpd.sh
COPY vsftpd.conf /etc/vsftpd/vsftpd.conf

ENTRYPOINT ["/bin/start_vsftpd.sh"]
