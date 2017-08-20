---
layout: post
title: 阿里巴巴在线编程题
---
周末的时候做了下阿里巴巴招聘的在线编程题目，题目挺有意思的，当时没有做完，这里重新做一下。
##### 题目描述
首先题目是这样的，网页中有如下所示的信息：
```
1 张三 男 13788888888 浙江 删除
2 李四 女 13788887777 四川 删除
3 王二 男 13788889999 广东 删除
```
根据面向对象的思想，完成点击“删除”后将相应的条目删除掉。不可以使用第三方库，JavaScript需使用ES6。
``` html
<div id="J_container">
    <div class="record-head">
      <div class="head id">序号</div><div class="head name">姓名</div><div class="head sex">性别</div><div class="head tel">电话号码</div><div class="head province">省份</div><div class="head">操作</div>
    </div>
    <ul id="J_List">
      <li><div class="id">1</div><div class="name">张三</div><div class="sex">男</div><div class="tel">13788888888</div><div class="province">浙江</div><div class="user-delete">删除</div></li>
      <li><div class="id">2</div><div class="name">李四</div><div class="sex">女</div><div class="tel">13788887777</div><div class="province">四川</div><div class="user-delete">删除</div></li>
      <li><div class="id">3</div><div class="name">王二</div><div class="sex">男</div><div class="tel">13788889999</div><div class="province">广东</div><div class="user-delete">删除</div></li>
    </ul>
</div>
```
##### 思路分析
首先获取到这三行，整体包装成一个对象，给该对象添加删除方法，将删除方法绑定给“删除”的点击事件。
##### 获取元素
如果可以使用jquery，可以使用选择函数`$`来进行选取元素。在客户端中，包含三种类，Window表示浏览器窗口，Document表示窗口中的文档，Element表示文档元素，也就是表示一对标签以及它们之间的内容。其中window,document分别表示当前页面的窗口对象和文档对象。而Element对象则可以使用document的`getElementById`方法来获得。另外还有几个方法`getElementsByTagNameNS(ns,name)`,`getElementsByTagName()`,`getElementsByName()`,`getElementsByClassName()`则会返回一个`NodeList`对象，也就是节点列表，由Element对象组成的一个列表。看上面的代码中故意给出了两个id，应该是给出了提示，这里选择id为`J_List`的ul元素是比较合适的，`document.getElementById("J_List")`返回一个ul节点。
下面需要遍历该ul节点，Element对象有一个用于遍历子节点的属性是`childNodes`，该属性将当前节点的子元素以只读数组的形式返回。在这里当使用了该属性后返回的是一个长度为7的数组，除了包含三个li节点之外，还包含了4个Text，也就是文本对象，这是因为在文档当中不仅包含了标签元素，还包含了它们之间的文本。如果想要忽略这些文本对象的话，可以使用`children`。
获得了三个节点后，给这三个节点中的包含"删除"字符的这一块添加点击事件，点击之后删除该行。到这里基本上整个程序就完成了。这里在调用数组的`forEach`方法时，遇到了一点问题，然后总结了下。
`Array.forEach(list,callbackfn)`函数作为可以直接调用，第一个参数是列表，第二个参数是调用函数。`Array.forEach.call(obj,list,callbackfn)`如果是这样调用的话，第一个参数传入的是对象，第二个是列表，第三个是调用函数，其实这里第一个参数没什么用。而`Array.prototype.forEach(callbackfn)`则为原型中的方法，这里没有传入列表参数，因为传入进行调用的列表参数就是当前对象。就像这样`[1,2,3].forEach(callbackfn)`。如果要调用该函数的话需要这样调用，`Array.prototype.forEach.call(list,callbackfn)`，第一个参数是表示上下文对象，也就是需要调用函数的列表。另外还有一种调用方式是这样的，`[].forEach.call(list,callbackfn)`，这里的调用和第一种调用不同，这是调用了一个对象方法，然后修改了该对象，使用了实际需要使用的list代替了该对象。
下面是最终的代码：

``` javascript
var J_List_Array = document.getElementById("J_List").children;
Array.forEach(J_List_Array,function(li_node){
    li_node.lastChild.onclick = function(){
        this.parentNode.remove();
    }
});
```