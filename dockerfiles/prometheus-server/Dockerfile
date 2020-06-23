FROM golang:1.14.2 as build

ENV CLONE_DIR="/src/github.com/prometheus"
RUN mkdir -p ${CLONE_DIR} \
 && cd ${CLONE_DIR} \
 && git clone https://github.com/prometheus/prometheus.git \
 && cd prometheus \
 && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
 && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
 && apt update && apt install yarn -y \
 && make build

FROM ubuntu:bionic
COPY --from=build /src/github.com/prometheus/prometheus/ /workspace/prometheus/

# tsdb.path
# RUN mkdir -p /prometheus && \
#     chown -R nobody:nogroup etc/prometheus /prometheus
# VOLUME     [ "/prometheus" ]
# WORKDIR    /prometheus

# web.console
RUN mkdir -p /etc/prometheus
RUN ln -s /workspace/prometheus/console_libraries /workspace/prometheus/consoles/ /etc/prometheus/

EXPOSE 9090

ENTRYPOINT ["/workspace/prometheus/prometheus"]
CMD        [ "--config.file=/workspace/prometheus/documentation/examples/prometheus.yml", \
             # "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/workspace/prometheus/console_libraries", \
             "--web.console.templates=/workspace/prometheus/consoles" ]
