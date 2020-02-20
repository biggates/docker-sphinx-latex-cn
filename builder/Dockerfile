FROM biggates/docker-sphinx-latex-cn:base

RUN addgroup --gid 1000 python \
    && useradd -g python --uid 1000 python

WORKDIR /home/python

RUN mkdir doc && chown -R python: .

USER python
COPY requirements.txt ./
COPY Makefile ./

RUN pip install --requirement requirements.txt --upgrade

VOLUME /home/python/doc
VOLUME /home/python/build
