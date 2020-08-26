
# Build node_exporter from source
FROM golang:1.14.2 as build
WORKDIR /
ARG VERSION
RUN test -n "$VERSION"
RUN git clone --single-branch --branch $VERSION --depth 1 https://github.com/prometheus/node_exporter.git \
 && cd node_exporter \
 && make build

# Configure Ubuntu-based node_exporter image
# Adapted from https://github.com/prometheus/node_exporter/blob/master/Dockerfile
FROM ubuntu:bionic
COPY --from=build /node_exporter/node_exporter /bin/node_exporter
COPY --from=build /node_exporter/LICENSE       /LICENSE
COPY --from=build /node_exporter/NOTICE        /NOTICE

EXPOSE      9100
USER        nobody
ENTRYPOINT  [ "/bin/node_exporter" ]
