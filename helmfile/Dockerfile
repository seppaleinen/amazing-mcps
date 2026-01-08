# mcp-helmfile Docker Image
# Combines the mcp-helmfile Python server with helm/helmfile/kubectl

FROM python:3.11-alpine

# Install system dependencies and DevOps tools in a single layer
RUN apk add --no-cache \
    curl \
    bash \
    git \
    && \
    # Install kubectl
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/ && \
    # Install Helm
    curl -fsSL https://get.helm.sh/helm-v3.14.0-linux-amd64.tar.gz | tar xz && \
    mv linux-amd64/helm /usr/local/bin/ && \
    rm -rf linux-amd64 && \
    # Install Helmfile
    curl -L -o /usr/local/bin/helmfile https://github.com/helmfile/helmfile/releases/download/v0.169.1/helmfile_0.169.1_linux_amd64 && \
    chmod +x /usr/local/bin/helmfile && \
    # Install Helm Diff plugin
    helm plugin install https://github.com/databus23/helm-diff && \
    # Install Python dependencies
    pip install --no-cache-dir fastmcp pydantic && \
    # Clone mcp-helmfile (shallow clone)
    git clone --depth 1 https://github.com/snowsky/mcp-helmfile.git /app && \
    rm -rf /app/.git

WORKDIR /app

# Expose MCP server port
EXPOSE 8000

# Start the MCP server with HTTP transport
CMD ["python", "-m", "fastmcp", "run", "server:mcp", "--transport", "http", "--host", "0.0.0.0", "--port", "8000"]
