FROM ghcr.io/opensafely-core/python:v2

# we are going to use an apt cache on the host, so disable the default debian
# docker clean up that deletes that cache on every apt install
RUN rm -f /etc/apt/apt.conf.d/docker-clean

# setup the ehrql extension. This can't be installed via pip because it
# requires Python >=3.11
RUN --mount=type=cache,target=/var/cache/apt \
    apt update && apt install -y curl unzip &&\
    curl -LJO https://github.com/opensafely-core/ehrql/archive/main.zip &&\
    unzip ehrql-main.zip
