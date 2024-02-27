FROM mcr.microsoft.com/devcontainers/python:3.12

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update -y \
    && DEBIAN_FRONTEND=noninteractive apt-get upgrade -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    git \
    pip \
    libzmq5 \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/cache/apt/* /var/lib/apt/lists/

RUN if ! getent group $GROUP_ID > /dev/null 2>&1; then groupadd -g $GROUP_ID mygroup; fi
RUN if ! getent passwd $USER_ID > /dev/null 2>&1; then useradd -m -u $USER_ID -g $GROUP_ID $USER; fi

RUN pip install --no-cache-dir jupyterlab

RUN curl -LO https://download.racket-lang.org/installers/8.12/racket-8.12-x86_64-linux-cs.sh \
    && chmod +x racket-8.12-x86_64-linux-cs.sh \
    && ./racket-8.12-x86_64-linux-cs.sh --in-place --create-dir --dest /usr/local/racket \
    && rm racket-8.12-x86_64-linux-cs.sh

ENV PATH="/usr/local/racket/bin:${PATH}"
RUN raco pkg install --auto --scope installation --batch iracket \
    && raco iracket install \
    && raco pkg install --auto --scope installation --batch racket-langserver

USER $USER_ID
WORKDIR /home/$USER_ID
RUN raco iracket install
