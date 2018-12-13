# NW.js 中文文档翻译计划
鉴于NW.js如此好用的项目没有中文文档，特此建立NW.js文档中文文档翻译计划。

## 如何使用
点击Download ZIP按钮下载本项目，解压缩后访问site文件夹中的index.html即可。

## 如何编译
中文文档同英文文档相同，使用markdown书写并用mkdocs编译而成。

需要Python和Python package manager pip 来安装MkDocs
使用pip安装mkdocs:
```
$ pip install mkdocs
```
运行 mkdocs -V 以检查是否正确安装
```
$ mkdocs -V
```
进入项目文件夹，执行以下命令编译
```
mkdocs build --clean
```
会基于mkdocs.yml配置将docs中的原始md文件编译到site文件夹中形成静态页面。

## 参与翻译
fork本项目并编辑docs文件夹中的md文件，然后申请提交。 