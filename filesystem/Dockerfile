# mcp-filesystem Docker Image
# Based on sandraschi/filesystem-mcp with HTTP transport

FROM python:3.11-slim

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    docker.io \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
RUN pip install --no-cache-dir \
    fastmcp>=2.14.1 \
    pydantic>=2.5.0 \
    docker>=6.0.0 \
    gitpython>=3.1.0 \
    aiohttp>=3.9.0

# Clone filesystem-mcp
RUN git clone https://github.com/sandraschi/filesystem-mcp.git /app
WORKDIR /app

# Create workspace directory
RUN mkdir -p /workspace

# Expose MCP server port
EXPOSE 8000

# Start the MCP server with HTTP transport
CMD ["python", "-m", "filesystem_mcp", "--host", "0.0.0.0", "--port", "8000"]
