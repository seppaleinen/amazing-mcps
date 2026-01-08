# mcp-helmfile Docker Image
# Combines the mcp-helmfile Python server with helm/helmfile/kubectl

FROM python:3.11-slim

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/

# Install Helm
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 && \
    chmod 700 get_helm.sh && \
    ./get_helm.sh && \
    rm get_helm.sh

# Install Helmfile
RUN curl -L -o /usr/local/bin/helmfile https://github.com/helmfile/helmfile/releases/download/v0.169.1/helmfile_0.169.1_linux_amd64 && \
    chmod +x /usr/local/bin/helmfile

# Install Helm Diff plugin
RUN helm plugin install https://github.com/databus23/helm-diff

# Install mcp-helmfile Python dependencies
RUN pip install --no-cache-dir fastmcp pydantic

# Clone and install mcp-helmfile
RUN git clone https://github.com/snowsky/mcp-helmfile.git /app
WORKDIR /app

# Expose MCP server port
EXPOSE 8000

# Start the MCP server with HTTP transport
# FastMCP supports --transport http for HTTP/SSE mode
CMD ["python", "-m", "fastmcp", "run", "server:mcp", "--transport", "http", "--host", "0.0.0.0", "--port", "8000"]
