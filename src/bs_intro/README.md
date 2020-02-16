# BeautifulSoup简介

> #### note:: 旧教程
> 
> 之前已有写过一个旧版本的教程，用`docbook`发布的：[Python专题教程：BeautifulSoup详解](https://www.crifan.com/files/doc/docbook/python_topic_beautifulsoup/release/html/python_topic_beautifulsoup.html)
> 
> 现已把其内容整理合并到此新版教程。

对于HTML网页的解析，可以使用[Python中的正则表达式re](http://book.crifan.com/books/python_regex_re_intro/website)去提取所需内容。

但是前提（往往是）被解析的html不够复杂，否则正则就很难写，或者说写不出来。

而对于html网页（和xml）的解析，有个专门的库，叫做：

* `BeautifulSoup`
  * 简称：`bs`
    * 最新版本是`v4`，简称：`bs4`
  * 核心功能：解析`HTML`和`XML`
  * 特点
    * 功能强大
    * 支持语法有问题的`HTML`的解析

## 为何叫`BeautifulSoup`

* `BeautifulSoup`
  * 中文直译：`美味的汤`
    * 个人推测是：
      * -》 （让人）喝起来很爽（的汤）
        * -》`BeautifulSoup`的目的就是：
          * 让你从网页中提取内容**很方便**
            * -》让你像**喝美味的汤一样的爽**

## 什么时候会用到BeautifulSoup

`BeautifulSoup`这个技术所属领域：一般来说属于Python的爬虫相关的技术领域范围内

一般是在：已经用`requests`等库或框架，爬取得到了网页源码，然后想要从html源码中提取特定的内容时

往往才会用到这个：BeautifulSoup

## BeautifulSoup的版本

* 之前：`BeautifulSoup 3`
  * 只支持`Python 2`
    * Python官网（在**20200101**之后）已不再继续维护`Python 2`了
      * 现在已经是20200216了，大家也都尽量不再用Python 2，而改用`Python 3`了
  * 最后版本：3.2.2
    * 截至：2019-10-05
  * 安装包：
    * `Debian`/`Ubuntu`：`python-beautifulsoup`
    * `Fedora`：`python-BeautifulSoup`
* 最新：`BeautifulSoup 4`
  * 支持：`Python 2` (`2.7`+)和`Python 3`
  * 最新版本是：`Beautiful Soup 4.8.2`
    * 截至：2019-12-24
  * 基于BeautifulSoup 4有个：`bs4`
    * 安装包
      * `Debian`/`Ubuntu`
        * `Python 2`：`python-bs4`
        * `Python 3`：`python3-bs4`
      * `Fedora`
        * `python-beautifulsoup4`

## 官网文档

* 主入口
  * Beautiful Soup: We called him Tortoise because he taught us
    * https://www.crummy.com/software/BeautifulSoup/
* api文档
  * 英文
    * Beautiful Soup Documentation — Beautiful Soup 4.4.0 documentation
      * https://www.crummy.com/software/BeautifulSoup/bs4/doc/
  * 中文
    * Beautiful Soup 4.4.0 文档 — Beautiful Soup 4.2.0 documentation
      * https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/
