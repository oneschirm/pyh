PyH is a simple python module (pyh.py) that lets you generate html output in a convenient and intuitive manner.

#Installation
Simply enter:

```bash
sudo ./setup.py install
```

#Usage
In your python script :

from pyh import *

#Quick example
The following python code snipet

```python
from pyh import *
page = PyH('My wonderful PyH page')
page.addCSS('myStylesheet1.css', 'myStylesheet2.css')
page.addJS('myJavascript1.js', 'myJavascript2.js')
page << h1('My big title', cl='center')
page << div(cl='myCSSclass1 myCSSclass2', id='myDiv1') << p('I love PyH!', id='myP1')
mydiv2 = page << div(id='myDiv2')
mydiv2 << h2('A smaller title') + p('Followed by a paragraph.')
page << div(id='myDiv3')
page.myDiv3.attributes['cl'] = 'myCSSclass3'
page.myDiv3 << p('Another paragraph')
page.printOut()
```

will generate the following html

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>My wonderful PyH page</title>
<link href="myStylesheet1.css" type="text/css" rel="stylesheet" />
<link href="myStylesheet2.css" type="text/css" rel="stylesheet" />
<script src="myJavascript1.js" type="text/javascript"></script>
<script src="myJavascript2.js" type="text/javascript"></script>
</head>
<body>
<h1 class="center">My big title</h1>
<div id="myDiv1" class="myCSSclass1 myCSSclass2">
<p id="myP1">I love PyH!</p>
</div>
<div id="myDiv2">
<h2>A smaller title</h2>
<p>Followed by a paragraph.</p>
</div>
<div id="myDiv3" class="myCSSclass3">
<p>Another paragraph</p>
</div>
</body>
</html>
```

#Extended Examples

##Introduction

PyH is a very powerful and compact python module that lets you write HTML content from within your python program. Hard-coding plain HTML into your python code is boring and can make your code completely unreadable. Plus, once you'll have a look at the source of the HTML file, it won't be readable either. PyH gives you a nice solution for all of this. PyH lets you code your web page like a GUI! 

##Features
* Automatic formating of HTML tags
* High customizability
* full CSS and Javascript awareness
* Automatic tag closure
* Object-oriented coding of HTML 

##The Tag objects

HTML tags can be generated by calling the function of the same name. The HTML tag <tag> is invoked via the class tag() :

```python
>>> mydiv = div()
>>> mydiv.render()
'<div></div>'
```

Tag attributes are passed as keyword parameters to the functions. The keyword is the same name as the attribute except for the attribute class which is replaced by cl. The content of the tag (text or sub-tags) is passed a non-keyword parameter :

```python
>>> mydiv = div('My content', cl='myCSSclass1 myCSSclass2', id='myCSSid1')
>>> mydiv.render()
'<div class="myCSSclass1 myCSSclass2" id="myCSSid1">
My content
</div>'
```

Other tags can also be passed as content :

```python
>>> mydiv = div(p('My paragraph.'), cl='myCSSclass1 myCSSclass2', id='myCSSid1')
>>> mydiv.render()
'<div class="myCSSclass1 myCSSclass2" id="myCSSid1">
<p>My paragraph</p>
</div>'
```

Once a tag object has been created, its HTML attributes can be modified by accessing its attributes member which is a dictionary :

```python
>>> mydiv = div()
>>> mydiv.attributes['id'] = 'myCSSid'
>>> mydiv.render()
'<div id="myCSSid"></div>\n'
```

Tag can be concatenated by the use of the simple + operator :

```python
>>> twoDivs = div() + div()
>>> twoDivs.render()
'<div></div>
<div></div>'
```

Tags can be included inside higher level tags either by passing them as non-keywords arguments as explained above or thanks to the << operator. The operation returns the last included tag :

```python
>>> myDiv = div(id='myTopLevelDiv')
>>> myPar = myDiv << div(id='myInnerDiv') << p(id='myPar') 
>>> myPar << span('My first span') + span('My second span')
>>> myDiv.render()
'<div id="myTopLevelDiv">
<div id="myInnerDiv">
<p id="myPar">
<span>My first span</span><span>My second span</span>
</p>
</div>
</div>'
```

Once a tag has been included into another one it can still be accessed as a member of the upper-level tag. The name of the member is the id of the tag if it is specified. Otherwise it is the name of the tag if it is the first tag of its kind, 'tag_001' if its the second and so on :

```python
>>> myDiv = div()
>>> myDiv << span(id='myspan')
>>> myDiv.myspan << 'content1'
>>> myDiv << span()
>>> myDiv.span << 'content2'
>>> myDiv << span()
>>> myDiv.span_001 << 'content3'
>>> myDiv.render()
'<div>
<span id="myspan">content1</span>
<span>content2</span>
<span>content3</span>
</div>'
```

*PyH stands for any of the following : Pour yourself a ScotcH, Poor young Hobo or Peel your HTML. Any other suggestions?*
