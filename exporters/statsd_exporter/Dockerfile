FROM golang:1.13 as builder

RUN mkdir /workspace
WORKDIR /workspace

RUN statsd_version=$(curl --silent https://api.github.com/repos/prometheus/statsd_exporter/releases/latest | grep '"tag_name":' | sed -E 's#.*"([^"]+)".*#\1#') && \
    echo "Fetching and building statsd_exporter version ${statsd_version}" && \
    git clone --single-branch --branch ${statsd_version} --depth 1 https://github.com/prometheus/statsd_exporter.git && \
    cd statsd_exporter && \
    make common-build

FROM ubuntu:bionic

COPY --from=builder /workspace/statsd_exporter/statsd_exporter /bin/statsd_exporter

USER        nobody
EXPOSE      9102 9125 9125/udp
HEALTHCHECK CMD wget --spider -S "http://localhost:9102/metrics" -T 60 2>&1 || exit 1
ENTRYPOINT  [ "/bin/statsd_exporter" ]
