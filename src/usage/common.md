# BeautifulSoup常用函数

## 所有函数和属性

参考 [Python & BeautifulSoup - can I use the function findAll repeatedly? - Stack Overflow](https://stackoverflow.com/questions/31866094/python-beautifulsoup-can-i-use-the-function-findall-repeatedly) 中别人回复，可以看出soup的所有属性和函数是：

```python
f.HTML_FORMATTERS           f.has_attr
f.XML_FORMATTERS            f.has_key
f.append                    f.hidden
f.attribselect_re           f.index
f.attrs                     f.insert
f.can_be_empty_element      f.insert_after
f.childGenerator            f.insert_before
f.children                  f.isSelfClosing
f.clear                     f.is_empty_element
f.contents                  f.name
f.decode                    f.namespace
f.decode_contents           f.next
f.decompose                 f.nextGenerator
f.descendants               f.nextSibling
f.encode                    f.nextSiblingGenerator
f.encode_contents           f.next_element
f.extract                   f.next_elements
f.fetchNextSiblings         f.next_sibling
f.fetchParents              f.next_siblings
f.fetchPrevious             f.parent
f.fetchPreviousSiblings     f.parentGenerator
f.find                      f.parents
f.findAll                   f.parserClass
f.findAllNext               f.parser_class
f.findAllPrevious           f.prefix
f.findChild                 f.prettify
f.findChildren              f.previous
f.findNext                  f.previousGenerator
f.findNextSibling           f.previousSibling
f.findNextSiblings          f.previousSiblingGenerator
f.findParent                f.previous_element
f.findParents               f.previous_elements
f.findPrevious              f.previous_sibling
f.findPreviousSibling       f.previous_siblings
f.findPreviousSiblings      f.recursiveChildGenerator
f.find_all                  f.renderContents
f.find_all_next             f.replaceWith
f.find_all_previous         f.replaceWithChildren
f.find_next                 f.replace_with
f.find_next_sibling         f.replace_with_children
f.find_next_siblings        f.select
f.find_parent               f.select_one
f.find_parents              f.setup
f.find_previous             f.string
f.find_previous_sibling     f.strings
f.find_previous_siblings    f.stripped_strings
f.format_string             f.tag_name_re
f.get                       f.text
f.getText                   f.unwrap
f.get_text                  f.wrap
```

供概览了解有哪些属性和功能。

## 常见属性

* 当前级别
  * 文本
    * [当前节点的文本值](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#string)
      * `curNode.string`
    * [当前节点的子节点的内容content的列表](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#contents-children)，即str的list
      * `curNode.contents`
* 同级
  * 兄弟节点
    * 向前
      * [前一个兄弟](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#next-sibling-previous-sibling)
        * `curNode.previous_sibling`
      * [前面的所有的兄弟节点](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#next-siblings-previous-siblings)
        * `curNode.previous_siblings`
    * 向后
      * [后一个兄弟](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#next-sibling-previous-sibling)
        * `curNode.next_sibling`
      * [后面的所有的兄弟节点](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#next-siblings-previous-siblings)
        * `curNode.next_siblings`
* 上下级
  * 向上
    * 当前节点的[父亲](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#parent)
      * `curNode.parent`
    * 当前节点的（向上查找到的）[所有的父亲节点](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#parents)
      * `curNode.parents`
  * 向下
    * 一级
      * （当前 直接的）[子节点的列表](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#contents-children)，即Tag的list
        * `curNode.children`
    * 所有子级
      * 文本
        * [所有子节点的文本](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#strings-stripped-strings)
          * `curNode.strings`
        * [去除空行后的所有子节点的文本](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#strings-stripped-strings)
          * `curNode.stripped_strings`
      * 节点
        * （其下所有的）[子孙节点的列表](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#descendants)
          * `curNode.descendants`

### 常见用法

#### tag的名字

```python
curTagStr = eachSoupNode.name
```

得到：

```xml
<XCUIElementTypeStaticText type="XCUIElementTypeStaticText" value="兴业 信用卡" name="兴业 信用卡" label="兴业 信用卡" enabled="true" visible="true" x="99" y="593" width="81" height="18"/>
```

中的tag名：`XCUIElementTypeStaticText`

#### 节点的attrs属性是dict字典

```python
curAttrib = eachSoupNode.attrs
```

就是一个dict了，对于：

```xml
<XCUIElementTypeButton type="XCUIElementTypeButton" enabled="true" visible="true" x="0" y="0" width="414" height="691">
```

值是：

```python
{'enabled': 'true', 'height': '691', 'type': 'XCUIElementTypeButton', 'visible': 'true', 'width': '414', 'x': '0', 'y': '0'}
```

另外例子：

html：

```html
<h4>
    <a href="../sanguozhanji/" target="_blank" title="三国战纪"><em
            class='keyword'>三国</em>战纪(官方正版)</a>
    <span>
        20年经典风靡街机厅
    </span>
</h4>
```

获取属性：

```python
h4Soup = dtSoup.find("h4")
h4aSoup = h4Soup.find("a")
h4aAttrDict = h4aSoup.attrs # h4aAttrDict={'href': '../sanguozhanji/', 'target': '_blank', 'title': '三国战纪'}
aHref = h4aAttrDict["href"] # '../sanguozhanji/'
aTitle = h4aAttrDict["title"] # '三国战纪'
```

#### 删除某个属性

官网文档：[attributes](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#attributes)

就像删除dict中的某个key一样：

```python
del curNode.attrs["keyToDelete"]
```

或：

```python
curNodeAttributeDict = curNode.attrs
del curNodeAttributeDict["keyToDelete"]
```

## 常见函数操作

### soup.find

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

### soup.findall

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

### decompose 删除节点

```python
nodeToDelete.decompose()
```

官网文档：[decompose()](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html#decompose)
