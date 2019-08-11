# mouse事件如何优化使其在手机端可以运行
  由于触碰手机都没有鼠标，所以无法对mouse的一系列指令做出反应，如
 ```JavaScript
  mousedown;mouseup;mousemove 等
  ```
所以必须进行一些列的优化才可以然这些指令在手机上运行。
步骤主要分为两个：


1. 找到手机上对应的指令，并命名一个新的指令，使其可以在手机与电脑端运行

2. 如果涉及坐标的代码，由于手机上的坐标与电脑的不同，还需要再进行优化

## 1.命名新的指令
在手机端与电脑端，有mouse（电脑）和touch（手机）常用的指令对应如下

```js
mousedown = touchstart
mouseup = touchend
mousemove = touchmove 
```
这里以mousedown和touchstart为例，先命名一个新的两个指令把这两个包括进去：
```js
let DownOrStart= ('ontouchstart' in window )? 'touchstart':'mousedown'
```
  这时候新的指令就命名好了，里面有个很关键的点很容易出bug，就是引号内的指令，里面不能有任何空格，不然会识别不了。
  接下来就可以把指令导入函数了：
```js
Element.addEventListener(DownOrStart,(e) =>{})
```
这时候就完成第一步了。

## 2.改作标的表达
电脑端的X、Y为：

* X=e.clientX
* Y=e.clientY

但在手机端中，是不同的，所以我们应该运用个短路的指令，使其根据客户端的不同二选一 :
```js
X=e.clientX||e.targetTouches(0).clientX
Y=e.clientY||e.targetTouches(0).clientY
```
要提醒的是，有关于坐标的地方，都需要进行这个优化。
