# 自定义View

自定义View绘制流程函数调用链

![](https://pic1.zhimg.com/80/b6014c0c4ded87613bbc9ff34b08d88b_1440w.jpg)

继承View，并重写相关回调验证：

```
onMeasure: 
onSizeChanged: 
onLayout: 
onDraw: 
```

当父布局是LinearLayout时，onMeasure执行两次；
当父布局是RelativeLayout时，onMeasure执行了四次；


## 贝塞尔曲线

明确使用贝塞尔曲线场景：

| 序号 | 释义 | 变化 |
| ---- | ---- | ---- | 
| 1 | 事先不知道曲线状态，需要实时计算时 | 天气预报气温变化的平滑折线图 |
| 2 | 显示状态会根据用户操作改变时 | QQ小红点，仿真书翻页效果 |
| 3 | 一些比较复杂的运动状态（配合PathMeasure使用） | 复杂运动状态的动画效果 |  

### 绘制流程

