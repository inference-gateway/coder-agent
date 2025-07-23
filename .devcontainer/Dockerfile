# Development environment for coder A2A Agent
FROM mcr.microsoft.com/devcontainers/go:1-1.24-bookworm

# Install additional tools
RUN apt-get update && apt-get install -y \
    curl \
    git \
    make \
    wget \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Install Task (Taskfile)
RUN curl -sL https://taskfile.dev/install.sh | sh

# Install A2A CLI
RUN curl -fsSL https://raw.githubusercontent.com/inference-gateway/a2a-cli/main/install.sh | bash

# Install Go tools for development
RUN go install -v golang.org/x/tools/gopls@latest \
    && go install -v github.com/go-delve/delve/cmd/dlv@latest \
    && go install -v honnef.co/go/tools/cmd/staticcheck@latest

# Use Powerlevel10k theme
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /home/vscode/.powerlevel10k

# Configure Powerlevel10k
RUN echo 'source /home/vscode/.powerlevel10k/powerlevel10k.zsh-theme' >> /home/vscode/.zshrc && \
    echo 'POWERLEVEL9K_DISABLE_CONFIGURATION_WIZARD=true' >> /home/vscode/.zshrc && \
    echo 'POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(dir vcs)' >> /home/vscode/.zshrc && \
    echo 'POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(command_execution_time status)' >> /home/vscode/.zshrc && \
    echo 'POWERLEVEL9K_COMMAND_EXECUTION_TIME_THRESHOLD=0' >> /home/vscode/.zshrc && \
    echo 'POWERLEVEL9K_COMMAND_EXECUTION_TIME_PRECISION=2' >> /home/vscode/.zshrc && \
    echo 'POWERLEVEL9K_COMMAND_EXECUTION_TIME_FORMAT="duration"' >> /home/vscode/.zshrc

# Set working directory
WORKDIR /workspace

# Create Go directories and set proper ownership
RUN mkdir -p /home/vscode/.cache/go-mod /home/vscode/.cache/go-build /home/vscode/go && \
    chown -R vscode:vscode /home/vscode/.cache /home/vscode/go

# Switch to vscode user
USER vscode

ENV GOPATH=/home/vscode/go
ENV GOMODCACHE=/home/vscode/.cache/go-mod
ENV GOCACHE=/home/vscode/.cache/go-build
ENV GOPROXY=https://proxy.golang.org,direct
ENV GOSUMDB=sum.golang.org
