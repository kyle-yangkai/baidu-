**注：**测试浏览器版本号——chrome 75.0.3770.80；opera 60.0.3255.109；firefox 67.0；ie 11。

之前《CSS之渐变效果的实现》中有讲到边框颜色渐变，但是只有讲最普通的边框渐变，其作用于圆角边框渐变时会覆盖掉圆角的效果，这不是我们预期的，所以我们需要寻找其他的方法解决这个需求。
当盒子同时设置圆角（radius）和渐变时，圆角失效，因此不能用这种方式来实现圆角边框颜色渐变。我们可以使用下面三个方法实现
##### 1 使用背景重叠
在此之前我们先来看看三个跟背景有关的属性`background-origin`,`background-clip`,`background-size`。
`background-origin`表示的是背景起始位置，其三个值如下，依次是
 - border-box  从边框开始；
 - padding-box(默认)  从内边距开始；
 - content-box  从内容开始。
```
background-origin: border-box | padding-box(默认) | content-box
```
`background-clip`表示的是背景填充位置，其四个值如下，依次是
- border-box(默认)  填充至边框；
-  padding-box  填充至内边距；
- content-box  填充之内容；
-  text  作为字体前景色。
```
background-clip: border-box(默认) | padding-box | content-box | text
```
`background-size`表示的是背景尺寸，其五个值如下，依次是
- contain  将图像扩大至适应最短的边，剩余部分默认重复图像
- cover   将图像扩大至适应最长的边，图像可能显示不完整
- length  两个值依次设置图像宽和高，未设置则为auto
- percentage  两个百分比依次设置图像宽和高，未设置则为auto
- auto  默认设置
```
background-size: contain | cover | <length> | <percentage> | auto(默认)
```
以下面代码为例
```
div {
  width: 900px;
  height: 300px;
  margin: 10px;
  padding: 30px;
  border:50px solid transparent;
  background-origin:border-box;
  background-clip: content-box,padding-box, border-box;
  background-size: contain,50px 50px,cover;
  background-image:url("css.jpg"),linear-gradient(yellow, green),url("css.jpg");
}
```
效果如图

![image.png](https://upload-images.jianshu.io/upload_images/13112949-8307ec4de035a4f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由上面的例子我们可以看出：
1. background-image可以多次添加图片或者渐变，需要用","隔开按照添加顺序依次由上往下层叠，简单来讲就是谁先声明，谁层级高。
2. `background-origin`,`background-clip`,`background-size`同样可以设置多个值，用","隔开，每个值对应的是background-image的值。

有了上述的知识，我们现在可以实现我们的需求了，其主要原理是利用背景重叠，第一个背景设置范围为padding和content，第二个背景设置范围为border，padding和content，那么第二个背景只有border显示，其中padding和content被第一个背景覆盖。
话不多说，上代码
```
div {
  width: 900px;
  height: 300px;
  margin: 10px;
  padding: 30px;
  border-radius: 50px; /*设置圆角*/
  border:50px solid transparent;   /*设置边框颜色透明，确保背景渐变色显示*/
  background-origin:border-box;  /*从边框开始背景图*/
  background-clip: padding-box, border-box; /*设置第一个背景和第二个背景的范围*/
  background-size: cover;
  /*由于背景图像不能设置纯色，所以可以使用下面的方式设置纯色*/
  background-image:linear-gradient(#fff, #fff),linear-gradient(yellow, green); 
}
```
效果如图

![image.png](https://upload-images.jianshu.io/upload_images/13112949-a6f7c51f9800fce1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 2 使用伪元素
让我们先来看代码
```
div {
  width: 900px;
  height: 300px;
  margin: 10px;
  padding: 30px;
  border-radius: 50px; /*设置圆角*/
  border:50px solid transparent;   /*设置边框颜色透明，确保背景渐变色显示*/
  background-clip: padding-box;   /*确保此北京范围为内边距内*/
  background: #fff;
}
div::after {
  position: absolute;
  /*以div的content为基准往外扩border的宽度*/
  top: -50px; 
  bottom: -50px;				
  left: -50px;
  right: -50px;
  border-radius: 50px;
  /*设置伪元素背景渐变色*/
  background-image: linear-gradient(yellow, green);
  content: '';
  /*利用层叠将div部分背景置顶*/
  z-index: -1;  
}
```
效果如下图，与方法1中效果相同

![image.png](https://upload-images.jianshu.io/upload_images/13112949-16d9dc30e2ab8861.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 3 使用遮罩
使用遮罩，顾名思义就是在div外面加一层div，其大小正好比里面的div大border的宽度，通过外面div的背景渐变来模拟圆角边框渐变。
具体代码如下：
```
/*内部div样式*/
.inside {
  width: 960px;
  height: 360px;
  margin: 10px;
  padding: 0px;
  border-radius: 50px; /*设置圆角*/
  border:50px solid transparent;   /*设置边框颜色透明，确保背景渐变色显示*/
  background-origin:border-box;  /*从边框开始背景图*/
  background-image: linear-gradient(yellow, green);
}
/*外部div样式*/
.outside {
  background: #fff;
  width: calc(100% - 60px);
  height: calc(100% - 60px);
  padding: 30px;
}
```
效果如下图，与方法1中效果相同

![image.png](https://upload-images.jianshu.io/upload_images/13112949-e6f0724805f139a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**
在实验过程中有以下几点需要注意：
1. 属性`background-origin`,`background-clip`,`background-size`针对于` background-image`生效，如果使用`background`进行渐变色的设置可能会出现不符预期的效果。
2. 边框外侧有圆角而内部无圆角是因为边框宽度设置比较大，圆角又设置的比较小。有兴趣的可以自己实验一下