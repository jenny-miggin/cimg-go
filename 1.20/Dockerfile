# vim:set ft=dockerfile:

# Do not edit individual Dockerfiles manually. Instead, please make changes to the Dockerfile.template, which will be used by the build script to generate Dockerfiles.

# By policy, the base image tag should be a quarterly tag unless there's a 
# specific reason to use a different one. This means January, April, July, or 
# October.
FROM cimg/base:2023.04

LABEL maintainer="Community & Partner Engineering Team <community-partner@circleci.com>"

ENV GO_VER="1.20.7"

# Install packages needed for CGO
RUN sudo apt-get update && sudo apt-get install -y --no-install-recommends \
		g++ \
		libc6-dev && \
	sudo rm -rf /var/lib/apt/lists/* && \
	# Check if we're facing the first minor release issue and adjust if needed
	if [[ ${GO_VER:(-2)} == ".0" ]]; then \
	  GO_VER=${GO_VER:0:-2}; \
	fi && \
	[[ $(uname -m) == "x86_64" ]] && ARCH="amd64" || ARCH="arm64" && \
	curl -sSL "https://go.dev/dl/go${GO_VER}.linux-${ARCH}.tar.gz" | sudo tar -xz -C /usr/local/ && \
	mkdir -p /home/circleci/go/bin

# Install related tools
ENV GOTESTSUM_V=1.10.1
ENV GOCI_LINT_V=1.53.3
RUN [[ $(uname -m) == "x86_64" ]] && ARCH="amd64" || ARCH="arm64" && \
	curl -sSL "https://github.com/gotestyourself/gotestsum/releases/download/v${GOTESTSUM_V}/gotestsum_${GOTESTSUM_V}_linux_${ARCH}.tar.gz" | \
	sudo tar -xz -C /usr/local/bin gotestsum && \
	curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sudo sh -s -- -b /usr/local/bin v${GOCI_LINT_V}

ENV GOPATH /home/circleci/go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH
