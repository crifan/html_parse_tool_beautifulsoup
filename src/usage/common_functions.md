# BeautifulSoup常用函数

## soup.find

* 官网文档
  * 中文
    * [find(name, attrs, recursive, string, **kwargs)](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#find)
  * 英文
    * [find(name, attrs, recursive, string, **kwargs)](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#find)

更多实际用法举例：

```python
    # <h1 class="h1user">crifan</h1>

    # method 1: no designate para name
    h1userSoup = soup.find("h1", {"class":"h1user"})

    # method 2: use para name
    h1userSoup = soup.find(name="h1", attrs={"class":"h1user"})

    h1userUnicodeStr = h1userSoup.string
```

修改其中内容：

注：只能改（Tag的）中的属性的值，不能改（Tag的）的值本身

```python
soup.body.div.h1.string = changedToString

soup.body.div.h1['class'] = "newH1User"
```

## soup.findall

* 官网文档
  * 中文
    * [find_all(name, attrs, recursive, string, **kwargs)](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#find-all)
  * 英文
    * [find_all(name, attrs, recursive, string, limit, **kwargs)](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#find-all)

默认findall会返回匹配的所有的元素

想要限制返回个数，可以加`limit`

`soup.find_all('title', limit=2)`

特殊：

`find` == `limit=1`的`findall`

即：如下是相同含义

```python
soup.find_all('title', limit=1)
# [<title>The Dormouse's story</title>]

soup.find('title')
# <title>The Dormouse's story</title>
```
