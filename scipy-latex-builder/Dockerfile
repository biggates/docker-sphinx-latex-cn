FROM biggates/docker-sphinx-latex-cn:latex-builder

USER python

ADD requirements_scipy.txt /tmp/

RUN python -m pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

RUN python -m venv .venv \
    && . .venv/bin/activate \
    && python -m pip install --upgrade -r /tmp/requirements_scipy.txt


ENTRYPOINT ["docker-entrypoint.sh"]
