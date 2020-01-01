---
title: matplotlib中文显示
date: 2019-06-04 15:14:12
tags: python
---

#### 方法一

```
from pylab import * 
import matplotlib
matplotlib.rcParams['font.family'] = 'Microsoft YaHei'
mpl.rcParams['font.sans-serif'] = ['Microsoft YaHei'] #更新字体格式
mpl.rcParams['font.size'] = 9 
```

#### 方法二

```
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif']=['SimHei'] #用来正常显示中文标签
plt.rcParams['axes.unicode_minus']=False #用来正常显示负号

```

#### 方法三

```

#pyplot并不默认支持中文显示，需要rcParams修改字体来实现
#rcParams的属性：
#‘font.family’ 用于显示字体的名字
#‘font.style’ 字体风格，正常’normal’ 或斜体’italic’
#‘font.size’ 字体大小，整数字号或者’large’ ‘x-small’

import matplotlib
matplotlib.rcParams[‘font.family’] = ‘STSong’
matplotlib.rcParams[‘font.size’] = 20
设定绘制区域的全部字体变成 华文仿宋，字体大小为20
```

#### 方法四 

自己下载字体，进行引入操作。

```
import matplotlib.font_manager as fm
fonts_msyh = fm.FontProperties(fname='./msyh.ttc')  # 字体位置

# 对于绘图中中文出现的不同位置，可以有以下的解决方案：
#图例中出现中文：

plt.legend(prop=fonts_msyh)

# title和横纵坐标等地方出现中文:

plt.xlabel('性别',fontproperties=fonts_msyh)
plt.ylabel('人数',fontproperties=fonts_msyh)
plt.title('直方图',fontproperties=fonts_msyh)
plt.xticks( (0,1),('男','女') ,fontproperties=fonts_msyh)
```

---

> 重要：在修改后应该清理matplotlib 缓存，rm rf  ~./matplotlib/


