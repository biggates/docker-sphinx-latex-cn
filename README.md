## 主要内容

### base

* 基于 [nikolaik/python-nodejs:python3.8-nodejs14](https://hub.docker.com/r/nikolaik/python-nodejs)
    * 主体是 Debian buster
    * git
    * python3, pip3

### builder

* plantuml
* graphviz/dot
* sphinx 相关的扩展 (see [requirements.txt](requirements.txt)):
    * Sphinx
    * recommonmark
    * sphinx_rtd_theme
    * sphinx-markdown-tables
    * sphinxcontrib-napoleon
    * sphinxcontrib-plantuml
    * sphinx-notfound-page
    * sphinx-jsonschema

### latex

* 操作系统：
    * texlive-xetex, texlive-lang-chinese, latexmk
    * fonts-freefont-otf

### scipy-builder

* scipy 相关的扩展
    * matplotlib
    * numpy
    * pandas
    * pandoc
    * scikit-learn
    * scipy
    * twine
    * wfdb
* ipynb 相关的扩展
    * nbsphinx

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

### Jenkins 环境

在 Jenkins 中有如下限制：

* 指定用 `root` 用户
* 工作空间被强制挂载到 `/root/workspace/` 目录
* requirements.txt 可能会发生变化

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
            // 安装项目中的 requirements，大部分常用项目已安装所以这里会比较快
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
$ docker build -t docker-sphinx-latex-cn:base -f base/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:builder -f builder/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:latex-base -f latex-base/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:latex-builder -f latex-builder/Dockerfile .
$ docker build -t docker-sphinx-latex-cn:scipy-builder -f scipy-builder/Dockerfile .
```

## 参考

* https://github.com/keimlink/docker-sphinx-doc
