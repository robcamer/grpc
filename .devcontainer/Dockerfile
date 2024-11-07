FROM gradescope/auto-builds:ubuntu-20.04
LABEL maintainer="fsgeek@gatech.edu"
USER root
# Create non-root user
ARG USERNAME=giosusr
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

ENV DEBIAN_FRONTEND=noninteractive
COPY setup.sh /$USERNAME/setup.sh
WORKDIR /$USERNAME
RUN /bin/bash setup.sh
RUN rm /$USERNAME/setup.sh

RUN unset DEBIAN_FRONTEND

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME
CMD /bin/bash