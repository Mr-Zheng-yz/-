# 自定义View

## `Paint` 画笔🖌️

### Paint常用操作速查表
| 操作类型 | 相关api | 备注 | 
| ---- | ---- | ---- |
| 抗锯齿 | **setAntiAlias** (boolean aa) </br> **constructor** (Paint.ANTI_ALIAS_FLAG) | 开启抗锯齿，`Paint`构造方法开启抗锯齿 |
| 填充风格 | **setStyle** (Style style) | 填充风格：`FILL`，`STROKE`，`FILL_AND_STROKE` |
| 线条形状 | **setStrokeWidth** (float width) </br> **setStrokeCap** (Cap cap) </br> **setStrokeJoin** (Join join) </br> **setStrokeMiter** (float miter) | 设置线条宽度，设置线头形状，设置拐角形状，设置`MITER`型拐角延长线最大值 |
| 初始化 | **reset** () </br> **set** (Paint src) </br> **setFlage** (int flags) | 重置`Paint`属性，复制`Paint`所有属性，批量设置flags。 |
| 设置着色 | **setColor** (int color) </br> **setARGB** (int a,int r,int g,int b) </br> **setShader** (Shader shader) </br> **setColorFilter** (ColorFilter colorFilter) </br> **setXfermode** (Xfermode xfermode)| 设置颜色 设置着色器（Shader） 设置颜色过滤 设置颜色处理方案 |
| 色彩优化 | **setDither** (boolean dither) </br> **setFilterBitmap** (boolean filter) | 设置抖动来优化色彩深度降低时的绘制效果，设置双线性过滤来优化 Bitmap 放大绘制的效果 |
| 图形轮廓 | **setPathEffect** (PathEffect effect) | 给图形的轮廓设置效果：</br> 单一效果：`CornerPathEffect`，`DiscretePathEffect`，`DashPathEffect`，`PathDashPatheffect` </br> 组合效果：`SumPathEffect`，`ComposePathEffecct` |
| 绘制附加效果 | **setShadowLayer**(float radius,float dx,float dy,int shadowColor) </br> **setMaskFilter** (MaskFilter maskfilter) | `setShadowLayer`在之后的绘制内容下加一层阴影。`setMaskFilter`设置绘制层上方的附加效果。MaskFilter有两种：`BlurMaskFilter`(模糊)和`EmbossMaskFilter`(浮雕) |
| 文字相关 | **drawText** () </br> | Paint 超过一半的方法都是 drawText() 相关的 |
| 获取绘制的Path | **getFillPath** (Path src,Path dst) </br> **getTextPath** (String text, int start, int end, float x, float y, Path path)  </br> **getTextPath** (char[] text, int index, int count, float x, float y, Path path) | 根据 `paint` 的设置，计算出绘制 `Path` 或文字时的实际 `Path`。 |


### 线条形状

#### 设置线头形状 : setStrokeCap()

`Cap`为枚举类型，分别是：`BUFF`平头，`ROUND`圆头，`SQUARE`方头，默认为`BUFF`。

![](https://wx4.sinaimg.cn/large/006tNc79ly1fig74qv8rij30ct05rglp.jpg)

#### 设置拐角形状 : setStrokeJoin()

Join为枚举类型，有三个值可以选择：`MITER`尖角，`BEVEL平`角，`ROUND`圆角，默认为`MITER`。

![](https://wx1.sinaimg.cn/large/006tNc79ly1fig75e27w6j30cp05ewem.jpg)

#### 设置尖角补充延长线最大值 : setStrokeMiter()

是对于`setStrokeJoin()`方法的补充。`MITER` 型连接点有一个额外的规则：当尖角过长时，自动改用 BEVEL 的方式来渲染连接点。至于多尖算尖角，由`setStrokeMiter()`的`miter`参数决定。

![](https://wx3.sinaimg.cn/large/006tNc79ly1fig7btolhij30e706dglp.jpg)

尖角的外缘端点和内部拐角的距离与线条宽度的比。

### setShader() ：Shader 

Shader主要使用其几个子类：`LinearGradient`，`RadialGradient`，`SweepGradient`，`BitmapShader`，`ComposeShader`

### setColorFilter() ：ColorFilter

ColorFilter不直接使用，而是使用它的三个子类：`LightingColorFilter`，`PorterDuffColorFilter`，`ColorMatrixColorFilter`

### setXfermode() ：Xfermode


### 色彩优化

#### 设置图像的抖动 : setDither()

实际场景中，抖动更多的作用是在图像降低色彩深度绘制时，避免出现大片的色带与色块。

![](https://wx4.sinaimg.cn/large/006tNc79ly1fig7d34s0jj30lf07t75x.jpg)

#### 是否使用双线性过滤绘制 Bitmap : setFilterBitmap()

Android默认使用最近邻插哦旅算法绘制放大图像，这种算法简单，但会出现马赛克效果。`setFilterBitmao()`可以让图像在放大绘制时，显得更加平滑。

![](https://wx2.sinaimg.cn/large/006tNc79ly1fig7dbga6ij30jb0a00tr.jpg)

### 为图形的轮廓设置效果：setPathEffect(PathEffect effect)

`PathEffect`为图形轮廓设置效果，对`Canvas`所有图形都有效。Android
中有6种`PathEffect`，分为两类，

- 单一效果：`CornerPathEffect` `DiscretePathEffect` `DashPathEffect` `PathDashPathEffect`
- 组合效果：`SumPathEffect` `ComposePathEffect`

#### 1. 单一效果的：CornerPathEffect

把所有拐角变圆润。

**构造方法**：CornerPathEffect ( float radius ) </br>
`raduis`：圆角半径。

```
PathEffect pathEffect = new CornerPathEffect(20);
paint.setPathEffect(pathEffect);
...
canvas.drawPath(path,paint);
```
效果:
<img width=300 src="https://wx1.sinaimg.cn/large/006tNc79ly1fig7dobrizj30iv0agt8z.jpg"/>

#### 2. 单一效果的：DiscretePathEffect

把线条进行随机偏离(离散)，偏离程度由参数决定。

**构造方法**：DiscretePathEffect ( float segmentLength,float deviation ) </br>
`segmentLength`：用来拼接每个线段的长度。</br>
`deviation`：偏离量。

```
PathEffect pathEffect = new DiscretePathEffect(20,5);
...
```
效果：
<img width="300" src="https://wx2.sinaimg.cn/large/006tNc79ly1fig7dug01cj30ir0bawet.jpg"/>

#### 3. 单一效果的：DashPathEffect

使用虚线绘制线条。

**构造方法**：DashPathEffect ( float intervals[], float phase ) </br>
`intervals[]`：指定虚线的格式，元素必须为偶数（最少2个），按照「画线长度，空白长度，画线长度，空白长度...」顺序绘制。 </br>
`phase`：虚线的偏移量。

```
PathEffect pathEffect = new DashPathEffect(new float[]{20,10,5,10},0);
...
```
效果：
<img src="https://wx4.sinaimg.cn/large/006tNc79ly1fig7dz1jenj30iw0b9mxh.jpg" width ="300"/>

#### 4. 单一效果的：PathDashPathEffect

使用一个Path来绘制「虚线」。

**构造方法**：PathDashPathEffect(Path shape, float advance, float phase, Style style) </br>
`shape`：绘制的Path。</br>
`advice`：两个相邻shape之间的距离。</br>
`phase`：虚线的偏移。</br>
`style`：指定拐角处shape的转换方式。

`style`有三个枚举值：

- TRANSLATE：位移
- ROTATE：旋转
- MORPH：变体

<img width = "300" src="https://wx1.sinaimg.cn/large/006tNc79ly1fig7efqw9qj30kn0h3dh5.jpg"/>

```
Path dashPath = ...;
PathEffect pathEffect = new PathDashPathEffect(dashPath,40,0,PathDashPathEffectStyle.TRANSLATE);
...
```
效果：<img width = "300" src = "https://wx2.sinaimg.cn/large/006tNc79ly1fig7e4xp38j30j30bc0t7.jpg"/>

#### 5. 混合效果的：SumPathEffect

按照两种`PathEffect`分别对目标进行绘制。

**构造方法**：SumPathEffect ( PathEffect first, PathEffect second ) 

```
PathEffect dashEffect = new DashPathEffect(new float[]{20, 10}, 0);
PathEffect discreteEffect = new DiscretePathEffect(20, 5); 
pathEffect = new SumPathEffect(dashEffect, discreteEffect);...
```
效果：
<img width="300" src="https://wx1.sinaimg.cn/large/006tNc79ly1fig7ekjh7lj30dw05jq2z.jpg"/>

#### 6. 混合效果的：ComposePathEffect

先对目标`Path`使用一个`PathEffect`，然后对这个改变后的`Path`使用另一个`PathEffect`。

**构造方法**：ComposePathEffect ( PathEffect outerpe, PathEffect innerpe ) 

```
PathEffect dashEffect = new DashPathEffect(new float[]{20, 10}, 0);
PathEffect discreteEffect = new DiscretePathEffect(20, 5); 
pathEffect = new ComposePathEffect(dashEffect, discreteEffect);
```
效果：
<img width="300" src="https://wx3.sinaimg.cn/large/006tNc79ly1fig7epf94aj30dr05eq2x.jpg"/>

***注意***： `PathEffect` 在有些情况下不支持硬件加速，需要关闭硬件加速才能正常使用：

> 1. Canvas.drawLine() 和 Canvas.drawLines() 方法画直线时，setPathEffect() 是不支持硬件加速的；
2. PathDashPathEffect 对硬件加速的支持也有问题，所以当使用 PathDashPathEffect 的时候，最好也把硬件加速关了。

## `Canvas` 画布 📄

`canvas`可以称之为画布，是安卓平台2D图形绘制的基础，可以在上面绘制各种东西。

### `Canvas`常用方法速查表

| 操作类型 | 相关api | 备注 | 
| ---- | ---- | ---- |
| 绘制颜色 | **drawColor** (int color) </br> **drawColor** (int color, PorterDuff.Mode mode) </br> **drawRGB** (int r,int g,int b) </br> **drawARGB** (int a,int r,int g,int b) | 使用单一颜色填充画布 |
| 绘制形状 | `点：`</br>**drawPoint** (float x,float y,Paint paint) </br> **drawPoints** (float[] pts,Paint paint) </br> **drawPoints** (float[] pts,int offset,int count,Paint paint) </br> `offset:`跳过数组的前几个数，`count:`点的数量</br>`线：` </br> **drawLine** (float startX,float startY,float stopX,float stopY,Paint paint) </br> **drawLines** (float[] pts,Paint paint) </br> **drawLines** (float[] pts,int offset,int count,Paint paint)</br>`矩形：`</br> **drawRect** (Rect rect,Paint paint) </br> **drawRect** (RectF rect,Paint paint)</br> **drawRect** (float left,float top,float right,float bottom,Paint paint)</br>`圆角矩形：`</br>**drawRoundRect** (RectF rect,float ex,float ry,Paint paint)</br> **drawRoundRect** (float left, float top, float right, float bottom, float rx, float ry,Paint paint)</br>`椭圆：`</br> **drawOval** (RectF oval,Paint paint) </br> **drawOval** (float left,float top,float right,float bottom,Paint paint)</br>`圆：`</br> **drawCircle** (float cx, float cy, float radius,Paint paint)</br>`弧:`</br> **drawArc** (RectF oval,float startAngle,float sweepAngle,boolean useCenter,Paint paint)</br> **drawArc** (float left, float top, float right, float bottom, float startAngle, float sweepAngle, boolean useCenter,Paint paint) | 绘制基本图形 |
| 绘制图片 | **drawBitmap** (Bitmap bitmap,float left,float top,Paint paint) </br> **drawBitmap** (Bitmap bitmap,Rect src,RectF dst,Paint paint) </br>  **drawBitmap** (Bitmap bitmap,Rect src,Rect dst,Paint paint) </br> **drawBitmap** (Bitmap bitmap, Matrix matrix,Paint paint) </br> **drawPicture** (Picture picture) <br> **drawPicture** (Picture picture,RectF dst) </br> **drawPicture** (Picture picture,Rect dst) | 绘制位图和图片 |
| 绘制文本 | **drawText** (char[] text,int index,int count,float x,float y,Paint paint)</br>**drawText** (String text,float x,floaty,Paint paint)</br>**drawText** (String text,int star,int end,float x,float y,Paint paint) </br> **drawText** (CharSequence text,int start,int end,float x,float y,Paint paint) </br> **drawTextOnPath** (char[] text,int index,int count,Path path,float hOffset,float vOffset,Paint paint) </br> **drawTextOnPath** (String text,Path path,float hOffset,float vOffset,Paint paint) </br> ~~**drawPosText**~~ ( ... ) | 绘制文字，根据路径绘制文字，绘制文字时指定定每个文字的位置（已弃用） |
| 绘制路径 | **drawPath** (Path path,Paint paint) | 绘制路径，绘制贝塞尔曲线时也需要用到该函数 | 
| 顶点操作 | **drawVertices** (VertexMode mode,int vertexCount,float[] verts,int vertOffset,float[] texs, int texOffset,int[] colors,int colorOffset,short[] indices, int indexOffset, int indexCount,Paint paint) </br> **drawBitmapMesh** (Bitmap bitmap,int meshWidth,int meshHeight,float[] verts,int vertOffset,Paint paint) | 通过对顶点操作可以使图像形变，	`drawVertices`直接对画布作用，`drawBitmapMesh`只作用于绘制的Bitmap |
| 画布剪裁 | **clipPath** (Path path) </br> **clipRect** (Rect rect) </br> **clipRect** (RectF rect) </br> **clipRect** (int left, int top, int right, int bottom) </br> **clipRect** (float left, float top, float right, float bottom) </br> | 设置画布的显示区域 |
| 画布快照 | **save** () </br> **restore** () </br> **saveLayerXXX** ()  </br> **restoreToCount** (int saveCount) </br> **getSaveCount** () | 保存当前状态，会滚到上一次保存状态，保存图层状态，会滚到指定状态，获取保存次数 |
| 画布变换 | **translate** (float dx, float dy) </br> **scale** (float sx, float sy) </br> **scale** (float sx, float sy, float px, float py) </br> **rotate** (float degrees) </br> **rotate** (float degrees, float px, float py) </br> **skew** (float sx, float sy) | 位移、缩放、 旋转、错切 |
| Matrix(矩阵) | **getMatrix** () </br> **setMatrix** (Matrix matrix) </br> **concat** (Matrix matrix) | 实际上画布的位移，缩放等操作都是图像矩阵Matrix，只不过Matrix难以理解和使用，故封装了一些常用方法 |

### 圆角矩形：`drawRoundkRect()`

圆角矩形的绘制有两个重载方法：

```
// 第一种
RectF rectF = new RectF(100,100,800,400);
canvas.drawRoundRect(rectF,30,30,mPaint);

// 第二种 API21的时候才添加上(AndroidX貌似没有这个限制)
canvas.drawRoundRect(100,100,800,400,30,30,mPaint);
```

与矩形相比，圆角矩形多出来了两个参数`rx`和`ry`，分别表示椭圆的两个半径。

<img width="200" src="http://gcsblog.oss-cn-shanghai.aliyuncs.com/blog/2019-04-29-071439.jpg?gcssloop"/>

当`rx`大于矩形宽度的一半，`ry`大于矩形高度的一半时，就变成椭圆了。






