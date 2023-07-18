---
abbrlink: ''
categories:
- - 折腾
date: '2023-07-18T12:24:40.673129+08:00'
tags:
- vercel
- umami
title: umami安装介绍
updated: 2023-7-18T13:32:32.512+8:0
---
## umami介绍

[umami开源仓库](https://github.com/umami-software/umami)

> Umami is an open-source, self-hosted web analytics solution

正如官网所说，umami是一个开源的，自托管的网络统计工具。可以帮助你更加准确的统计网站的数据。

界面截图：

![umami](https://cdn.histcat.top/rawimg/umami.1.261l6ql8a7eo.png)

![umami](https://cdn.histcat.top/rawimg/umami.2.75qyvtpce800.png)

## 安装过程简谈

1. 注册数据库账户，获得链接url
2. fork仓库，vercel部署
3. 添加网站并配置

### 1.注册数据库账户，获得连接url

随便找一个免费的mysql或postgresql数据库，这里推荐[Supabase](https://supabase.com/)，点击[这里](https://supabase.com/dashboard/sign-up)注册，可以直接使用github账户。登录后点击“New Project”。

![umami](https://cdn.histcat.top/rawimg/umami.3.3yq9fupg6e60.webp)

name随便填，Database Password为数据库密码，**务必记牢**。地区不用改，点击Create New Project，之后要等待一段时间初始化。

![umami](https://cdn.histcat.top/rawimg/umami.4.62dvi2g2kfs0.webp)

之后按照图片流程，点击copy，并把密码改成设置的。数据库设置至此完毕。

### 2.fork仓库，vercel部署

[点击](https://github.com/umami-software/umami)进入后fork仓库，打开vercel，新建项目并导入仓库。![umami](https://cdn.histcat.top/rawimg/umami.5.2id62uvtxnq0.webp)

点击"Environment Variables"，添加3个环境变量

```json
DATABASE_URL:刚刚复制并改动的字符串
HASH_SALT:随机串（哈希盐）
TRACKER_SCRIPT_NAME:随机串（js文件名称，防止被广告拦截插件拦截）
```

Deploy，绑定域名，完成！

### 3.添加网站并配置

（在右上角的地球图标那里可以改成中文）

点击添加网站

![umami](https://cdn.histcat.top/rawimg/umami.6.4ro0erq2rde0.webp)

名字随便起，域名填你想统计的网站的域名。

![umami](https://cdn.histcat.top/rawimg/umami.7.4zfugjhg7m40.webp)

把跟踪代码复制到网站的\<head\>部分（每个网站的实现方法不同，例如hexo主题一般可以在配置文件里添加）就完成啦QwQ
