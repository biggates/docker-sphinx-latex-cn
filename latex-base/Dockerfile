FROM biggates/docker-sphinx-latex-cn:base

# RUN sed -i -E 's/(deb|security).debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

RUN apt-get update && apt-get install --no-install-recommends -y \
    texlive \
    texlive-latex-extra \
    texlive-latex-recommended \
    texlive-xetex \
    texlive-science \
    latexmk \
    texlive-lang-chinese \
    fonts-freefont-otf \
    fonts-noto-cjk \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
