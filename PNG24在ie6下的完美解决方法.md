# PNG24在ie6下的完美解决方法:DD_belatedPNG

DD_belatedPNG这款插件就是我推荐的： 它支持 `backgrond-position` 与 `background-repeat`，这是其他 js 插件不具备的.同时 DD_belatedPNG 还支持 `a:hover` 伪类属性，以及 `img` 标签。

> DD_belatedPNG官网：[http://www.dillerdesign.com/experiment/DD_belatedPNG/](http://www.dillerdesign.com/experiment/DD_belatedPNG/)

## 使用方法 ##

在页面底部，body标签结束前插入以下代码：

    <!-- [if IE 6]>
    <script type="text/javascript" src="script src="./DD_belatedPNG/DD_belatedPNG_0.0.8a-min.js">
    </script>
    <script type="text/javascript">
    //<![CDATA[ DD_belatedPNG.fix('.header img,.menu li,.list a'); //]]>
    </script>
    <![endif]-->

注意在这里也你要处理的标签：

`DD_belatedPNG.fix('.header img,.menu li,.list a');`

如处全部图片：

`DD_belatedPNG.fix('img');`

如处标签header下的图片：

`DD_belatedPNG.fix('.header img');`

如处理多个标签可以连写使用：

`DD_belatedPNG.fix('.header img.menu li');`
如果你想偷懒可以用以下方式来兼容（**不推荐**）：

`DD_belatedPNG.fix('*');`