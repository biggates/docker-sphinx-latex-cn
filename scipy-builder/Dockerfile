FROM biggates/docker-sphinx-latex-cn:builder

USER python

RUN python -m venv .venv \
    && . .venv/bin/activate \
    && python -m pip install --upgrade \
        matplotlib \
        nbsphinx \
        numpy \
        pandas \
        pandoc \
        pip \
        scikit-learn \
        scipy \
        setuptools \
        twine \
        wfdb \
        wheel


ENTRYPOINT ["docker-entrypoint.sh"]
