---
layout:  post
title: javascript高级程序设计-chapter13-事件
tags: javascript高级程序设计  ReadingNotes
categories: javascript高级程序设计
---


* TOC
{:toc}




本章内容
    理解浏览器对象window对象----BOM核心
    控制窗口,框架个弹出窗口
    利用location对象中的页面信息
    利用navigator对象了解浏览器




##window对象：

    BOM的核心对象是window，他表示一个浏览器实例，他是js访问的接口也是ECMAScript规定的global对象。//这句话不甚明白

##全局作用域：

    window是global对象，所有任何定义在全局的属性和方法都是window的属性和方法。
    delete操作符无法删除var 定义的全局变量，但是能够删除global对象里面的属性和方法，也就是能删除window对象里面的属性和方法。在用delete删除var 定义的全局变量是 IE<9抛出错误，其他返回false；
    Windows Mobile 平台的 IE 浏览器不允许通过 window.property = value 之类的形式，直接在 window 对象上创建新的属性或方法。可是，在全局作用域中声明的所有变量和函数，照样会变成 window 对象的成员。

##窗口关系以及框架：
    如果页面包含框架，则每个框架都有自己的window属性，并且保存在frames集合中，在frames框架集中，可以通过数值索引（从0开始，从上到下，从左到右）来访问框架，也可以通过window的name属性来访问框架。

    top：top对象指向的最外层的框架，他是浏览器窗口。使用它可以确保在一个框架中正确的访问另外一个框架。window对象指定的是这个框架的特定实例。用法：top.frames[0]这样的方式去访问。
    parent：指向的是该框架的父框架。在没有框架集的情况下，top和parent和window指向是同一个窗口。
    注意，除非最高层窗口是通过 window.open()打开的（本章后面将会讨论） ，否则其 window 对象的 name 属性不会包含任何值。
    self：指向的是当前框架本身。
    top，parent，self都是window的属性，所有我们可以通过window.parent.parent.frames[]来访问相应的框架。

##窗口位置：
    用来确定window对象位置的属性和方法有很多。
    screenLeft和screenTop




##location对象
    location对象即是window的对象也是document的对象；
    location对象有如下的属性：
        属性名                   例子                              说明
        hash                   #content                         返回url中的hash（#后面跟的字符）如果url不包含，则返回空
        host                   www.baidu.com:80                 返回服务器的名称和端口号（如果有）
        hostname               www.baidu.com                    返回不带端口号的服务器名称
        pathname               /activity/topic/index            返回url中的文件名和目录
        port                   8080                             返回端口
        protocol                http：                          返回走的协议
        search                 ?q=****                          返回URL的查询字符串。这个字符串以问号开头
    查询字符参数：
        方法一：//这个方法返回的是一个数组对象，做判断时应该这样子去调用queryPrams().name
        function queryPrams(){
            var prams = (window.location.search.length > 0 ? window.location.search.substring(1) : ''),
                args = [],
                items = prams !== '' ? prams.split('&') : [],
                item = '',
                name = '',
                value = '';
            for (var i = 0 ; i < items.length ; i++) {
                item = items[i].split('=');
                name = decodeURIComponent(item[0]);
                value = decodeURIComponent(item[1]);
                if (name.length) {
                    args[name] = value ;
                }
            }
            return args;
        }

        方法二：//返回匹配的字符串
        function queryPrams(pram){
            var prams = (window.location.search.length >0 ? window.location.search.substring(1) : ''),
                args = [],
                items = prams !== '' ? prams.split('&') : [],
                item = [];
                for(var i = 0 ; i < items.length ; i++ ) {
                    item = items[i].split('=');
                    if (decodeURIComponent(item[0])===pram) {
                        return decodeURIComponent(item[1])
                    }
                }
        }

    位置操作：
        location.assign(url):这个方法可以在浏览器中一个地址，并且会在历史记录中生成一条记录。直接为location.href赋值也会调用这个方法。
        location.replace(url):这个方法不会再浏览器中生成历史纪录。所以打开了之后不能够回退。我在chrome中测试，会生成历史纪录，原生的任然会被使用。
        //replace()需要进行更多的实践。
        location.reload(true):如果不加参数true，那么不会进行强制加载，文件没有改变的情况下，会从缓存中加载文件。


##navigator对象：
    navigator对象是所有支持javascript的浏览器都有的，但是和其他的BOM对象一样，各浏览器之前有区别。
    检查插件：
        在非IE的浏览器中，插件都保存在plugins数组中。数组中的没一项都包含name，description，filename，length（插件所处理的MIME类型数量）。

        代码：
        function hasPlugin(name){
            name = name.toLowerCase();
            for (var i = 0 ; i < navigator.plugins.length ; i++) {
                if (navigator.plugins[i].name.toLowerCase().indexOf(name) > -1) {
                    return true;
                }
            }
            return false;
        }
    插件检测：
        IE中的插件检测：IE不支持Netscape，检测IE插件的唯一方式就是使用专门的ActiveXObject类型（这个类型可以去操作系统的文档，比如去创建文档等等，这是一个很有趣的专有对象），在IE中是以COM对像的方式实现插件的，而COM对象使用唯一标识符来标识。所有检测插件需要知道这个插件的唯一标识符。
        eg：
        function hasIEPlugins(name) {
            try {
                new ActiveXObject(name);
                return true;
            } catch {
                return false;
            }
        }
        //这个函数只接受一个COM标识符参数，Flash插件的COM标识符是：'ShockwaveFlash.ShockwaveFlash'
        plugins 集合有一个名叫 refresh()的方法，用于刷新 plugins以反映最新安装的插件。这个方法接收一个参数：表示是否应该重新加载页面的一个布尔值。如果将这个值设置为 true，则会重新加载包含插件的所有页面；否则，只更新 plugins集合，不重新加载页面。

        注册处理程序：//这部分比较难懂  稍后详细研究。


##screen对象：
    screen对象基本上用来表明客户端的能力，其中包括浏览器窗口显示外部的显示器的信息，如像素宽度和高度等，每个浏览器中的screen对象包含着各不相同的信息。
    属性                              包含的值

    availHeight 屏幕的像素高度减系统部件高度之后的值（只读）    全支持
    availLeft 未被系统部件占用的最左侧的像素值（只读）  
    availTop 未被系统部件占用的最上方的像素值（只读）  
    availWidth 屏幕的像素宽度减系统部件宽度之后的值（只读）     全支持
    bufferDepth 读、写用于呈现屏外位图的位数 
    colorDepth 用于表现颜色的位数；多数系统都是32（只读）    
    deviceXDPI 屏幕实际的水平DPI（只读） 
    deviceYDPI 屏幕实际的垂直DPI（只读） 
    fontSmoothingEnabled
    表示是否启用了字体平滑（只读） 
    height 屏幕的像素高度                                       全支持
    left 当前屏幕距左边的像素距离 
    logicalXDPI 屏幕逻辑的水平DPI（只读） 
    logicalYDPI 屏幕逻辑的垂直DPI（只读） 
    pixelDepth 屏幕的位深（只读）   
    top 当前屏幕距上边的像素距离 
    updateInterval 读、写以毫秒表示的屏幕刷新时间间隔 
    width 屏幕的像素宽度                                        全支持


**  许多浏览器都会禁用调整浏览器窗口大小的能力


##history对象：
    history是window对象，每个浏览器每个标签页，每个w框架(frames)都有自己的history对象与特定的window相关联。

    go():它接受一个数值参数，这个方法可以在历史纪录里面随意的跳转。之前location对象里面的replace()方法无法生成history纪录，那么久无发使用go()去跳转，跳转按钮会被禁用掉。

    back():后退一页：
    forward():前进一页；
    length：页面历史纪录的长度，这个纪录包括所有的纪录（包括back()和forward()的纪录），对于加载到第一个页面而言，我们还是必须使用这个属性。

小结：
    每个框架(frames)都有自己的window属性，和window。history属性，而且每个框架都保存在frames集合中，我们可以通过对应的index去访问对应的frame；
    有一些窗口指针，用来指向其他的窗口，比如top，self，parent；
    top是最外围的框架，self是当前window本身，parent表示当前框架的父级框架。
    location可以访问到浏览器的属性，以及一些当前页面url信息。
    replace()方法可以跳转url，并且不会被保存到history中去
    navigator对象提供了浏览器信息，不过差异很大，但是还是有一些属性，比如userAgent等 //需要去查找资料发现那些事公用的














