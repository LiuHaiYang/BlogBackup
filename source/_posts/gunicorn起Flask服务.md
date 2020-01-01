---
title: gunicorn起Flask服务
date: 2018-11-26 21:34:19
tags: python
---

在使用 gunicorn 启动服务时候有可能失败显示没安装库，
但是库也是装成功的，所以需要如下方法进行启动。
当时遇到这个问题，搜了好久才找到有个大神这样解决的，
今天分享下！
---
    import re
    import sys
    from gunicorn.app.wsgiapp import run
    if __name__ == '__main__':
        sys.argv[0] = re.sub(r'(-script\.pyw|\.exe)?$','',sys.argv[0])
        sys.exit(run())

![iamge](https://images.pexels.com/photos/1181671/pexels-photo-1181671.jpeg)