# mcp-helmfile Docker Image
# Combines the mcp-helmfile Python server with helm/helmfile/kubectl

FROM python:3.11-alpine

# Define versions
ARG HELM_VERSION=3.14.0
ARG HELMFILE_VERSION=0.169.1

# Install system dependencies
RUN apk add --no-cache \
    curl \
    bash \
    git

# Detect architecture and install kubectl
RUN ARCH=$(uname -m) && \
    case ${ARCH} in \
    x86_64) KUBECTL_ARCH="amd64" ;; \
    aarch64) KUBECTL_ARCH="arm64" ;; \
    *) echo "Unsupported architecture: ${ARCH}" && exit 1 ;; \
    esac && \
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${KUBECTL_ARCH}/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/

# Install Helm (multi-arch)
RUN ARCH=$(uname -m) && \
    case ${ARCH} in \
    x86_64) HELM_ARCH="amd64" ;; \
    aarch64) HELM_ARCH="arm64" ;; \
    *) echo "Unsupported architecture: ${ARCH}" && exit 1 ;; \
    esac && \
    curl -fsSL "https://get.helm.sh/helm-v${HELM_VERSION}-linux-${HELM_ARCH}.tar.gz" | tar xz && \
    mv linux-${HELM_ARCH}/helm /usr/local/bin/ && \
    rm -rf linux-${HELM_ARCH}

# Install Helmfile (multi-arch)
RUN ARCH=$(uname -m) && \
    case ${ARCH} in \
    x86_64) HF_ARCH="amd64" ;; \
    aarch64) HF_ARCH="arm64" ;; \
    *) echo "Unsupported architecture: ${ARCH}" && exit 1 ;; \
    esac && \
    curl -L -o /usr/local/bin/helmfile \
    "https://github.com/helmfile/helmfile/releases/download/v${HELMFILE_VERSION}/helmfile_${HELMFILE_VERSION}_linux_${HF_ARCH}" && \
    chmod +x /usr/local/bin/helmfile

# Install Helm Diff plugin (use --version to get pre-built binary)
RUN helm plugin install https://github.com/databus23/helm-diff --version v3.9.4

# Install Python dependencies
RUN pip install --no-cache-dir fastmcp pydantic

# Clone mcp-helmfile (shallow clone)
RUN git clone --depth 1 https://github.com/snowsky/mcp-helmfile.git /app && \
    rm -rf /app/.git

WORKDIR /app

# Expose MCP server port
EXPOSE 8000

# Start the MCP server with HTTP transport
CMD ["python", "-m", "fastmcp", "run", "server:mcp", "--transport", "http", "--host", "0.0.0.0", "--port", "8000"]
