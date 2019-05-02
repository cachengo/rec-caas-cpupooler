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

FROM alpine:3.9
MAINTAINER Balazs Szekeres <balazs.szekeres@nokia.com>

ARG DEPENDENCY_MANAGER
ENV DEP_VERSION=$DEPENDENCY_MANAGER
ARG CPUPOOLER
ENV CPUPOOLER_VERSION=$CPUPOOLER
ENV GOPATH /build
ENV PATH /usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN apk update \
&&  apk upgrade \
&&  apk add --no-cache --virtual .build-deps build-base git mercurial go glide bash tar curl \
&&  mkdir -p $GOPATH/src/github.com/nokia/CPU-Pooler \
&&  curl -fsSL -o /usr/local/bin/dep https://github.com/golang/dep/releases/download/v${DEP_VERSION}/dep-linux-amd64 \
&&  chmod +x /usr/local/bin/dep \
&&  curl -fsSL -k https://github.com/nokia/CPU-Pooler/archive/v${CPUPOOLER_VERSION}.tar.gz | tar zx --strip-components=1 -C $GOPATH/src/github.com/nokia/CPU-Pooler \
&&  cd $GOPATH/src/github.com/nokia/CPU-Pooler \
&&  dep ensure --vendor-only \
# cpu-device-plugin
&&  CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o cpu-device-plugin ./cmd/cpu-device-plugin \
&&  mv $GOPATH/src/github.com/nokia/CPU-Pooler/cpu-device-plugin /cpu-device-plugin \
# cpu-setter
&&  CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o cpu-setter ./cmd/cpusetter \
&&  mv $GOPATH/src/github.com/nokia/CPU-Pooler/cpu-setter /cpu-setter \
# cpu-webhook
&&  CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o cpu-device-webhook ./cmd/webhook \
&&  mv $GOPATH/src/github.com/nokia/CPU-Pooler/cpu-device-webhook /cpu-device-webhook \
&&  apk del .build-deps \
&&  rm -rf $GOPATH \
&&  rm -rf /build \
&&  rm -rf /root/.glide

