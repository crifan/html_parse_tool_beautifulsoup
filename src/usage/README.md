# 使用BeautifulSoup提取html网页内容

接着介绍，如何用`BeautifulSoup`去提取HTML网页中的特定内容。

举例：

假设有html如下：

```html
    <li class="clearfix"><span>2020-12-31</span>
                                                  
              <a ahref="/xsglxt/f/jyxt/anony/showZwxx?zpxxid=104719161&type=" href="javascript:void(0)" style="color:#ff0000;" fbfw="外">2019-2020年度全国各地选调生招录、事业单位人才引进信息汇总————全国各地选调生信息汇总</a>
                                                          
        </li>     
    <li class="clearfix"><span>2020-12-31</span>
                                                  
              <a ahref="/xsglxt/f/jyxt/anony/showZwxx?zpxxid=41174064&type=" href="javascript:void(0)" style="color:#ff0000;" fbfw="外">学术就业相关资讯————清华大学学生职业发展指导中心</a>
                                                          
        </li>  
...
```

需要：提取中的`li`中的`a`的`ahref`值和文本内容`content`

## 用BeautifulSoup提取html的典型步骤

先从html中解析出soup：

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(inputHtml, 'html.parser')
```

再去从soup中提取对应的元素

比如找到所有的`li`的元素

```python
foundLiList = soup.find_all('li', attrs={"class": "clearfix"})
```

或：

```python
foundLiList = soup.find_all('li', class_="clearfix")
```

找到了`li`后，再去找其中的`a`元素

```python
foundA = eachLi.find("a", attrs={"fbfw":"外"})
```

或者是：直接找`li`中的`a`元素

```python
foundAList = soup.find_all('a', attrs={"fbfw":"外"})
```

或者再加上额外限定条件：`a`中存在属性`ahref`，且值非空

此处搜索条件中，用上了正则的写法

```python
import re
ahrefNonEmptyP = re.compile("\S+")
foundAList = soup.find_all('a', attrs={"fbfw":"外", "ahref": ahrefNonEmptyP})
```

以及也可以加上style值的限定：

```python
ahrefNonEmptyP = re.compile("\S+") # ahref="/xsglxt/f/jyxt/anony/showZwxx?zpxxid=104719161&type="
styleColorP = re.compile("color:#[a-zA-Z0-9]+;") # style="color:#ff0000;"
foundAList = soup.find_all('a', attrs={"fbfw":"外", "ahref": ahrefNonEmptyP, "style": styleColorP})
```

然后对于`findall`找到的是**列表**，每个元素类型是tag：

```bash
<class 'bs4.element.Tag'>
```

然后对于每个`tag`去通过**字典**获取**属性值**，通过`string`获取`文本值`：

```python
ahref = eachA["ahref"]
contentStr = eachA.string
```

即可获取到需要的值：

```python
# ahref=/xsglxt/f/jyxt/anony/showZwxx?zpxxid=104719161&type=
# contentStr=2019-2020年度全国各地选调生招录、事业单位人才引进信息汇总————全国各地选调生信息汇总
```

如此，即是基本的，典型的，BeautifulSoup的soup的用法了。
