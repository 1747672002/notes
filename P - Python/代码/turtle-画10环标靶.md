使用`turtle`模块画一个简单的10环标靶
```python
#-*- coding:utf-8 -*-

import turtle as tur

# 初始化画图窗口大小和位置
tur.setup(500, 500, 200, 200)

# 循环执行画圈
for i in range(1,11):
    # 设置每圈之间的间隔
    fd_num = 15;

    # 计算每次递增后圈的半径
    num = i * fd_num

    # 画圈
    tur.circle(num)

    # 移动画笔位置
    if i < 10:
        # 画笔抬起
        tur.up()
        # 调整画笔方向
        tur.seth(-90)
        # 移动画笔到指定位置
        tur.fd(fd_num)
        # 重新调整画笔方向
        tur.seth(0)
        # 画笔放下
        tur.down()

```
