# ======================================================================
#  Dockerfile
#  Dev Container Dockerfile using asdf and Docker-in-Docker.
# ======================================================================
FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu-22.04

ARG ASDF_VERSION="v0.14.1"

# Install dependencies for asdf and Docker
RUN apt-get update && apt-get install -y \
    curl \
    git \
    build-essential \
    libffi-dev \
    libssl-dev \
    libbz2-dev \
    liblzma-dev \
    libsqlite3-dev \
    libreadline-dev \
    libncursesw5-dev \
    tk-dev \
    zlib1g-dev \
    xz-utils \
    libgdbm-dev \
    uuid-dev \
    ca-certificates \
    lsb-release \
    gnupg \
    && rm -rf /var/lib/apt/lists/*

# Install asdf
ENV ASDF_DIR=/usr/local/asdf
ENV ASDF_DATA_DIR=/workspace/.asdf
RUN git clone https://github.com/asdf-vm/asdf.git ${ASDF_DIR} --branch ${ASDF_VERSION}
ENV PATH="${ASDF_DIR}/bin:${ASDF_DATA_DIR}/shims:${PATH}"

# Copy .tool-versions and install specified tool versions
WORKDIR /workspace
COPY .tool-versions /workspace/.tool-versions

# Add asdf plugins for Node.js, pnpm, python, and poetry
RUN asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git && \
    asdf plugin add pnpm https://github.com/jonathanmorley/asdf-pnpm.git && \
    asdf plugin add python https://github.com/asdf-community/asdf-python.git && \
    asdf plugin add poetry https://github.com/asdf-community/asdf-poetry.git && \
    asdf install && \
    asdf reshim

# Create marker files for installed versions
RUN mkdir -p /workspace/.cache/versions && \
    printf "$(asdf where nodejs)\n" > /workspace/.cache/versions/nodejs-version && \
    printf "$(asdf where pnpm)\n" > /workspace/.cache/versions/pnpm-version && \
    printf "$(asdf where python)\n" > /workspace/.cache/versions/python-version && \
    printf "$(asdf where poetry)\n" > /workspace/.cache/versions/poetry-version

# Default command
CMD ["bash"]
