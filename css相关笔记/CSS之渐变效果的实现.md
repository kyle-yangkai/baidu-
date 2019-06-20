**注：**测试浏览器版本号——chrome 75.0.3770.80；opera 60.0.3255.109；firefox 67.0；ie 11。

之前《CSS之图片特殊用法》有讲过不同用途的image属性，渐变作为image的属性值参与以上用途。下面主要讲一讲渐变的实际用法。
## 一 线性渐变（linear-gradient)
#### 1 使用direction控制渐变方向
语法：
```
background-image: linear-gradient(to direction, color-1  percentage-1/length-1,color-2  percentage-2/length-2,...); 
```
**解释说明：**
1.to direction表示的是渐变的方向，可以省略不写，若省略，则默认从上到下；direction可以是单个方向参数如top/bottom/left/right，也可以是两个方向参数如bottom right，其表示从左上角到右下角渐变。
2.后面color参数至少有两个。
3. length参数表示当前颜色中心线位置，百分比percentage参数也表示当前颜色中心线的位置，可以省略，若省略则按照下图规则。

![颜色渐变规则.jpg](https://upload-images.jianshu.io/upload_images/13112949-906cfee20daf5a0f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
以上图为例，五种颜色将空间均分成四等份，其中五条分割线为各自颜色的中心线，以此为基准颜色渐变。
下面给出几个具体案例
```
/*方向缺省，百分比缺省，渐变从上到下，五种颜色按规则渐变*/
background-image: linear-gradient(red, yellow,blue,orange,black);
/*从左到右，按照百分比渐变*/
background-image: linear-gradient(to right, red 10%, yellow 30%,blue 70%,orange  90%,black);
/*从左上到右下，按照具体像素渐变*/
background-image: linear-gradient(to bottom right, red 10px, yellow 150px,blue 170px,orange  200px,black);
```
#### 2 使用angle控制渐变方向
语法
```
background-image: linear-gradient(angle, color-1  percentage-1/length-1,color-2  percentage-2/length-2,...); 
```
与上述（使用direction控制渐变方向）渐变方式唯一的区别就是该方法是使用角度（angle）控制方向，下图可以看出其渐变规则。当角度为0deg时，渐变方向从下到上；当角度是90deg时，渐变方向从左到右。

![image.png](https://upload-images.jianshu.io/upload_images/13112949-a14ba0af8c4a7638.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 二 径向渐变（radial-gradient)
径向渐变由中心向外进行颜色渐变。
语法
```
background-image: radial-gradient(size shape, color-1  percentage-1/length-1, color-2  percentage-2/length-2, ...); 
```
**解释说明：**
1. size参数定义渐变的大小，值可以是closest-side | farthest-side | closest-corner | farthest-corner | <length>。
<length>可以去取单值，表示渐变形状为圆形（circle）且渐变半径为该值；也可以取双值，表示渐变形状为椭圆（ellipse）且其值依次为渐变的横轴半径和纵轴半径。
该值也可以缺省，其默认值与shape有关，当shape为circle时，其默认值为farthest-corner；当shape为ellipse时，其默认值与容器尺寸相关，其横轴半径为容器宽的一半，纵轴半径为容器高的一半。
2. shape参数定义渐变的形状，值可以是circe | ellipse。
该值也可以缺省，默认值为ellipse。
3. 渐变中心点的位置可以自定义，语法为`circle at 20px 50px`或者`ellipse at 20% 50%`
## 三 重复渐变（repeating-linear-gradient或者repeating-radial-gradient)
语法
```
/*重复线性渐变*/
background-image: repeating-linear-gradient(to direction, color-1  percentage-1/length-1,color-2  percentage-2/length-2,...); 
/*重复径向渐变*/
background-image: repeating-radial-gradient(size shape, color-1  percentage-1/length-1, color-2  percentage-2/length-2, ...); 
```
**解释说明：**
1. 只需加`repeating-`即可变成重复渐变，其他参数没有变化。
2. 最后一种颜色后必须加`percentage/length`参数，且其小于容器尺寸，否则没有重复渐变。

## 四 边框渐变
上面以背景为例讲述了渐变的分类及实现，下面简单扩展一下，讲一讲边框的渐变，之前写过一篇文章《CSS之图片特殊用法》，里面有讲到将图片作为边框的背景，渐变神色与上述情况类似，只需要将图片地址改为渐变色就可以了，示例代码如下
```
border-image: linear-gradient( blue ,green ,yellow ,black) 10; 
```

## 五 字体颜色渐变
同样由《CSS之图片特殊用法》可以延伸，要是字体颜色渐变，只需要将背景图改为渐变色就可以了，具体代码如下：
```
background-image:radial-gradient( blue 10px,green 20px,yellow ,black); 
background-clip: text;
-webkit-background-clip: text;  /*兼容chrome*/
color: transparent; 
```
**注：**background-clip有兼容性问题，此需要根据浏览器不同使用不同的代码；text属性值目前是实验性值，且其在ie中无效，不建议在生产环境中使用。