FROM biggates/docker-sphinx-latex-cn:builder

USER python

RUN python -m venv .venv \
    && . .venv/bin/activate \
    && python -m pip install --upgrade \
        matplotlib \
        nbsphinx \
        numpy \
        pandas \
        pip \
        scikit-learn \
        scipy \
        setuptools \
        twine \
        wfdb \
        xlsxwriter \
        wheel


ENTRYPOINT ["docker-entrypoint.sh"]
