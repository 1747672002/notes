绘制一个等边三角形，再在三角形的内部绘制一个小的等边三角形
```python
#-*- coding:utf-8 -*-

import turtle as tur

tur.setup(500, 500, 200, 200)

tur.fd(200)

tur.seth(120)

tur.fd(200)

tur.seth(240)

tur.fd(200)


# 画重叠的小等边三角形
tur.seth(60)

tur.fd(100)

tur.seth(0)

tur.fd(100)

tur.seth(240)

tur.fd(100)

tur.seth(120)

tur.fd(100)

```
