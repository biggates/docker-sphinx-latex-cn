FROM biggates/docker-sphinx-latex-cn:latex-base

RUN addgroup --gid 1000 python \
    && useradd -g python --uid 1000 python

WORKDIR /home/python

COPY bin/docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

RUN mkdir doc && chown -R python: .

USER python
COPY requirements.txt ./
COPY Makefile ./

# RUN python -m pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

RUN python -m venv /home/python/.venv/ \
    && . /home/python/.venv/bin/activate \
    && python -m pip install --requirement requirements.txt

VOLUME /home/python/build

WORKDIR /home/python/doc

ENTRYPOINT ["docker-entrypoint.sh"]
