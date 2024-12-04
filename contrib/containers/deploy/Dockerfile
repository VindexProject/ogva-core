FROM phusion/baseimage:focal-1.0.0
LABEL maintainer="Ogva Developers <dev@ogva.org>"
LABEL description="Dockerised OgvaCore, built from Travis"

ARG USER_ID
ARG GROUP_ID

ENV HOME /home/ogva

# add user with specified (or default) user/group ids
ENV USER_ID ${USER_ID:-1000}
ENV GROUP_ID ${GROUP_ID:-1000}
RUN groupadd -g ${GROUP_ID} ogva && \
    useradd -u ${USER_ID} -g ogva -s /bin/bash -m -d /home/ogva ogva && \
    mkdir /home/ogva/.ogvacore && \
    chown ${USER_ID}:${GROUP_ID} -R /home/ogva

COPY bin/* /usr/local/bin/

RUN chmod a+x /usr/local/bin/*

USER ogva

VOLUME ["/home/ogva"]

EXPOSE 40102 40112 41102 41112

WORKDIR /home/ogva
