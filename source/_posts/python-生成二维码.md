---
title: python 生成属于自己网站的二维码
tags: python
abbrlink: 7936
date: 2017-10-01 21:42:06
---

### 安装Python库 qrcode

- `pip install qrcode`

- qrcode 依赖 Image 库  `pip  install Image`

  上代码

  ```
  import qrcode
  img = qrcode.make('https://liuhaiyang.github.io/')
  with open('ocean.png', 'wb') as f:
  img.save(f)
  ```

  ​


