## 主要内容

### base

供 multi stage build 使用。

* 基于 [nikolaik/python-nodejs:python3.10-nodejs20](https://hub.docker.com/r/nikolaik/python-nodejs)
  * 主体是 Debian bullseye
  * git
  * python3, pip3
* plantuml (openjdk)
* graphviz/dot
* pandoc

### builder

用于通过 Sphinx 生成简单软件的 html 文档。

* python 用户
* `/home/python/doc` 和 `/home/python/build` 目录
* sphinx 相关的 python 扩展 (see [requirements.txt](requirements.txt)):
    * Sphinx
    * myst-parser
    * sphinx-autobuild
    * sphinx-last-updated-by-git
    * sphinx_rtd_theme
    * sphinxcontrib-plantuml
    * sphinx-notfound-page

### latex-base

包含 latex 基础环境，供 multi stage build 使用。

* latex 相关:
  * texlive, texlive-xetex
  * texlive-latex-extra, texlive-latex-recommended
  * latexmk
  * texlive-lang-chinese
  * texlive-science
* 字体:
  * fonts-freefont-otf
  * fonts-noto-cjk

### latex-builder

用于使用 Sphinx 生成简单软件的 pdf。

### scipy-builder

包含常见的一些 python 库，如 numpy 等。这个镜像主要用于使用 sphinx 渲染软件文档 (如 type hints) 等。

具体请看 [requirements_scipy.txt](./requirements_scipy.txt)

* scipy 相关的扩展
    * matplotlib
    * numpy
    * pandas
    * scikit-learn
    * scipy
* ipynb 相关的扩展
    * myst-nb

### scipy-latex-builder

在 latex 的基础上安装了 scipy-builder 中指定的各项 python packages 的镜像。

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

(Linux)

```bash
$ docker run --rm -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build biggates/docker-sphinx-latex-cn:builder make html
```

(Windows)

```cmd
> docker run --rm -v "%CD%":/home/python/doc -v "%CD%\build":/home/python/build biggates/docker-sphinx-latex-cn:builder make html
```

产物是整个 `build/html` 目录。

### 编译 pdf

```bash
$ docker run --rm -v "$(pwd)":/home/python/doc -v "$(pwd)/build":/home/python/build biggates/docker-sphinx-latex-cn:latex-builder make latexpdf
```

产物在 `build/latex/` 。

### 在 Jenkins 环境中使用

在 Jenkins 中有如下限制：

* 指定用 `root` 用户
* 工作空间被强制挂载到 `/root/workspace/` 目录
* 实际项目中的 requirements.txt 可能会发生变化

在 Jenkinsfile 中按如下逻辑编写即可：

```
pipeline {
    // 在前面的 stage 里面检出项目，并处理 `docs` 目录中含有 sphinx 相关的内容
    stage('Sphinx build') {
        agent {
            docker {
                // reuseNode 是为了直接利用之前检出好的项目
                reuseNode true
                image 'biggates/docker-sphinx-latex-cn:builder'

                // jenkins 会强制用 root 用户登录，因此手动再设置一次 PATH 否则找不到 sphinx
                args "-e PATH=/home/python/.venv/bin:/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            }
        }
        steps {
            // 安装项目中的 requirements
            sh "pip install -r requirements.txt"

            // 如果使用 autodoc 组件，则还需要手动将当前目录安装到 pip
            sh "pip install -e ."

            dir ('./docs') {
                sh "make html"
            }
        }
    }
}
```

## 手动编译镜像

```bash
$ docker build -t biggates/docker-sphinx-latex-cn:base -f base/Dockerfile .
$ docker build -t biggates/docker-sphinx-latex-cn:builder -f builder/Dockerfile .
$ docker build -t biggates/docker-sphinx-latex-cn:latex-base -f latex-base/Dockerfile .
$ docker build -t biggates/docker-sphinx-latex-cn:latex-builder -f latex-builder/Dockerfile .
$ docker build -t biggates/docker-sphinx-latex-cn:scipy-builder -f scipy-builder/Dockerfile .
$ docker build -t biggates/docker-sphinx-latex-cn:scipy-latex-builder -f scipy-latex-builder/Dockerfile .
```

## 参考

* https://github.com/keimlink/docker-sphinx-doc
