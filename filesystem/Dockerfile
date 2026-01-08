# mcp-filesystem Docker Image
# Based on sandraschi/filesystem-mcp with HTTP transport

FROM python:3.11-alpine

# Install system dependencies and Python packages in a single layer
RUN apk add --no-cache \
    git \
    docker-cli \
    && \
    # Install Python dependencies
    pip install --no-cache-dir \
    fastmcp>=2.14.1 \
    pydantic>=2.5.0 \
    docker>=6.0.0 \
    gitpython>=3.1.0 \
    aiohttp>=3.9.0 \
    && \
    # Clone filesystem-mcp (shallow clone)
    git clone --depth 1 https://github.com/sandraschi/filesystem-mcp.git /app && \
    rm -rf /app/.git && \
    # Create workspace directory
    mkdir -p /workspace

WORKDIR /app

# Expose MCP server port
EXPOSE 8000

# Start the MCP server with HTTP transport
CMD ["python", "-m", "filesystem_mcp", "--host", "0.0.0.0", "--port", "8000"]
