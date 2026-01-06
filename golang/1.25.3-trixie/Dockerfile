FROM --platform=$BUILDPLATFORM golang:1.25.3-trixie AS vscode

ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG TARGETOS
ARG TARGETARCH
ENV LESSCHARSET utf-8

WORKDIR /workspace

RUN  <<EOF
apt-get update && apt-get install -y vim git git-secrets unzip less bash-completion npm procps
echo "source /usr/share/bash-completion/completions/git" >> ~/.bashrc
echo export PATH="$PATH:$(go env GOPATH)/bin" >> ~/.bashrc
go install -v golang.org/x/tools/gopls@latest
go install -v github.com/go-delve/delve/cmd/dlv@latest
go install -v github.com/cweill/gotests/gotests@latest
go install -v github.com/goreleaser/goreleaser/v2@latest
curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin latest

# Install GitHub CLI
(type -p wget >/dev/null || (apt update && apt install wget -y)) \
	&& mkdir -p -m 755 /etc/apt/keyrings \
	&& out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
	&& cat $out | tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
	&& chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
	&& mkdir -p -m 755 /etc/apt/sources.list.d \
	&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
	&& apt update \
	&& apt install gh -y

npm i -g @anthropic-ai/claude-code
EOF
