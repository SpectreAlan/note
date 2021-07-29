## 安装

```sh
# 安装脚手架
npm install gitbook-cli -g 
# 初始化
gitbook init [bookName]
# 预览书籍
gitbook serve
# 打包
gitbook build
```

## 目录结构

```sh
├── book.json
├── README.md
├── SUMMARY.md
├── chapter-1/
|   ├── README.md
|   └── something.md
└── chapter-2/
    ├── README.md
    └── something.md
```

### book.json

```
title - 标题
author - 作者信息
description - 书本描述
language - 使用的语言
gitbook - 指定gitbook版本
root - 指定存放 GitBook 文件的根目录
links - 在侧边栏添加链接
styles - 自定义样式
plugins - 插件
pluginsConfig - 插件配置
structure - 设置 Readme, Summary, Glossary等对应的文件
```

示例如下：

```json
{
     "title": "AlanGrady’s personal note",
     "description": "这是一个description",
     "author": "AlanGrady",
     "links": {
          "sidebar": {
               "Home": "https://jszoo.com"
          }
     },
     "plugins": [
          "-lunr",
          "-search",
          "search-pro",
          "-highlight",
          "-sharing",
          "-font-settings",
          "-livereload",
          "book-summary-scroll-position-saver",
          "chapter-fold",
          "multipart",
          "popup",
          "github",
          "favicon",
          "copy-code-button",
          "expandable-chapters-small",
          "prism",
          "splitter",
          "anchors",
          "navigator",
          "lightbox"
     ],
     "pluginsConfig": {
          "github": {
               "url": "https://github.com/SpectreAlan"
          },
          "favicon": {
               "shortcut": "/assets/images/favicon.ico",
               "bookmark": "/assets/images/favicon.ico",
               "appleTouch": "/assets/images/logo.png",
               "appleTouchMore": {
                    "120x120": "/assets/images/logo.png",
                    "180x180": "/assets/images/logo.png"
               }
          },
          "expandable-chapters-small": {},
          "prism": {
               "css": [
                    "prismjs/themes/prism-okaidia.css"
               ]
          }
     }
}
```
### README.md
GitBook前言

### SUMMARY.md
概要文件主要存放 GitBook 的文件目录信息，左侧的目录就是根据这个文件来生成的，默认对应的文件是 SUMMARY.md
示例如下：
```
* [Git](notes/git/readme.md)
  * [Git使用](notes/git/git.md)
  * [gitbook](notes/git/gitbook.md)

* [服务器](notes/server/readme.md)
  * [Nginx](notes/server/nginx.md)
  * [Mysql](notes/server/mysql.md)
```
出来的效果：
```
Git
    Git使用
    gitbook
服务器
    Nginx
    Mysql
```
