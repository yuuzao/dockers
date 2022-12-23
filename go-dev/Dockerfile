FROM debian:latest

LABEL name="yuuzao's dev environment"

USER root

ENV ANSIBLE_VERSION=5.9.0 \
  GO_VERSION=1.16.15 \
  GOLANGCI_LINT_VERSION=v1.45.2 \
  GOSS_VERSION=v0.3.16 \
  HELM_VERSION=3.6.2 \
  KUBECTL_VERSION=v1.21.3 \
  PACKER_VERSION=1.8.3 \
  PROTOBUF_VERSION=3.7.0 \
  PYJWT_VERSION=2.1.0 \
  PYWINRM_VERSION=0.4.3 \
  SHELLCHECK_VERSION=v0.8.0 \
  SHFMT_VERSION=3.4.2 \
  UPX_VERSION=3.96 \
  PATH=$PATH:/usr/local/go/bin:/go/bin:/usr/local/bin/docker \
  GOPATH=/go

RUN apt-get update && apt-get install -y apt-transport-https ca-certificates

SHELL ["/bin/bash", "-c"]

RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
        && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

RUN apt-get update
RUN \
  export DEBIAN_FRONTEND=noninteractive && \
  apt-get install -y --no-install-recommends \
  bash \
  build-essential \
  ca-certificates \
  curl \
  git-core \
  gcc \
  g++ \
  openssh-server \
  jq \
  libffi-dev \
  libc6 \
  libssl-dev \
  libunwind8 \
  man \
  make \
  mercurial \
  net-tools \
  netcat \
  openssh-client \
  procps \
  python3 \
  python3-dev \
  python3-pip \
  python3-setuptools \
  rsync \
  ruby \
  unzip \
  util-linux \
  vim \
  wamerican \
  wget \
  zip \
  && curl -L https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz | tar -C /usr/local -xz \
  && mkdir -p /go/bin \
  && export GOPROXY=https://goproxy.cn \
  && go install -v github.com/AlekSi/gocov-xml@latest \
  && go install -v github.com/axw/gocov/gocov@latest \
  && go install -v github.com/dgrijalva/jwt-go/cmd/jwt@latest \
  && go install -v github.com/go-bindata/go-bindata/go-bindata@v3.1.2 \
  && go install -v github.com/go-delve/delve/cmd/dlv@v1.9.0 \
  && go install -v github.com/golang/protobuf/protoc-gen-go@latest \
  && go install -v github.com/haya14busa/goverage@latest \
  && go install -v github.com/mitchellh/gox@latest \
  && go install -v github.com/onsi/ginkgo/ginkgo@v1.16.5 \
  && curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | sh -s -- -b ${GOPATH}/bin ${GOLANGCI_LINT_VERSION} \
  && curl -sSL https://github.com/koalaman/shellcheck/releases/download/${SHELLCHECK_VERSION}/shellcheck-${SHELLCHECK_VERSION}.linux.x86_64.tar.xz \
  | tar -vxJ -C /usr/local/bin --strip=1 \
  && curl -sSL https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_linux_amd64.zip -o /tmp/packer.zip \
  && unzip /tmp/packer.zip -d /usr/local/bin \
  && curl -o /usr/local/bin/shfmt -sSL https://github.com/mvdan/sh/releases/download/v{SHFMT_VERSION}/shfmt_v{SHFMT_VERSION}_linux_amd64 \
  && chmod +x /usr/local/bin/shfmt \
  && curl -L "https://github.com/aelsabbahy/goss/releases/download/${GOSS_VERSION}/goss-linux-amd64" -o /usr/local/bin/goss \
  && chmod +rx /usr/local/bin/goss \
  && curl -L "https://github.com/aelsabbahy/goss/releases/download/${GOSS_VERSION}/dgoss" -o /usr/local/bin/dgoss \
  && chmod +rx /usr/local/bin/dgoss \
  && curl -sSL -o /tmp/upx.tar.xz https://github.com/upx/upx/releases/download/v${UPX_VERSION}/upx-${UPX_VERSION}-amd64_linux.tar.xz \
  && tar -xvf /tmp/upx.tar.xz -C /tmp \
  && mv /tmp/upx-${UPX_VERSION}-amd64_linux/upx /usr/local/bin/upx \
  && pip3 install --disable-pip-version-check --no-cache-dir --upgrade pip \
  && pip3 install --disable-pip-version-check --no-cache-dir --upgrade ansible==${ANSIBLE_VERSION} ansible-core==2.12.6 --force-reinstall \
  && ansible-galaxy collection install ansible.windows:==1.10.0 \
  && apt-get purge --auto-remove -y libffi-dev python3-dev \
  && apt-get autoremove -y \
  && apt-get clean -y \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/man /usr/share/doc ${GOPATH}/pkg/* ${GOPATH}/src/* /root/cache /root/.cache \
  && go clean -cache -testcache -modcache
