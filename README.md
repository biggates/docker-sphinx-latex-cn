## 主要内容

### base

* 操作系统
    * 基于 [nikolaik/python-nodejs:python3.8-nodejs12](https://hub.docker.com/layers/nikolaik/python-nodejs/python3.8-nodejs12/images/sha256-b0d1808fc94f41e02af1323eb41071aa484317170f9a09d7db4522d2df2c7ef5?context=explore)
        * Debian buster
        * git
        * python3, pip3
    * 再安装 plantuml
* pip:
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
$ docker run --rm -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build biggates/docker-sphinx-latex-cn:builder make html
```

产物是整个 `build/html` 目录。

### 编译 pdf

```bash
$ docker run --rm -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build biggates/docker-sphinx-latex-cn:latex-builder make latexpdf
```

产物在 `build/latex/` 。

### node 环境

在 Jenkins 中可以直接使用 `biggates/docker-sphinx-latex-cn:latex-builder` 作为 node 的 agent :

```
pipeline {
  agent {
    docker {
        image 'biggates/docker-sphinx-latex-cn:latex-builder'
    }
  }
}
```

## 参考

* https://github.com/keimlink/docker-sphinx-doc
