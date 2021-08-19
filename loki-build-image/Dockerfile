FROM golang:1.16.6-buster
RUN apt-get update && \
    apt-get install -qy \
    musl gnupg ragel \
    file zip unzip jq gettext\
    protobuf-compiler libprotobuf-dev \
    libsystemd-dev && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install some necessary dependencies.
# Forcing GO111MODULE=on is required to specify dependencies at specific versions using the go mod notation.
# If we don't force this, Go is going to default to GOPATH mode as we do not have an active project or go.mod
# file for it to detect and switch to Go Modules automatically.
# It's possible this can be revisited in newer versions of Go if the behavior around GOPATH vs GO111MODULES changes
RUN GO111MODULE=on go get \
    github.com/golang/protobuf/protoc-gen-go@v1.3.0 \
    github.com/gogo/protobuf/protoc-gen-gogoslick@v1.2.1 \
    github.com/gogo/protobuf/gogoproto@v1.2.1 \
    github.com/go-delve/delve/cmd/dlv@v1.3.2 \
    # Due to the lack of a proper release tag, we use the commit hash of
    # https://github.com/golang/tools/releases v0.1.7
    golang.org/x/tools/cmd/goyacc@58d531046acdc757f177387bc1725bfa79895d69 \
    github.com/mitchellh/gox && \
    rm -rf /go/pkg /go/src
ENV GOCACHE=/go/cache

COPY build.sh /
RUN chmod +x /build.sh
ENTRYPOINT ["/build.sh"]
