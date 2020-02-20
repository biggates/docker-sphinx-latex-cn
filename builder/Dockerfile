FROM biggates/docker-sphinx-latex-cn:base

RUN addgroup --gid 1000 python \
    && useradd -g python --uid 1000 python

COPY docker-entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/docker-entrypoint.sh

WORKDIR /home/python

RUN mkdir doc && chown python: doc

USER python
COPY requirements.txt ./
COPY Makefile ./

ENV PIP_DISABLE_PIP_VERSION_CHECK True
ENV PIP_NO_CACHE_DIR False
ENV PYTHONUNBUFFERED True

RUN python3 -m venv .venv \
    && . .venv/bin/activate \
    && pip3 install --requirement requirements.txt --upgrade

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

VOLUME /home/python/doc
VOLUME /home/python/build

ENTRYPOINT ["docker-entrypoint.sh"]
