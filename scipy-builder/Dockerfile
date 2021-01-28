FROM biggates/docker-sphinx-latex-cn:builder

USER python

RUN python -m venv .venv \
    && . .venv/bin/activate \
    && python -m pip install --update \
        matplotlib \
        numpy \
        pandas \
        pip \
        scikit-learn \
        scipy \
        setuptools \
        twine \
        wfdb \
        wheel


ENTRYPOINT ["docker-entrypoint.sh"]
