---
abbrlink: ''
categories:
- 博客配置
date: '2022-08-13 13:49:55'
tags:
- mathjax
- 博客配置
title: hexo博客mathjax支持
updated: '2022-08-13 13:49:57'
---我采用的这个主题没有自带数学渲染器，（但写文章怎么能没有latex呢），所以就寻找安装latex办法这里给出一种简单方法。qwq

## 安装

```bash
$ npm install hexo-filter-mathjax
$ hexo clean
```

## 配置

在博客目录下的_config.yml内添加

```yaml
mathjax:
  tags: none # or 'ams' or 'all'
  single_dollars: true # 单个美元符号是否运用latex
  cjk_width: 0.9 # relative CJK char width
  normal_width: 0.6 # relative normal (monospace) width
  append_css: true # add CSS to pages rendered by MathJax
  every_page: false #如果设为true，每个页面都会加载mathjax 建议关闭
  packages: # extra packages to load
  extension_options: {}
    # you can put your extension options here
    # see http://docs.mathjax.org/en/latest/options/input/tex.html#tex-extension-options for more detail
```

在需要加载latex的front-matter写入`mathjax: true`

## 注意

1. 单个美元符号引入的公式美元符号，开头的$后和结尾的$前不能有空格
2. 如果latex内符号和markdown语法有冲突 请用\转义 如* 要写成\*
3. [this](https://latexlive.com) 可以将你的latex转义

