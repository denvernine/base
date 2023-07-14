# syntax=docker/dockerfile:1-labs
FROM ubuntu:latest

ARG UNAME
ARG UID
ARG GID=${UID}

RUN <<end_of_run
  apt-get update
  apt-get install -y --no-install-recommends \
    curl ca-certificates
  apt-get clean
  rm -rf /var/lib/apt/lists/*

  groupadd --gid ${GID} ${UNAME}
  useradd --non-unique --uid ${UID} --gid ${GID} --create-home ${UNAME} --shell /bin/bash
end_of_run

USER ${UNAME}

ENV HOME=/home/${UNAME}
ENV VOLTA_HOME=${HOME}/.volta
ENV PATH=${VOLTA_HOME}/bin:$PATH

RUN <<end_of_run
  curl -sSL https://get.volta.sh | bash

  volta install node@lts
end_of_run
