ARG BASE_IMG=alpine:3.16

FROM $BASE_IMG AS pidproxy

RUN apk --no-cache add alpine-sdk \
 && git clone https://github.com/ZentriaMC/pidproxy.git \
 && cd pidproxy \
 && git checkout 193e5080e3e9b733a59e25d8f7ec84aee374b9bb \
 && sed -i 's/-mtune=generic/-mtune=native/g' Makefile \
 && make \
 && mv pidproxy /usr/bin/pidproxy \
 && cd .. \
 && rm -rf pidproxy \
 && apk del alpine-sdk


FROM $BASE_IMG

COPY --from=pidproxy /usr/bin/pidproxy /usr/bin/pidproxy
RUN \
  apk update && \
  apk upgrade && \
  apk add \
  vsftpd \
  tini && \
  rm -rf /var/cache/apk/*

COPY --chmod=744 start_vsftpd.sh /bin/start_vsftpd.sh
COPY vsftpd.conf /etc/vsftpd/vsftpd.conf

ENTRYPOINT ["/sbin/tini", "--", "/bin/start_vsftpd.sh"]
