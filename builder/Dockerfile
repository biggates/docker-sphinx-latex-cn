FROM biggates/docker-sphinx-latex-cn:base

RUN addgroup --gid 1000 python \
    && useradd -g python --uid 1000 python

WORKDIR /home/python

COPY bin/docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

RUN mkdir doc && chown -R python: .

USER python
COPY requirements.txt ./
COPY Makefile ./

RUN python -m venv .venv \
    && . .venv/bin/activate \
    && python -m pip install --requirement requirements.txt

VOLUME /home/python/doc
VOLUME /home/python/build

ENTRYPOINT ["docker-entrypoint.sh"]
