FROM debian:latest

LABEL name="whl's dev environment" \
        summary="whl's dev environment" \
        description="whl's dev environment"

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
  zip

  RUN curl -L https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz | tar -C /usr/local -xz \
  && mkdir -p /go/bin \
  && go get -v github.com/AlekSi/gocov-xml@latest \
  && go get -v github.com/axw/gocov/gocov@latest \
  && go get -v github.com/dgrijalva/jwt-go/cmd/jwt@latest \
  && go get -v github.com/go-bindata/go-bindata/go-bindata@v3.1.2  \
  && go get -v github.com/go-delve/delve/cmd/dlv@v1.9.0  \
  && go get -v github.com/golang/protobuf/protoc-gen-go@latest \
  && go get -v github.com/haya14busa/goverage@latest \  
  && go get -v github.com/mitchellh/gox@latest \
  && go get -v github.com/onsi/ginkgo/ginkgo@v1.16.5 \
  RUN curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | sh -s -- -b ${GOPATH}/bin ${GOLANGCI_LINT_VERSION}
  RUN curl -sSL https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_linux_amd64.zip -o /tmp/packer.zip
  RUN unzip /tmp/packer.zip -d /usr/local/bin
  RUN curl -o /usr/local/bin/shfmt -sSL https://github.com/mvdan/sh/releases/download/v{SHFMT_VERSION}/shfmt_v{SHFMT_VERSION}_linux_amd64
  RUN chmod +x /usr/local/bin/shfmt
  RUN curl -L "https://github.com/aelsabbahy/goss/releases/download/${GOSS_VERSION}/goss-linux-amd64" -o /usr/local/bin/goss
  RUN chmod +rx /usr/local/bin/goss
  RUN curl -L "https://github.com/aelsabbahy/goss/releases/download/${GOSS_VERSION}/dgoss" -o /usr/local/bin/dgoss
  RUN chmod +rx /usr/local/bin/dgoss

  RUN pip install --disable-pip-version-check --no-cache-dir --upgrade pip
  RUN pip install --disable-pip-version-check --no-cache-dir --upgrade ansible==${ANSIBLE_VERSION} ansible-core==2.12.6 --force-reinstall
  RUN ansible-galaxy collection install ansible.windows:==1.10.0
  RUN apt-get purge --auto-remove -y libffi-dev python3-dev
  RUN apt-get autoremove -y
  RUN apt-get clean -y
  RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/man /usr/share/doc ${GOPATH}/pkg/* ${GOPATH}/src/* /root/cache /root/.cache
  RUN go clean -cache -testcache -modcache
