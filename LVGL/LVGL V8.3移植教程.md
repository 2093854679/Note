# LVGL V8.3移植教程

## 一.源文件获取和裁剪

1. 到lvgl官网clone源文件

   https://github.com/lvgl/lvgl

2. 裁剪源文件

   只需要留下这三个文件夹和两个.C文件

   原文件名：“lv_conf_template.h”，修改为： "lv_conf.h";

   ![image-20241206194820888](D:\Desktop\Study_Note\LVGL\images\image-20241206194820888.png)

3. src源文件各个文件夹说明

## 二.添加lvgl文件到keil工程

1. keil新建文件夹

   打开kei工程添加四个文件夹

   <img src="D:\Desktop\Study_Note\LVGL\images\image-20241206200012494.png" alt="image-20241206200012494" style="zoom:50%;" />

2. 添加.c文件

   lvconf文件夹下添加

   ![image-20241206200433987](D:\Desktop\Study_Note\LVGL\images\image-20241206200433987.png)

   port文件夹下添加

   ![image-20241206200332410](D:\Desktop\Study_Note\LVGL\images\image-20241206200332410.png)

​		src文件夹说明

![image-20241206202033765](D:\Desktop\Study_Note\LVGL\images\image-20241206202033765.png)



font里面是用到的格式，先添加这三个，其他需要再添加

![image-20241206202344371](D:\Desktop\Study_Note\LVGL\images\image-20241206202344371.png)

报错，extra下面都是小组件，都打开添加一下

![image-20241206203141218](C:\Users\48527\AppData\Roaming\Typora\typora-user-images\image-20241206203141218.png)

extra里面的theme也要添加一下，以及draw里面的sw，和layout里面的文件

主要还是看报错，缺什么我们就去添加什么。

3.添加.h文件

![image-20241206204012720](D:\Desktop\Study_Note\LVGL\images\image-20241206204012720.png)

提示报错，编译器改成version 6

![image-20241206204107572](D:\Desktop\Study_Note\LVGL\images\image-20241206204107572.png)

## 三.注册显示和触摸驱动

1. 启用lvgl_conf.h

   定义屏幕的分辨率以及色彩宽度，RGB565为16位

   ![image-20241206204233376](D:\Desktop\Study_Note\LVGL\images\image-20241206204233376.png)

2. 启用lv_port_disp.h，并且修改

![image-20241206204442978](D:\Desktop\Study_Note\LVGL\images\image-20241206204442978.png)

![image-20241206204542171](D:\Desktop\Study_Note\LVGL\images\image-20241206204542171.png)

3.启用lv_port_indev.h，并且修改

![image-20241206204723371](D:\Desktop\Study_Note\LVGL\images\image-20241206204723371.png)

![image-20241206204836427](D:\Desktop\Study_Note\LVGL\images\image-20241206204836427.png)

## 四.VLGL初始化

![image-20241206205027403](D:\Desktop\Study_Note\LVGL\images\image-20241206205027403.png)



![image-20241206205145122](C:\Users\48527\AppData\Roaming\Typora\typora-user-images\image-20241206205145122.png)

![image-20241206205224301](D:\Desktop\Study_Note\LVGL\images\image-20241206205224301.png)

## 测试

![image-20241206205316703](D:\Desktop\Study_Note\LVGL\images\image-20241206205316703.png)
