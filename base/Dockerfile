FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive
ENV TZ Asia/Shanghai

RUN apt-get update && apt-get install -y \
    git \
    plantuml \
    python3-pip \
    python3-venv \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ARG BUILD_DATE
ARG VCS_REF
ARG VERSION

LABEL org.label-schema.build-date="${BUILD_DATE}"
LABEL org.label-schema.name="Docker Sphinx Latex cn Image"
LABEL org.label-schema.description="A Docker image for Sphinx and pdf, a documentation tool written in Python."
LABEL org.label-schema.vcs-ref="${VCS_REF}"
LABEL org.label-schema.vcs-url="https://github.com/biggates/docker-sphinx-latex-cn"
LABEL org.label-schema.vendor="biggates"
LABEL org.label-schema.version="${VERSION}"
LABEL org.label-schema.schema-version="1.0"

CMD ["/bin/bash"]