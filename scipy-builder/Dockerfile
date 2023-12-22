FROM biggates/docker-sphinx-latex-cn:builder

USER python

ADD requirements_scipy.txt /tmp/

# g set global.index-url https://mirrors.aliyun.com/pypi/simple/

RUN python -m venv /home/python/.venv/ \
    && . /home/python/.venv/bin/activate \
    && python -m pip install --upgrade -r /tmp/requirements_scipy.txt


ENTRYPOINT ["docker-entrypoint.sh"]
