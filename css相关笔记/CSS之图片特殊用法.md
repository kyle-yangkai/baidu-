*前沿：说起image，大家肯定会想到的两个功能：1. 直接使用<img>标签插入到html中；2. 作为背景图使用，那么还有没有什么其他的“骚操作”呢？*
## 一 图片的常规用途
#### 1 作为元素插入html中
这是我们最常用的用途，语法也十分简单，如下：
```
<img src="图片地址">
```
#### 2 作为背景图
```
background-image: url("图片地址")
```
## 二 图片的特殊用途
下面我们来重点说一说图片的特殊用途
#### 1 作为边框背景
边框一般都使用纯色或者渐变色来作为背景色，很少有用图片做背景。虽然不常用，但不代表我们不需要知道，毕竟多一点知识准没错！！！
首先我们了解以下有关border-image的属性：
1.获取边框图片资源
```
border-image-source: url("图片地址")；
```
2.将边框图片分区
```
border-image-slice：<length> | <percentage>; 
```
这个属性时重点也是难点，我们需要特别注意。该属性其实就是将边框图片切割成9部分，如下图所示。然后属性值表示的是图中虚线的位置，即切割的位置。
![image.png](https://upload-images.jianshu.io/upload_images/13112949-5bd8ab4aba542e5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
与padding的属性取值一样，border-image-slice的取值顺序是上-右-下-左，其值可以为单值，也可以为多值。单值和多值所表示的含义与padding取值一样。举例如下
```
/* 所有的边 */
border-image-slice: 30%; 

/* 垂直方向 | 水平方向 */
border-image-slice: 10% 30%;

/* 顶部 | 水平方向 | 底部 */
border-image-slice: 30 30% 45;

/* 上 右 下 左 */
border-image-slice: 7 12 14 5;
```
取值为数值时，数值默认表示的是像素px，取值为百分比时，其相对对应的边去百分比。
划分好区域后，1-8板块分别对应边框相应的区域，然后根据选定的填充方式填充。
**注：如果border-image-slice的值中多了一个fill（fill放在任意位置都可以），其含义时上图的9部分作为该区域的背景图使用**

3.选择填充方式
```
border-image-repeat: stretch(默认) | repeat | round | space;  
```
上述代码中四个值的含义分别是：
- stretch：默认;侧面的图像被拉伸来填满边界。这通常看起来很糟糕和像素化，所以不推荐。
- repeat：边图像被重复，直到边界被填满。根据具体情况，这可能看起来不错，但您可能会看到一些难看的图像片段。
- round： 边的图像被重复，直到边界被填满，它们都被稍微拉伸，这样就不会出现碎片。
- space：边图像被重复，直到边界被填满，每个拷贝之间添加了少量的间隔，这样就不会出现任何片段。这个值只在Safari(9+)和Internet Explorer(11+)中得到支持。

通过以上3个属性我们就可以设置边框图了，当然跟设置背景图一样，我们也可以简写，示例代码如下：
```
border-image: url(border-image.png) 40 round;
```
**注意：**border-image还有两个不常用的属性：
```
border-image-outset: <length> | <percentage>; 
border-image-width: <length> | <percentage>; 
```
前者表示边框图像相对边框向外偏移的数值，取值方式同padding一样；后者表示边框图像的宽度，若border-image-width>border-width，则边框图像向内部扩展，其取值方式同padding一样。

#### 2 作为文本颜色
我知道文本颜色可以是纯色，也可以是渐变色，但是图片能充当文本颜色却是我之前的知识盲区。要实现上述效果，我们需要先了解一下background-clip属性及其属性值。background-clip表示元素的背景（背景图或者颜色）是否延伸到边框下面，可取的值有：
- border-box（背景延伸到边框外沿）， 
- padding-box（背景延伸到内边距外沿），
- content-box（背景只在内容区域）， 
- text（背景被剪裁成文字的前景色）。

到这里我们可以知道我们需要用的属性值为text。将图片作为文本颜色的代码为：
```
background-image: url("图片地址")
background-clip: text;
-webkit-background-clip: text;  /*兼容chrome*/
color: transparent; 
```
**注：**background-clip有兼容性问题，此需要根据浏览器不同使用不同的代码；text属性值目前是实验性值，不建议再生产环境中使用。

####3 作为列表符号背景
示例代码如下：
```
list-style-image: url("图片地址")
```
**注：**list-style-image会覆盖掉list-style-type；我们也尽量不要使用这个属性，因为其尺寸不能改变，不能自适应屏幕大小，可以使用背景替代。