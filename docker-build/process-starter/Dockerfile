# Copyright 2019 Nokia
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM centos:7
MAINTAINER Balazs Szekeres <balazs.szekeres@nokia.com>

ARG DEPENDENCY_MANAGER
ENV DEP_VERSION=$DEPENDENCY_MANAGER
ARG CPUPOOLER
ENV CPUPOOLER_VERSION=$CPUPOOLER
ENV GOPATH /build
ENV BUILD_DIR="$GOPATH/src/github.com/nokia/CPU-Pooler"
ENV PATH=$PATH:/usr/local/go/bin

RUN yum update -y \
&&  yum install -y \
      go \
      git \
      bash \
      glide \
      libtool \
      autoconf \
      coreutils \
      curl \
      binutils \
      tar \
      grep \
      make \
      gcc \
      gcc-c++ \
&&  curl https://dl.google.com/go/go1.12.7.linux-arm64.tar.gz | tar -C /usr/local -xvz \
&&  curl -fsSL -o /usr/local/bin/dep https://github.com/golang/dep/releases/download/v${DEP_VERSION}/dep-linux-arm64 \
&&  chmod +x /usr/local/bin/dep \
&&  mkdir -p $BUILD_DIR \
&&  git clone https://github.com/nokia/CPU-Pooler.git $BUILD_DIR \
&&  cd $BUILD_DIR \
&&  git fetch --tags \
&&  git checkout ${CPUPOOLER_VERSION} \
&&  dep ensure --vendor-only \
# process-starter
&&  CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o process-starter ./cmd/process-starter \
&&  mv $BUILD_DIR/process-starter /process-starter \
&&  rm -rf $GOPATH
