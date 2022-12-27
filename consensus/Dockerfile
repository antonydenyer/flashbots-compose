FROM golang:bullseye

RUN apt-get update && \
    apt-get install -y \
    curl xz-utils \
    gcc g++ mingw-w64 \
    gcc-aarch64-linux-gnu g++-aarch64-linux-gnu \
    cmake libssl-dev libxml2-dev vim apt-transport-https \
    zip unzip libtinfo5 patch zlib1g-dev autoconf libtool \
    pkg-config make docker.io gnupg2 libgmp-dev python

RUN go install github.com/bazelbuild/bazelisk@latest

RUN mkdir -p /usr/local/bin
RUN cp /go/bin/bazelisk /usr/local/bin/bazelisk
RUN ln -s /go/bin/bazelisk /usr/local/bin/bazel

# This both verifies that bazel is in $PATH,
# and also caches the latest bazel release at the time of docker build.
RUN bazel version

RUN mkdir -p /workspace

WORKDIR /workspace

RUN git clone https://github.com/flashbots/prysm.git .

RUN bazel build //cmd/beacon-chain:beacon-chain --config=release

ENTRYPOINT ["bazel","run","//cmd/beacon-chain:beacon-chain","--config=release","--"]