# IE 789,Firefox,Chrome兼容性详解收集

##CSS部分

### div类

1 . 居中问题
div里的内容，IE默认为居中，而FF默认为左对齐
可以尝试增加代码margin:auto

2 . 高度问题
两上下排列或嵌套的div，上面的div设置高度(height)，如果div里的实际内容大于所设高度，在FF中会出现两个div重叠的现象；但在IE中，下面的div会自动给上面的div让出空间
所以为避免出现层的重叠，高度一定要控制恰当，或者干脆不写高度，让他自动调节，比较好的方法是 height:100%;
但当这个div里面一级的元素都float了的时候，则需要在div块的最后，闭和前加一个沉底的空div，对应CSS是：

    .float_bottom {clear:both;height:0px;font-size:0px;padding:0;margin:0;border:0;line-height:0px;overflow:hidden;}

3 . clear:both;
不想受到float浮动的，就在div中写入clear:both;

4 . IE浮动 margin 产生的双倍距离

    .#box {
    float:left;
    width:100px;
    margin:0 0 0 100px; //这种情况之下IE会产生200px的距离
    display:inline; //使浮动忽略
    }

5 . padding 问题
FF设置 padding 后，div会增加 height 和 width，但IE不会 （* 标准的 XHTML1.0 定义 dtd 好像一致了）
高度控制恰当，或尝试使用 height:100%;
宽度减少使用 padding
但根据实际经验，一般FF和IE的 padding 不会有太大区别，div 的实际宽 = width + padding ，所以div写全 width 和 padding，width 用实际想要的宽减去 padding 定义

6 . div嵌套时 y 轴上 padding 和 marign 的问题
FF里 y 轴上 子div 到 父div 的距离为 父padding + 子marign
IE里 y 轴上 子div 到 父div 的距离为 父padding 和 子marign 里大的一个
FF里 y 轴上 父padding=0 且 border=0 时，子div 到 父div 的距离为0，子marign 作用到 父div 外面

7 . padding，marign，height，width 的傻瓜式解决技巧
注意是技巧，不是方法：
写好标准头

    <!DOCTYPE html PUBLIC “-//W3C//DTD XHTML 1.0 Transitional//EN” “http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd”>
    <html xmlns=”http://www.w3.org/1999/xhtml”>

高尽量用padding，慎用margin，height尽量补上100%，父级height有定值子级height不用100%，子级全为浮动时底部补个空clear:both的div
宽尽量用margin，慎用padding，width算准实际要的减去padding

## javascript部分


1 . document.form.item 问题
问题：
代码中存在 document.formName.item(“itemName”) 这样的语句，不能在FF下运行
解决方法：
改用 document.formName.elements["elementName"]

2 . 集合类对象问题
问题：
代码中许多集合类对象取用时使用()，IE能接受，FF不能
解决方法：
改用 [] 作为下标运算，例：

    document.getElementsByName(“inputName”)(1) 改为 document.getElementsByName(“inputName”)[1]

3 . window.event
问题：
使用 window.event 无法在FF上运行
解决方法：
FF的 event 只能在事件发生的现场使用，此问题暂无法解决。可以把 event 传到函数里变通解决：\

    onMouseMove = “functionName(event)”
    function functionName (e) {
    e = e || window.event;
    ……
    }

4 . HTML对象的 id 作为对象名的问题
问题：
在IE中，HTML对象的 ID 可以作为 document 的下属对象变量名直接使用，在FF中不能
解决方法：
使用对象变量时全部用标准的 getElementById(“idName”)

5 . 用 idName 字符串取得对象的问题
问题：
在IE中，利用 eval(“idName”) 可以取得 id 为 idName 的HTML对象，在FF中不能
解决方法：
用 getElementById(“idName”) 代替 eval(“idName”)

6 . 变量名与某HTML对象 id 相同的问题
问题：
在FF中，因为对象 id 不作为HTML对象的名称，所以可以使用与HTML对象 id 相同的变量名，IE中不能
解决方法：
在声明变量时，一律加上 var ，以避免歧义，这样在IE中亦可正常运行
最好不要取与HTML对象 id 相同的变量名，以减少错误

7 . event.x 与 event.y 问题
问题：
在IE中，event 对象有x,y属性，FF中没有
解决方法：
在FF中，与 event.x 等效的是 event.pageX ，但event.pageX IE中没有
故采用 event.clientX 代替 event.x ，在IE中也有这个变量
event.clientX 与 event.pageX 有微妙的差别，就是滚动条
要完全一样，可以这样：
mX = event.x ? event.x : event.pageX;
然后用 mX 代替 event.x

8 . 关于frame
问题：
在IE中可以用 window.testFrame 取得该frame，FF中不行
解决方法：

    window.top.document.getElementById(“testFrame”).src = ‘xx.htm’
    window.top.frameName.location = ‘xx.htm’

9 . 取得元素的属性
在FF中，自己定义的属性必须 getAttribute() 取得

10 . 在FF中没有 parentElement，parement.children 而用 parentNode，parentNode.childNodes
问题：
childNodes 的下标的含义在IE和FF中不同，FF的 childNodes 中会插入空白文本节点
解决方法：
可以通过 node.getElementsByTagName() 来回避这个问题
问题：
当html中节点缺失时，IE和FF对 parentNode 的解释不同，例如：

    <form>
    <table>
    <input/>
    </table>
    </form>

FF中 input.parentNode 的值为form，而IE中 input.parentNode 的值为空节点
问题：
FF中节点自己没有 removeNode 方法
解决方法：
必须使用如下方法 `node.parentNode.removeChild(node)`

11 . const 问题
问题：
在IE中不能使用 const 关键字
解决方法：
以 var 代替

12 . body 对象
FF的 body 在 body 标签没有被浏览器完全读入之前就存在，而IE则必须在 body 完全被读入之后才存在
这会产生在IE下，文档没有载入完时，在body上appendChild会出现空白页面的问题
解决方法：
一切在body上插入节点的动作，全部在onload后进行

13 . url encoding
问题：
一般FF无法识别js中的&
解决方法：
在js中如果书写url就直接写&不要写&

14 . nodeName 和 tagName 问题
问题：
在FF中，所有节点均有 nodeName 值，但 textNode 没有 tagName 值，在IE中，nodeName 的使用有问题
解决方法：
使用 tagName，但应检测其是否为空

15 . 元素属性
IE下 input.type 属性为只读，但是FF下可以修改

16 . document.getElementsByName() 和 document.all[name] 的问题
问题：
在IE中，`getElementsByName()`、`document.all[name]` 均不能用来取得 div 元素
是否还有其它不能取的元素还不知道（这个问题还有争议，还在研究中）

17 . 调用子框架或者其它框架中的元素的问题
在IE中，可以用如下方法来取得子元素中的值

    document.getElementById(“frameName”).(document.)elementName
    window.frames["frameName"].elementName

在FF中则需要改成如下形式来执行，与IE兼容：

    window.frames["frameName"].contentWindow.document.elementName
    window.frames["frameName"].document.elementName

18 . 对象宽高赋值问题
问题：
FireFox中类似 obj.style.height = imgObj.height 的语句无效
解决方法：
统一使用 `obj.style.height = imgObj.height + “px”;`

19 . innerText的问题
问题：
innerText 在IE中能正常工作，但是 innerText 在FireFox中却不行
解决方法：
在非IE浏览器中使用`textContent`代替`innerText`

20 . event.srcElement和event.toElement问题
问题：
IE下，even对象有srcElement属性，但是没有target属性；Firefox下，even对象有target属性，但是没有srcElement属性
解决方法：
`var source = e.target || e.srcElement;`
`var target = e.relatedTarget || e.toElement;`

21 . 禁止选取网页内容
问题：
FF需要用CSS禁止，IE用JS禁止
解决方法：
IE: `obj.onselectstart = function() {return false;}`
FF: `-moz-user-select:none;`

22 . 捕获事件
问题：
FF没有`setCapture()`、`releaseCapture()`方法
解决方法：
IE:
`obj.setCapture();`
`obj.releaseCapture();`
FF:
    window.captureEvents(Event.MOUSEMOVE|Event.MOUSEUP);
    window.releaseEvents(Event.MOUSEMOVE|Event.MOUSEUP);
    if (!window.captureEvents) {
    o.setCapture();
    }else {
    window.captureEvents(Event.MOUSEMOVE|Event.MOUSEUP);
    }
    if (!window.captureEvents) {
    o.releaseCapture();
    }else {
    window.releaseEvents(Event.MOUSEMOVE|Event.MOUSEUP);
    }

## 列表类

1 . ul 标签在FF中默认是有 padding 值的，而在IE中只有margin有值
先定义 `ul {margin:0;padding:0;}`

2 . ul和ol列表缩进问题
消除ul、ol等列表的缩进时，样式应写成: `{list-style:none;margin:0px;padding:0px;}`

### 显示类

1 .  display:

    display:block,inline 两个元素
    display:block; //可以为内嵌元素模拟为块元素
    display:inline; //实现同一行排列的的效果
    display:table; //for FF,模拟table的效果
    display:block 块元素，元素的特点是：

总是在新行上开始；
高度，行高以及顶和底边距都可控制；
宽度缺省是它的容器的100%，除非设定一个宽度
`<div>，<p>，<h1>，<form>，<ul> 和 <li>` 是块元素的例子
`display:inline` 就是将元素显示为行内元素，元素的特点是：
和其他元素都在一行上；
高，行高及顶和底边距不可改变；
宽度就是它的文字或图片的宽度，不可改变。
`<span>，<a>，<label>，<input>，<img>，<strong>` 和 `<em>` 是 inline 元素的例子

2 . 鼠标手指状显示
全部用标准的写法 `cursor: pointer;`

### 背景、图片类

1 . `background` 显示问题
全部注意补齐 `width，height` 属性

2 . 背景透明问题
    IE: filter: progid: DXImageTransform.Microsoft.Alpha(style=0,opacity=60);
    IE: filter: alpha(opacity=10);
    FF: opacity:0.6;
    FF: -moz-opacity:0.10;
最好两个都写，并将`opacity`属性放在下面

***bug report***: `mortalskuld at gmail.com`