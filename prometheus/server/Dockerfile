# Build Prometheus from source
FROM golang:1.14.2 as build
WORKDIR /

ARG VERSION
RUN test -n "$VERSION"

RUN git clone --single-branch --branch $VERSION --depth 1 https://github.com/prometheus/prometheus.git \
 && cd prometheus \
 && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
 && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
 && apt update && apt install yarn bzip2 -y \
 && make npm_licenses build

# Configure Ubuntu-based Prometheus image
# Adapted from https://github.com/prometheus/prometheus/blob/master/Dockerfile
FROM ubuntu:bionic
COPY --from=build /prometheus/prometheus                             /bin/prometheus
COPY --from=build /prometheus/promtool                               /bin/promtool
COPY --from=build /prometheus/documentation/examples/prometheus.yml  /etc/prometheus/prometheus.yml
COPY --from=build /prometheus/console_libraries/                     /usr/share/prometheus/console_libraries/
COPY --from=build /prometheus/consoles/                              /usr/share/prometheus/consoles/
COPY --from=build /prometheus/LICENSE                                /LICENSE
COPY --from=build /prometheus/NOTICE                                 /NOTICE
COPY --from=build /prometheus/npm_licenses.tar.bz2                   /npm_licenses.tar.bz2

RUN ln -s /usr/share/prometheus/console_libraries /usr/share/prometheus/consoles/ /etc/prometheus/
RUN mkdir -p /prometheus && \
    chown -R nobody:nogroup etc/prometheus /prometheus

USER       nobody
EXPOSE     9090
VOLUME     [ "/prometheus" ]
WORKDIR    /prometheus
ENTRYPOINT [ "/bin/prometheus" ]
CMD        [ "--config.file=/etc/prometheus/prometheus.yml", \
             "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/usr/share/prometheus/console_libraries", \
             "--web.console.templates=/usr/share/prometheus/consoles" ]
