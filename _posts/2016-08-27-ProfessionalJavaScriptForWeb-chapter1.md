---
layout:  post
title: javascript高级程序设计-chapter1-简介
tags: javascript高级程序设计  ReadingNotes
categories: javascript高级程序设计
---

* TOC
{:toc}

## JavaScript实现
-    JavaScript的实现包含三个部分：核心（ECMAScript），文档对象模型（DOM）,浏览器对象模型（BOM）。
-    ECMAScript由ECMA-262定义的标准。ECMAScript定义的知识JavaScript这门语言的基本规范：语法，类型，语句，关键字，保留字，操作符，对象。

## ECMAScript：
    ECMA-262定义的的ECMAscript与浏览器没有关系。它并没有定义输出和输入，它定义的只是这门语言的基础，而在此基础上可以构建更完善的语言脚本。浏览器只是JavaScript这门语言的宿主环境之一。宿主环境不仅提供基本的JavaScript实现，也会提供语言的扩展，以便于与环境之间进行交互。DOM就是属于这种扩展。
    ECMAscript大致规定了以下内容：
        语法
        类型
        语句
        关键字
        保留字
        操作符
        对象

## ECMA兼容：

    支持 ECMA-262 描述的所有“类型、值、对象、属性、函数以及程序句法和语义”（ECMA-262
    支持 Unicode 字符标准。
    此外，兼容的实现还可以进行下列扩展。
    添加 ECMA-262 没有描述的“更多类型、值、对象、属性和函数”。 ECMA-262 所说的这些新增
    特性，主要是指该标准中没有规定的新对象和对象的新属性。
    支持 ECMA-262 没有定义的“程序和正则表达式语法”。（也就是说，可以修改和扩展内置的正
    则表达式语法。）

## 文档对象模型（DOM document object model）：

    DOM 可以将任何 HTML 或 XML 文档描绘成一个由多层节点构成的结构，DOM把整个页面映射为一个多层节点结构。我们通过DOM  API进行操作（补充下这些API的东西，不是很理解。），这些操作包括删除，增加，修改节点。

### DOM1级

    包含两个部分内容。DOM core和DOM HTML
    在DOM core中，规定如何映射xml的文档结构。以便于简化对文档任意部分的访问和操作。这个核心里面也支持XML标签。
    在DOM html中，对DOM进行了扩展，添加了针对于HTML的对象和方法。比如getElementById等。

### DOM2级

    引入了众多新类型和新接口。主要分为4个方面：DOM视图，DOM事件，DOM样式，DOM遍历和范围。
    在DOM视图中，定义了不同文档的视图的接口。在2级视图中，AbstractView定义了一个文档的抽象视图，所有的文档视图都是基于这个视图来展现的。DocumentView 基于AbstractView,DocumentView可以被js，样式，等修改。
    在DOM事件中，定义了事件和事件处理的接口。比如onclick事件；
    在DOM样式中，定义了基于css为元素应用样式的接口。
    DOM 遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口。

### DOM3级

    引入了DOMLoad and save和DOM Validation
    DOMLoad and save中定义了加载和保存模块的方法；
    DOM Validation定义了验证文档的方法。
    并且DOM3级对DOM核心进行了扩展，开始支持XML1.0，包含了XML infoset，XPath，XML Base。

### Web浏览器对DOM的支持

    IE5.5引用DOM1，IE6和IE7没有引入DOM，IE8对DOM实现进行了修复。
    Netscape在Netscape…6（Mozilla 0.6.0）开始支持DOM，在Netscape7之后，mozilla把重心放到了firefox上面。并且firefox3完全支持DOM1级，几乎支持DOM2级，甚至部分支持DOM3级
    下面是各个浏览器对DOM的支持情况：
         浏览器                                        DOM兼容性
         Netscape 6+（mozilla 0.6.0）               1级，2级（几乎全部），3级（部分）
         IE2~IE4                                    不支持
         IE5                                        1级（最小限度，几乎不支持）
         IE5.5 到IE8                                1级（几乎全部）
         IE9+                                       1级，2级，3级
         opera 1~6                                  不支持
         opera 7~8x                                 1级（几乎全部），2级（部分）
         opera 9~9.9                                1级，2级（几乎全部）,3级（部分）
         opera 10+                                  1级，2级，3级（部分）
         safari 1.0                                 1级
         safari 2+                                  1级，2级（部分）
         chrome 1+                                  1级，2级（部分）
         firefox 1+                                 1级，2级（几乎全部），3级（部分）

## BOM 浏览器对象模型

    开发人员可以通过BOM控制浏览器显示页面以外的部分。但是通过BOM操作浏览器的部分却没有相应的规范。这个问题在h5中得到了解决。html5把bom功能写入了正式的规范。下面这些算是一些BOM的扩展。
        弹出新浏览器窗口的功能
        移动，缩放，和关闭浏览器窗口的功能。
        提供浏览器详细的navigator对象，这个对象里面包含了一些浏览器信息。
        提供浏览器所加载页面的详细信息的location对象。
        提供用户显示器分辨率详细信息的screen对象。
        对cookies的支持。
        像XMLHttpRequest和IE的ActiveXobject这样的自定义对象。
    因为浏览器版本的多样性，浏览器之前对BOM的操作有些细微的差异。在H5中，我们能够通过js更好的去操作BOM。