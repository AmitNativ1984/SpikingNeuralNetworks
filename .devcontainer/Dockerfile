ARG BASE_IMAGE
FROM $BASE_IMAGE AS base

RUN DEBIAN_FRONTEND=noninteractive 

RUN echo ${DEBIAN_FRONTEND}
RUN apt-get update && \
    apt-get install -y \
    wget \
    python3-pip \
    python3-argcomplete \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Python packages
RUN pip3 install \
    argcomplete \
    spikingjelly \
    snntorch \
    bash-completion \
    && rm -rf /root/.cache/pip
       
# Create the user
# ===================
ARG USERNAME
ENV USERNAME=$USERNAME
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Check if "ubuntu" user exists, delete it if it does, then create the desired user
RUN if getent passwd ubuntu > /dev/null 2>&1; then \
        userdel -r ubuntu && \
        echo "Deleted existing ubuntu user"; \
    fi && \
    groupadd --gid $USER_GID $USERNAME && \
    useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME && \
    echo "Created new user $USERNAME"

# Add sudo support for the non-root user
RUN apt-get update && apt-get install -y sudo \
  && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
  && chmod 0440 /etc/sudoers.d/$USERNAME \
  && rm -rf /var/lib/apt/lists/*

# Set up autocompletion for user
RUN apt-get update && apt-get install -y git-core bash-completion \
  && rm -rf /var/lib/apt/lists/*

USER $USERNAME
