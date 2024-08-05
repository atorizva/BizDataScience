#.devcontainer/Dockerfile

FROM nvidia/cuda:12.4.0-base-ubuntu22.04

LABEL maintainer="atoriz98@pm.me"

ARG USERNAME=dev
ARG UID=1000
ARG GID=${UID}

ENV SHELL=/bin/bash
SHELL ["/bin/bash", "-c"]

RUN groupadd --gid $GID $USERNAME \
  && useradd --system --uid ${GID} --gid ${UID} --create-home ${USERNAME} \
  && chown -R ${UID}:${GID} /home/${USERNAME}

ENV DEBIAN_FRONTEND=noninteractive 
ARG PYTHON_ENV=env
ENV PYTHON_ENV=$PYTHON_ENV

# Install system dependencies
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
      git \
      python3 python3-dev python3-pip python3-venv \
    && rm -rf /var/lib/apt/lists/* 

USER $USERNAME
WORKDIR /home/$USERNAME

COPY requirements.txt requirements.txt

# Install Python environment
RUN python3 -m venv $PYTHON_ENV && source $PYTHON_ENV/bin/activate \
    && pip install --upgrade pip && pip install -r requirements.txt \
    && pip install torch torchvision torchaudio --index-url \
    https://download.pytorch.org/whl/cu124

CMD [ "bin/bash", "-c", "source $PYTHON_ENV/bin/activate && exec bash"]