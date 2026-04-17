FROM mcr.microsoft.com/devcontainers/base:ubuntu

ARG USERNAME=vscode
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    vim \
    ca-certificates \
    wget \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install GitHub CLI from the official GitHub apt repository.
RUN mkdir -p -m 755 /etc/apt/keyrings \
    && out=$(mktemp) && wget -nv -O"$out" https://cli.github.com/packages/githubcli-archive-keyring.gpg \
    && cat "$out" > /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && rm -f "$out" \
    && mkdir -p -m 755 /etc/apt/sources.list.d \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" > /etc/apt/sources.list.d/github-cli.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends gh \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js 24+
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs \
    && npm install -g pnpm \
    && rm -rf /var/lib/apt/lists/*

RUN git config --global core.editor vim \
    && git config --global safe.directory /workspaces

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
