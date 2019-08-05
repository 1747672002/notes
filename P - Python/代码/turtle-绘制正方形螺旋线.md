绘制一个正方形的螺旋线

```python
#-*- coding:utf-8 -*-

import turtle as tur

# 初始化画图窗口大小和位置
tur.setup(500, 500, 200, 200)

for i in range(0,20):
    num = 5
    a = num * (i+(i+1))
    b = num * (i+(i+2))

    tur.seth(90)
    tur.fd(a)
    tur.seth(180)
    tur.fd(a)
    tur.seth(-90)
    tur.fd(b)
    tur.seth(0)
    tur.fd(b)
    

'''
初始值
5
轮数  运行规律
 1    5 5 10 10
 2    15 15 20 20
 3    25 25 30 30
 4    35 35 40 40
 5    45 45 50 50
'''

```
