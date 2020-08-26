# Build Prometheus from source
FROM golang:1.14.2 as build
WORKDIR /
RUN git clone https://github.com/prometheus/pushgateway.git \
 && cd pushgateway \
 && make

# Configure Ubuntu-based Pushgateway image
# Adapted from https://github.com/prometheus/pushgateway/blob/master/Dockerfile
FROM ubuntu:bionic
COPY --from=build --chown=nobody:nogroup /pushgateway/pushgateway /bin/pushgateway
COPY --from=build                        /pushgateway/LICENSE     /LICENSE
COPY --from=build                        /pushgateway/NOTICE      /NOTICE

EXPOSE 9091
RUN mkdir -p /pushgateway && chown nobody:nogroup /pushgateway
WORKDIR /pushgateway

USER 65534

ENTRYPOINT [ "/bin/pushgateway" ]
