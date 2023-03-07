FROM biggates/docker-sphinx-latex-cn:latex-base

WORKDIR /root/

COPY requirements.txt ./

RUN python -m pip install --requirement requirements.txt --upgrade
