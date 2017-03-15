---

title: 如何改变sumlime 3的图标
date: 2017-03-13 22:39:21
---
### 首先需要两个工具: 
* ResEdit http://pan.baidu.com/share/link?uk=3030058&shareid=429117308
* Axialis IconWorkshop http://www.skycn.com/soft/appid/6671.html
<!-- more -->
### 开始
#### 我们打开ResEdit 点击File-Open project 选择你**安装包**里的那个sublime_txt.exe文件(不是你桌面的那个).
![](/demo/icon.png) 
在右侧我们可以看出这个ico文件包含了三个尺寸的ico,分别是16*16,32*32,48*48
![](/demo/icon2.png) 
#### 我们去 http://www.easyicon.net/ 这个网站找图标,下载这三个尺寸的,后缀名是.ico
#### 打开Axialis IconWorkshop,点击来自数个图像的window图标![](/demo/icon3.png) 然后选择所有文件 ![](/demo/icon4.png).把刚才下载好的那三个尺寸的ico文件加进去.最后保存下就算合成为一个ico文件了.
#### 最后打开ResEdit 选择icon右键-Add Resource-icon-确定. 然后找到我们刚才合成的那个ico文件.
![](/demo/icon5.png)
#### 删除原来 icon下sublime自带的那个icon.