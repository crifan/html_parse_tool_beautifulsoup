# 注意事项和心得

## `BeautifulSoup v3`升级到`BeautifulSoup v4`后的函数变化

官网[已解释](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#id78), BeautifulSoup从`v3`升级到`v4`后，很多函数名变化了：

* `renderContents` -> `encode_contents`
* `replaceWith` -> `replace_with`
* `replaceWithChildren` -> `unwrap`
* `findAll` -> `find_all`
* `findAllNext` -> `find_all_next`
* `findAllPrevious` -> `find_all_previous`
* `findNext` -> `find_next`
* `findNextSibling` -> `find_next_sibling`
* `findNextSiblings` -> `find_next_siblings`
* `findParent` -> `find_parent`
* `findParents` -> `find_parents`
* `findPrevious` -> `find_previous`
* `findPreviousSibling` -> `find_previous_sibling`
* `findPreviousSiblings` -> `find_previous_siblings`
* `nextSibling` -> `next_sibling`
* `previousSibling` -> `previous_sibling`

其他细节变化：

* Beautiful Soup构造方法的参数部分也有名字变化
  * `BeautifulSoup(parseOnlyThese=...)` -> `BeautifulSoup(parse_only=...)`
  * `BeautifulSoup(fromEncoding=...)` -> `BeautifulSoup(from_encoding=...)`
* 为了适配Python3,修改了一个方法名
  * `Tag.has_key()` -> `Tag.has_attr()`
* 修改了一个属性名,让它看起来更专业点
  * `Tag.isSelfClosing` -> `Tag.is_empty_element`
* 修改了下面3个属性的名字,以免与Python保留字冲突.这些变动不是向下兼容的,如果在BS3中使用了这些属性,那么在BS4中这些代码无法执行.
  * `UnicodeDammit.Unicode` -> `UnicodeDammit.Unicode_markup`
  * `Tag.next` -> `Tag.next_element`
  * `Tag.previous` -> `Tag.previous_element`

## 注意事项

### BeautifulSoup的Tag的属性

BeautifulSoup处理这种html源码：

```html
<a href="http://creativecommons.org/licenses/by/3.0/deed.zh" target="_blank">版权声明</a>
```

后，是可以通过 `soup.a['href']` 去获得对应的href属性值的。

但是，想要去获得当前的某个未知的BeautifulSoup.Tag中，一共存在多少个属性，以及每个属性的值的时候，却不知道如何下手了。

比如对于如下html源码：

```xml
<p class="cc-lisence" style="line-height:180%;">......</p>
```

想要得知，一共有两个属性，分别是class和style，然后就可以通过上面的方法，去获得对应的属性值了。

对此问题，虽然看到了官网的解释：[Tags的属性](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#tag)中的`你可以将Tag看成字典来访问标签的属性`

但是还是无法通过：

```python
if("class" in soup)
```

的方式去判断soup中是否存在class属性，因为此时soup是Beautifulsoup.Tag类型变量，而不是dict变量。

并且，如果去强制将soup转换成为dict变量：

```python
soupDict = dict(soup)
```

会报错的。

最后，还是无意间发现，原来`Beautifulsoup.Tag`是有个`attrs`属性的，其可以获得对应的元组列表，每一个元组是对应属性名和属性值：

```python
attrsList = soup.attrs
print("attrsList=%s" % attrsList()

attrsList= [('class', 'cc-lisence'), ('style', 'line-height:180%;')]
```

这样自己就可以从元组列表中去转换，获得属性的列表或字典变量了，就可以接着按照自己意愿去处理了。

另外，此处也通过`soup.name`获得了该tag的名字

而想要获得整个soup变量所有的属性和方法的话，可以用经典的dir去打印出来:

```python
print("dir(soup)=%s" % dir(soup))
```

此处的打印输出为：

```bash
dir(soup)= ['BARE_AMPERSAND_OR_BRACKET', 'XML_ENTITIES_TO_SPECIAL_CHARS', 'XML_SPECIAL_CHARS_TO_ENTITIES', '__call__', '__contains__', '__delitem__', '__doc__', '__eq__', '__getattr__', '__getitem__', '__init__', '__iter__', '__len__', '__module__', '__ne__', '__nonzero__', '__repr__', '__setitem__', '__str__', '__unicode__', '_convertEntities', '_findAll', '_findOne', '_getAttrMap', '_invert', '_lastRecursiveChild', '_sub_entity', 'append', 'attrMap', 'attrs', 'childGenerator', 'containsSubstitutions', 'contents', 'convertHTMLEntities', 'convertXMLEntities', 'decompose', 'escapeUnrecognizedEntities', 'extract', 'fetch', 'fetchNextSiblings', 'fetchParents', 'fetchPrevious', 'fetchPreviousSiblings', 'fetchText', 'find', 'findAll', 'findAllNext', 'findAllPrevious', 'findChild', 'findChildren', 'findNext', 'findNextSibling', 'findNextSiblings', 'findParent', 'findParents', 'findPrevious', 'findPreviousSibling', 'findPreviousSiblings', 'first', 'firstText', 'get', 'has_key', 'hidden', 'insert', 'isSelfClosing', 'name', 'next', 'nextGenerator', 'nextSibling', 'nextSiblingGenerator', 'parent', 'parentGenerator', 'parserClass', 'prettify', 'previous', 'previousGenerator', 'previousSibling', 'previousSiblingGenerator', 'recursiveChildGenerator', 'renderContents', 'replaceWith', 'setup', 'substituteEncoding', 'toEncoding']
```

有需要的话，可以对这些属性和方法，都尝试一下，以便更加清楚其含义。

刚写完上面这句话呢，然后自己随便测试了一下`attrMap`：

```python
attrMap = soup.attrMap
print("attrMap=%s" % attrMap)
```

然后就很惊喜的发现，原来此处的attrMap，就是我程序中所需要的属性的dict变量啊：

```python
attrMap= {'style': 'line-height:180%;', 'class': 'cc-lisence'}
```

这样，就又省去了我程序中将attrs转换为dict变量的操作了，更加提高了效率。

在次感谢Beautifulsoup的开发者。

## BeautifulSoup有时候会遇到非法的，不支持的html源码而导致无法解析或无法正常解析html

在使用Beautifulsoup过程中，对于大多数html源码，通过指定正确的编码，或者本身是默认UTF-8编码而无需指定编码类型，其都可以正确解析html源码，得到对应的soup变量。

然后就接着去利用soup实现你所想要的功能了。

但是有时候会发现，有些html解析后，有些标签等内容丢失了，即所得到的soup不是所期望的完整的html的内容。

这时候，很可能遇到了非法的html，即其中可能包含了一些不合法的html标签等内容，导致Beautifulsoup虽然可以解析，没有报错，但是实际上得到的soup变量，内容缺失了一部分了。

比如我就遇到过不少这样的例子：

### 部分Blogbus的帖子的html中非html5和html5的代码混合导致Beautifulsoup解析错误

之前在[为BlogsToWordPress添加Blogbus支持](http://www.crifan.com/record_add_blogbus_for_blogstowordpress/)过程中去解析Blogbus的帖子的时候，遇到一个特殊的帖子：

http://ronghuihou.blogbus.com/logs/89099700.html

其中一堆的非html5的代码中，包含了这样一段html5的代码的写法，即标签属性值不加括号的：

```html
<SCRIPT language=JavaScript> 
document.oncontextmenu=new Function("event.returnValue=false;"); //禁止右键功能,单击右键将无任何反应 
document.onselectstart=new Function( "event.returnValue=false;"); //禁止先择,也就是无法复制 
</SCRIPT language=JavaScript>
```

结果导致Beautifulsoup解析错误，得到的soup中，找不到所需要的各种class等属性值。

对应的解决办法就是，把这部分的代码删除掉，然后再解析就可以了：

其中一堆的非html5的代码中，包含了这样一段html5的代码的写法，即标签属性值不加括号的：

```python
foundInvliadScript = re.search("<SCRIPT language=JavaScript>.+</SCRIPT language=JavaScript>", html, re.I | re.S )
logging.debug("foundInvliadScript=%s", foundInvliadScript)
if(foundInvliadScript):
    invalidScriptStr = foundInvliadScript.group(0)
    logging.debug("invalidScriptStr=%s", invalidScriptStr)
    html = html.replace(invalidScriptStr, "")
    logging.debug("filter out invalid script OK")

soup = htmlToSoup(html)
```

### 判断浏览器版本的相关代码，导致Beautifulsoup解析不正常

之前在给[BlogsToWordpress](http://www.crifan.com/crifan_released_all/website/python/blogstowordpress/)添加新浪博客的支持的过程中

遇到很多新浪博客的帖子的html中，包含很多判断浏览器版本的相关代码：

```html
<!–[if lte IE 6]>
xxx
xxx
<![endif]–>
```

由此导致Beautifulsoup解析html不正常。

### font标签嵌套层次太多，导致Beautifulsoup无法解析html

接上面那个解析新浪博客帖子的例子，期间又遇到另外一个问题，对于一些特殊帖子：

http://blog.sina.com.cn/s/blog_5058502a01017j3j.html

其包含特殊的好几十个font标签且是一个个嵌套的代码，导致无法Beautifulsoup无法解析html，后来把对应嵌套的font标签删除掉，才可以正常解析。

相关python代码为：

```python
# handle special case for http://blog.sina.com.cn/s/blog_5058502a01017j3j.html
processedHtml = processedHtml.replace('<font COLOR="#6D4F19"><font COLOR="#7AAF5A"><font COLOR="#7AAF5A"><font COLOR="#6D4F19"><font COLOR="#7AAF5A"><font COLOR="#7AAF5A">', "")
processedHtml = processedHtml.replace("</FONT></FONT></FONT></FONT></FONT></FONT>", "")
```

遇到其他类似的问题，也可以去删除或替换出错代码，即可解决问题。

不过需要说明的是，很多时候，你未必很容易就找到出错的代码。

想要找到出错的代码，更多的时候，需要你一点点调试，一点点的删除看似可疑的一些html源码，然后最终才能定位到出错的代码，然后删除掉后，才可以正常工作的。

## 使用心得

## crifan的Beautifulsoup函数

之前已把一些常用功能封装成函数，放到自己的库中了，详见：

[[crifanLibPython/crifanBeautifulsoup.py at master · crifan/crifanLibPython](https://github.com/crifan/crifanLibPython/blob/master/crifanLib/crifanBeautifulsoup.py)

## `find`和`find_all`中支持正则写法

如之前例子中所提到的：

```python
ahrefNonEmptyP = re.compile("\S+") # ahref="/xsglxt/f/jyxt/anony/showZwxx?zpxxid=104719161&type="
styleColorP = re.compile("color:#[a-zA-Z0-9]+;") # style="color:#ff0000;"
foundAList = soup.find_all('a', attrs={"fbfw":"外", "ahref": ahrefNonEmptyP, "style": styleColorP})
```

`soup.find`和`soup.find_all`中，都支持正则的写法：

```python
# also match a-incontent a-title cs-contentblock-hoverlink
titleP = re.compile("a-incontent a-title(\s+?\w+)?")
foundIncontentTitle = soup.find(attrs={"class":titleP})
```

就可以同时匹配多种情况了：

* `a-incontent a-title`
* `a-incontent a-title cs-contentblock-hoverlink`
* `a-incontent a-title xxx`
