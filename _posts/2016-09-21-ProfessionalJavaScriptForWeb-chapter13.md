---
layout:  post
title: javascript高级程序设计-chapter13-事件
tags: javascript高级程序设计  ReadingNotes
categories: javascript高级程序设计
---


* TOC
{:toc}



## 本章内容

- 理解事件流
- 使用事件处理程序
- 不同的事件类型

<br/>

## 事件流

**1、事件冒泡**

-  1.IE的事件流叫做事件冒泡
-  2.由具体元素到document元素。
-  3.现在几乎所有的浏览器都支持事件冒泡。

**2、事件捕获**
- 1.由document到具体元素。
- 2.老版本的浏览器不支持，因此很少有人使用事件捕获。我们也建议读者放心地使用事件冒泡，在有特殊需要时再使用事件捕获。

**3、DOM事件流**
- DOM2级事件规定事件冒泡分为3个阶段，事件捕获阶段，处于目标阶段，事件冒泡阶段。首先是事件捕获为截取事件提供机会，然后是目标接收到事件，最后在冒泡阶段对事件作出响应。即使“ DOM2 级事件”规范明确要求捕获阶段不会涉及事件目标，但 IE9、 Safari、 Chrome、 Firefox 和 Opera 9.5 及更高版本都会在捕获阶段触发事件对象上的事件。结果，就是有两个机会在目标对象上面操作事件。

<br>
## 事件处理程序


**1、html事件处理程序**

- 在html中绑定事件。这样子不推介

```html
        <a href="" onclick="function(){console.log(this)}"></a>
```


**2、DOM0级处理程序**
```javascript
        var btn = document.getElementById("myBtn");
            btn.onclick = function(){
            alert(this.id); //"myBtn"
        };
```
这种方法绑定的事件处理程序，会在冒泡阶段被处理。可以用btn.onclick = null，来取消绑定事件。
多次绑定相同的事件会被后面的覆盖掉，这也是为什么可以用btn.onclick = null;来取消绑定事件的原因。



**3、DOM2级处理程序**

- 2级定义了2个方法。addEventListener() 和 removeEventListener()。所有的DOM节点都包含这2个方法。他们接受3个参数，处理的事件名;处理的函数;一个布尔值，如果这个值为true，表示在捕获阶段调用处理函数，为false，表示在冒泡阶段调用事件处理函数。

- 在这两个事件中处理函数为匿名函数，则无法被移除。

```javascript
    demo:
        var btn = document.getElementById("myBtn");
            btn.addEventListener('click', function() {
                console.log(this); //this 指向的是btn
            }, false);

```



**4、IE事件处理程序**

- attachEvent(),detachEvent();这两个方法接受相同的两个参数，名称，和处理函数。由于ie8以及更早版本只支持时间冒泡，所有通过这两个时间添加的函数都会被加到冒泡阶段。

- **注意：**ie中的事件名称需要有on！

```javascript
        var EventUtil = {
            addHandler: function(element, type, handler){
                if (element.addEventListener){
                    element.addEventListener(type, handler, false);
                } else if (element.attachEvent){
                    element.attachEvent("on" + type, handler);
                } else {
                    element["on" + type] = handler;
                }
            },
            removeHandler: function(element, type, handler){
                if (element.removeEventListener){
                    element.removeEventListener(type, handler, false);
                } else if (element.detachEvent){
                    element.detachEvent("on" + type, handler);
                } else {
                    element["on" + type] = null;
                }
            }
        };
```

## 事件对象

在触发 DOM 上的某个事件时，会产生一个事件对象 event，这个对象中包含着所有与事件有关的信息。包括导致事件的元素、事件的类型以及其他与特定事件相关的信息。

**1、DOM中的事件对象**

- DOM会把事件对象Event 传入到事件处理函数中去，无论使用的是DOM0级还是DOM3级。
- 下面是几个比较常用的事件属性




| 属性/方法           |   类 型         | 读/写    |                  说       明                                                  |
| -------------       |:--------:       | :---:    |     :-----------------------------------------------------------------        |
| preventDefault()    | Function        | 只读     |     取 消 事 件 的 默 认 行 为 。 如 果 cancelable是true，则可以使用这个方法  |
| target              | Element         | 只读     |     事件的目标                                                                |
| type                | String          | 只读     |     被触发的事件的类型                                                        |
| stopPropagation()   | Function        | 只读     |     取消事件的进一步捕获或冒泡。如果bubbles为true，则可以使用这个方法         |
| detail              | Integer         | 只读     |     与事件相关的细节信息                                                      |
| currentTarget       | Element         | 只读     |     其事件处理程序当前正在处理事件的那个元素                                  |

在事件处理程序内部，对象 this 始终等于 currentTarget 的值，而 target 则只包含事件的实
际目标。如果直接将事件处理程序指定给了目标元素，则 this、 currentTarget 和 target 包含相同
的值。

```javascript

    var btn = document.getElementById("myBtn");
    btn.onclick = function(event){
        alert(event.currentTarget === this); //true
        alert(event.target === this); //true
    };

```

由于 click 事件的目标是按钮，因此这三个值是相等的。如果事件处理程序存在于按钮的父节点中（例如 document.body），那么这些值是不相同的。再看下面的例子。

```javascript

    document.body.onclick = function(event){
        alert(event.currentTarget === document.body); //true
        alert(this === document.body); //true
        alert(event.target === document.getElementById("myBtn")); //true  指的上面例子中的btn
    };

```
当单击这个例子中的按钮时， this 和 currentTarget 都等于 document.body，因为事件处理程序是注册到这个元素上的。然而， target 元素却等于按钮元素，因为它是 click 事件真正的目标。由于按钮上并没有注册事件处理程序，结果 click 事件就冒泡到了 document.body，在那里事件才得到了处理。



**理解：**
注册和执行是不一样的，在一个事件里面，有完整的事件流，在这个事件流的过程中，我们在不同的阶段可以注册不同的事件处理函数，当事件流传播到这个事件注册的位置时，事件处理函数就会执行，但是stopPropagation()可以阻止事件流的向后传递，当我们使用stopPropagation()后，后面的事件流就会被截断。

**1、IE中的事件对象**

IE中的事件对象和DOM中的事件对象不同，分为以下几个方面：

1. 1、在使用DOM0级添加事件的时候

IE的事件对象是一个window对象的一个属性存在。

```javascript

    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
        var event = window.event;
        alert(event.type); //"click"
    };

```
**注：**IE事件type属性和DOM事件的type属性是相同的

2. 2、使用attachEvent()，detachEvent()来添加或者删除事件

- 可以在window中访问事件（跟DOM0级一样的访问方式）
- 可以在绑定事件中传入一个event事件，跟addEventListener一样的模式

```javascript

    var btn = document.getElementById("myBtn");
    btn.attachEvent('onclick', function(event) { //可以这么使用

        console.log(window.event.type) //也可以这么访问

    })

```

3. 3、HTML事件绑定

- 和html事件绑定一样，在html元素中直接传入event事件

```javascript

   <input type="button" value="Click Me" onclick="alert(event.type)">

```

1. 4、事件属性

事件属性有很大的差别，但是有下面的几个属性，所有的事件对象都包含

| 属性/方法           |   类 型         | 读/写    |                  说       明                                                  |
| -------------       |:--------:       | :---:    |     :-----------------------------------------------------------------        |
| cancelBubble        | Boolean         | 读/写    |     默认值为false，但将其设置为true就可以取消事件冒泡（与DOM中的stopPropagation()方法的作用相同）  |
| returnValue         | Boolean         | 读/写    |     默认值为true，但将其设置为false就可以取消事件的默认行为（与
DOM中的preventDefault()方法的作用相同）                                                                |
| srcElement          | Element         | 只读     |     事件的目标（与DOM中的target属性相同）                                                        |
| type                | String          | 只读     |     被触发的事件的类型         |


区别：cancelBubble 只能去除事件冒泡，因为IE不支持事件捕获，但是stopPropagatioin()可以同时取消事件捕获和事件冒泡。

1.  5、跨浏览器的事件对象


```javascript

var EventUtil = {
    addHandler: function(element, type, handler){
    //省略的代码
    },
    getEvent: function(event){
        return event ? event : window.event;
    },
    getTarget: function(event){
        return event.target || event.srcElement;
    },
    preventDefault: function(event){
        if (event.preventDefault){
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },
    removeHandler: function(element, type, handler){
        //省略的代码
    },
    stopPropagation: function(event){
        if (event.stopPropagation){
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
};
```

## 事件类型

DOM3级规定了以下的事件类型
UI（User Interface，用户界面）事件，当用户与页面上的元素交互时触发；

- 焦点事件，当元素获得或失去焦点时触发；
- 鼠标事件，当用户通过鼠标在页面上执行操作时触发；
- 滚轮事件，当使用鼠标滚轮（或类似设备）时触发；
- 文本事件，当在文档中输入文本时触发；
- 键盘事件，当用户通过键盘在页面上执行操作时触发；
- 合成事件，当为 IME（Input Method Editor，输入法编辑器）输入字符时触发；
- 变动（mutation）事件，当底层 DOM 结构发生变化时触发。

h5页定制了一组事件，这里需要去查阅下资料

**支持：**DOM3 级事件模块在 DOM2 级事件模块基础上重新定义了这些事件，也添加了一些新事件。包括IE9 在内的所有主流浏览器都支持 DOM2 级事件。 IE9 也支持 DOM3 级事件。

**1、UI事件**

- load ：当页面完全加载后在 window 上面触发，当所有框架都加载完毕时在框架集上面触发，当图像加载完毕时在<img>元素上面触发，或者当嵌入的内容加载完毕时在<object>元素上面触发。

- unload：当页面完全卸载后在 window 上面触发，当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在<object>元素上面触发。

- abort：在用户停止下载过程时，如果嵌入的内容没有加载完，则在<object>元素上面触发。

- error ：当发生 JavaScript 错误时在 window 上面触发，当无法加载图像时在<img>元素上面触发，当无法加载嵌入内容时在<object>元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。第 17 章将继续讨论这个事件。
**比如：**在zepto中用来提示错误的时候，监听的errer事件；

- select：当用户选择文本框（\<input\>或\<texterea\>）中的一或多个字符时触发。第 14 章将继续讨论这个事件。

- resize：当窗口或框架的大小变化时在 window 或框架上面触发。

- scroll：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。 <body>元素中包含所加载页面的滚动条。

上面这些事件都属于DOM2级事件，要确定浏览器是否支持DOM2级事件，可以使用以下代码：
    var isSupported = document.implementation.hasFeature("HTMLEvents", "2.0");
    是否支持DOM3级，可以使用以下代码：
    var isSupported = document.implementation.hasFeature("UIEvent", "3.0");

**事件详解**

 1. load：

在window或者body中

window上的onload事件是当页面所有的元素都加载完毕之后触发的事件，本来在DOM2级中，只要求在document中实现这个事件，但是为了保证向后兼容性，各大浏览器都在window中实现了这个事件。我们推介使用这个事件的时候，尽量使用js的形式来实现。

在image中实现

图像元素也可以触发load事件，无论在DOM中的图像还是在HTML中的图像。
这里先说一下触发的机制，在图片进行加载的时候，我们需要有一个src属性，我这个src属性里面的东西加载完成的时候，就会触发load事件，那么在外部引用的js也有src属性，这个是否也可以触发load呢？在 IE9+、 Firefox、 Opera、 Chrome 和 Safari 3+及更高版本中， <script>元素也会触发 load 事件，以便开发人员确定动态加载的 JavaScript 文件是否加载完毕。这里需要注意的有2点 IE8不支持，还有动态加载的img和script元素，load事件必须在src赋值之前定义。


```
demo:script的load
EventUtil.addHandler(window, "load", function(){
    var script = document.createElement("script");
    EventUtil.addHandler(script, "load", function(event){
        alert("Loaded");
    });
    script.src = "example.js";
    document.body.appendChild(script);
});

demo：image的load

EventUtil.addHandler(window, "load", function(){
    var image = new Image();
    EventUtil.addHandler(image, "load", function(event){
        alert("Image loaded!");
    });
    image.src = "smile.gif";
});

```
这里的Image() 对象是在DOM0级实现的。

<br>
<br>
<b> 这里顺带说下load事件  defer属性，document.readyState ,DOMContentLoaded</b>

首先我们看下MDN中的解释：

DOMContentLoaded： 页面文档完全加载并解析完毕之后,会触发DOMContentLoaded事件，HTML文档不会等待样式文件,图片文件,子框架页面的加载(load事件可以用来检测HTML页面是否完全加载完毕(fully-loaded))。

document.readyState： 当document文档正在加载时,返回"loading"。当文档结束渲染但在加载内嵌资源时，返回"interactive"，并引发DOMContentLoaded事件。当文档加载完成时,返回"complete"，并引发load事件。

defer： 这个布尔属性定义该脚本是否会延迟到文档解析完毕后才执行。但因为这个功能还未被所有主流浏览器实施，开发人员不应假设脚本实际上都会被延迟执行。defer属性不应在没有src属性的脚本标签上使用。从Gecko 1.9.2开始, 没有src属性的脚本标签的defer属性会被忽略。但是在Gecko 1.9.1中，如果定义了defer属性，即使内嵌的脚本也会被延迟执行。

从MDN中已经得到了详细的解释，那么总结一下
load：文档加载完成时执行，包括js，image，css样式表。
defer :会使有src属性的元素，延迟到文档解析完毕后才执行，至于解析和加载时2个概念，千万别混了。
document.readyState： 这个是事件是在加载的时候的一个属性，其中比较重要的几种状态，加载时-loading；结束渲染但在加载内嵌资源-interactive并且这个时候引发内容加载事件DOMContentLoaded，以及最后的-complete表明内嵌资源加载完成。
所以load和DOMContentLoaded这两个事件不过使加载过程中不同状态触发的事件。其实它总共包含下面5总状态

   uninitialized（未初始化）：对象存在但尚未初始化。
 loading（正在加载）：对象正在加载数据。
 loaded（加载完毕）：对象加载数据完成。
 interactive（交互）：可以操作对象了，但还没有完全加载。
 complete（完成）：对象已经加载完毕。


下面看看各大框架的ready函数：

zepto 的ready函数

```

readyRE = /complete|loaded|interactive/;
...
...
ready: function(callback){
      // need to check if document.body exists for IE as that browser reports
      // document ready when it hasn't yet created the body element
      if (readyRE.test(document.readyState) && document.body) callback($)
      else document.addEventListener('DOMContentLoaded', function(){ callback($) }, false)
      return this
    },

```

jquery 的ready函数

```

// Catch cases where $(document).ready() is called
// after the browser event has already occurred.
// Support: IE <=9 - 10 only
// Older IE sometimes signals "interactive" too soon
if ( document.readyState === "complete" ||
    ( document.readyState !== "loading" && !document.documentElement.doScroll ) ) {

    // Handle it asynchronously to allow scripts the opportunity to delay ready
    window.setTimeout( jQuery.ready );

} else {

    // Use the handy event callback
    document.addEventListener( "DOMContentLoaded", completed );

    // A fallback to window.onload, that will always work
    window.addEventListener( "load", completed );
}
```

解释下 document.documentElement.doScroll 这个东西，这个属性存在，那么说明浏览器可以执行scroll，也表明dom加载完成。
用到的原理就是上面查到的这些。


 2. resize：

当一个浏览器窗口被调整到一个新的高度或者宽度时，会触发resize事件。这个事件在window上触发。
与其他发生在 window 上的事件类似，在兼容 DOM 的浏览器中，传入事件处理程序中的 event 对象有一个 target 属性，值为 document；

```
EventUtil.addHandler(window, "resize", function(event){
    alert("Resized");
});
```

3.scroll事件

它表示的是一种页面中相应元素的变化。
在混杂模式下，可以通过body元素的scrollLeft 和scrollTop来监听这个变化，而在标准模式下，除了safari了之外的所有浏览器都会通过html来反应这种变化。（safari依然根据body）

这里解释下混杂模式和标准模式。document.compatMode 这个属性可以用来判断时标准模式还是混杂模式。如果为混杂模式，那么他的指为BackCompat，如果为标准模式，那么它的指为CSS1Compat。

```
    EventUtil.addHandler(window, "scroll", function(event){
        if (document.compatMode == "CSS1Compat"){
            alert(document.documentElement.scrollTop);
        } else {
            alert(document.body.scrollTop);
        }
    });
```

scroll事件和resize事件一样，会被多次触发。

其实scroll函数会在滚动的时候触发速度特别快，如果进行DOM操作，会影响性能，其实较好的解决方案就是防抖动函数，利用setTimeout 来延时触发，当然还是不推荐在scroll中操作DOM节点。

下面这两个函数来自underscore.js 有些地方得引用没写出来，大家可以去网站查 https://github.com/jashkenas/underscore/blob/master/underscore.js

节流函数

```
// 简单的节流函数
function throttle(func, wait, mustRun) {
    var timeout,
        startTime = new Date();

    return function() {
        var context = this,
            args = arguments,
            curTime = new Date();

        clearTimeout(timeout);
        // 如果达到了规定的触发时间间隔，触发 handler
        if(curTime - startTime >= mustRun){
            func.apply(context,args);
            startTime = curTime;
        // 没达到触发间隔，重新设定定时器
        }else{
            timeout = setTimeout(func, wait);
        }
    };
};

```

防抖函数：

```
// Returns a function, that, as long as it continues to be invoked, will not
  // be triggered. The function will be called after it stops being called for
  // N milliseconds. If `immediate` is passed, trigger the function on the
  // leading edge, instead of the trailing.
  _.debounce = function(func, wait, immediate) {
    var timeout, result;

    var later = function(context, args) {
      timeout = null;
      if (args) result = func.apply(context, args);
    };

    var debounced = restArgs(function(args) {
      if (timeout) clearTimeout(timeout);
      if (immediate) {
        var callNow = !timeout;
        timeout = setTimeout(later, wait);
        if (callNow) result = func.apply(this, args);
      } else {
        timeout = _.delay(later, wait, this, args);
      }

      return result;
    });

    debounced.cancel = function() {
      clearTimeout(timeout);
      timeout = null;
    };

    return debounced;
  };
```


**2、焦点事件**
bulr   在元素失去焦点时触发。这个事件不会冒泡；所有浏览器都支持它。
focus

DOMFocusIn：在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但它冒泡。只有Opera 支持这个事件。 DOM3 级事件废弃了 DOMFocusIn，选择了 focusin。
DOMFocusOut：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。只有 Opera支持这个事件。 DOM3 级事件废弃了 DOMFocusOut，选择了 focusout。
focusin：在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但它冒泡。支持这个事件的浏览器有 IE5.5+、 Safari 5.1+、 Opera 11.5+和 Chrome。
focusout：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。支持这个事件的浏览器有 IE5.5+、 Safari 5.1+、 Opera 11.5+和 Chrome。

当焦点从页面中的一个元素移动到另一个元素，会依次触发下列事件：
(1) focusout 在失去焦点的元素上触发；
(2) focusin 在获得焦点的元素上触发；
(3) blur 在失去焦点的元素上触发；
(4) DOMFocusOut 在失去焦点的元素上触发；
(5) focus 在获得焦点的元素上触发；
(6) DOMFocusIn 在获得焦点的元素上触发。

检测事件支持
var isSupported = document.implementation.hasFeature("FocusEvent", "3.0");



**3、鼠标与轮滚事件**

DOM3级定义了9个鼠标事件



**4、键盘与文本事件**

**5、复合事件事件**

**6、变动事件**

**7、html5事件**

**8、设备事件**

**9、触摸与手势事件**






















