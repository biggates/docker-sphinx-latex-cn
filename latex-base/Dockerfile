FROM biggates/docker-sphinx-latex-cn:base

RUN apt-get update && apt-get install --no-install-recommends -y \
    texlive \
    texlive-latex-extra \
    texlive-latex-recommended \
    texlive-xetex \
    latexmk \
    texlive-lang-chinese \
    fonts-freefont-otf \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
