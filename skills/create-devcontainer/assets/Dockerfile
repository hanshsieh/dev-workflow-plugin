FROM mcr.microsoft.com/devcontainers/base:ubuntu

ARG USERNAME=vscode
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    gh \
    vim \
    ca-certificates \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js 24+
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs \
    && npm install -g pnpm \
    && rm -rf /var/lib/apt/lists/*

RUN git config --global core.editor vim \
    && git config --global safe.directory /workspace

# "Understand-Anything" plugin helps you understand the codebase.
# "dev-workflow-plugin" plugin helps you with development workflow.
# "Understand-Anything"'s pre-requisites:
# - Install python3, nodejs
# - Symlink at "~/.understand-anything-plugin"
RUN mkdir -p /home/${USERNAME}/.cursor/plugins/local \
    && rm -rf /home/${USERNAME}/.cursor/plugins/local/Understand-Anything \
    && rm -rf /home/${USERNAME}/.cursor/plugins/local/dev-workflow-plugin \
    && git clone https://github.com/Lum1104/Understand-Anything.git /home/${USERNAME}/.cursor/plugins/local/Understand-Anything \
    && git clone https://github.com/hanshsieh/dev-workflow-plugin.git /home/${USERNAME}/.cursor/plugins/local/dev-workflow-plugin \
    && ln -sfn /home/${USERNAME}/.cursor/plugins/local/Understand-Anything/understand-anything-plugin /home/${USERNAME}/.understand-anything-plugin \
    && chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}/.cursor \
    && chown -h ${USERNAME}:${USERNAME} /home/${USERNAME}/.understand-anything-plugin
