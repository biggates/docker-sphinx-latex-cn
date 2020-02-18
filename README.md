## 主要内容

### base

* 操作系统：
    * Ubuntu 18.04 LTS
    * 使用 mirrors.aliyun.com 作为加速源
    * git
    * python3, pip3
    * plantuml
* pip:
    * 使用 mirrors.aliyun.com 作为加速源
    * sphinx 2.4.1
    * recommonmark
    * sphinx_rtd_theme
    * sphinx-markdown-tables
    * sphinxcontrib-plantuml
    * sphinx-notfound-page

### latex

* 操作系统：
    * texlive-xetex, texlive-lang-chinese, latexmk
    * fonts-freefont-otf

## 手动编译镜像

```bash
$ docker build -t docker-sphinx-latex-cn:base -f base/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:builder -f builder/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:latex-base -f latex-base/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:latex-builder -f latex-builder/Dockerfile .
```

## 使用

### 目录约定

在主机中，基于 git 管理一个目录（含有 `sphinx/source/conf.py`），将其挂载到 `/home/python/doc` ；另外将 `build` 目录挂载到 `/home/python/build` 。

具体的目录结构为：

```
build/
sphinx/
├─source/
|   ├─conf.py
|   ├─index.rst
|   ├─_static/
|   └─_templates/
├─.git/
├─make.bat
└─Makefile
```

### 初始化工作空间

```bash
$ chmod -R +rw build
```

### 清理编译目录

```bash
$ rm -rf build/**
```

### 编译 html

```bash
$ docker run --rm -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build docker-sphinx-latex-cn:builder make html
```

产物是整个 `build/html` 目录。

### 编译 pdf

```bash
$ docker run --rm -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build docker-sphinx-latex-cn:latex-builder make latexpdf
```

产物在 `build/latex/` 。

## 参考

* https://github.com/keimlink/docker-sphinx-doc
