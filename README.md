## 主要内容

* 操作系统：
    * Ubuntu 18.04 LTS
    * 使用 mirrors.aliyun.com 作为加速源
    * git
    * texlive-xetex, latexmk
    * python3
    * msttcorefonts, fonts-freefont-otf
* Python:
    * recommonmark
    * sphinx_rtd_theme
    * sphinx-markdown-tables
    * sphinxcontrib-plantuml
    * sphinx-notfound-page

## 约定

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

## 使用

### 编译基础镜像

```bash
$ docker build -t docker-sphinx-build:local sphinx/docker
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
$ docker run -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build docker-sphinx-build:local make html
```

产物是整个 `build/html` 目录。

### 编译 pdf

```bash
$ docker run -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build docker-sphinx-build:local make latexpdf
```

产物在 `build/latex/` 。

## 参考

* https://github.com/keimlink/docker-sphinx-doc
