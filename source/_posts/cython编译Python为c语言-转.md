---
title: cython编译Python为c语言-转
date: 2020-03-14 22:08:37
tags: python
---
转自 - cython编译Python为c语言[源博客](https://www.cnblogs.com/zhangjxblog/p/12168360.html)
---
![image](https://images.pexels.com/photos/3902882/pexels-photo-3902882.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

#### 方法一：
---
 - 执行命令：cython test.py
 - 结果：会在同一目录下面生成test.c文件
 - 执行命令： gcc -c -fPIC -I /usr/include/python2.7 test.c
 - 结果： 在同一目录下面生成test.o文件
 - 执行命令： gcc -shared test.o -c test.so
 - 结果： 在同一目录下面生成test.so文件
 - 最后，生成的test.so文件就是需要的文件


#### 方法二：
---
```
[setup.py]
from distutils.core import setup
from Cython.Build import cythonize

setup(
    name = "test",
    ext_modules = cythonize("test.py")
    )
```
 - 执行命令： python setup.py build_ext --inplace


#### 第二种办法是对单独文件进行编译，下面介绍一种批量的办法：
---
```
#-*- coding:utf-8 -*-_
import os
import re 
from distutils.core import Extension, setup
from Cython.Build import cythonize
from Cython.Compiler import Options
 
# __file__ 含有魔术变量的应当排除，Cython虽有个编译参数，但只能设置静态。
exclude_so = ['__init__.py', 'run.py']
sources = 'backend'
 
 
extensions = []
remove_files = []
for source,dirnames,files in os.walk(sources):
    for dirpath, foldernames, filenames in os.walk(source):
        if 'test' in dirpath:
            break;
        for filename in filter(lambda x: re.match(r'.*[.]py$', x), filenames):
            file_path = os.path.join(dirpath, filename)
            if filename not in exclude_so:
                extensions.append(
                        Extension(file_path[:-3].replace('/', '.'), [file_path], extra_compile_args = ["-Os", "-g0"],
                                  extra_link_args = ["-Wl,--strip-all"]))
                remove_files.append(file_path[:-3]+'.py')
                remove_files.append(file_path[:-3]+'.pyc')

Options.docstrings = False
compiler_directives = {'optimize.unpack_method_calls': False, 'always_allow_keywords': True}
setup(  
        # cythonize的exclude全路径匹配，不灵活，不如在上一步排除。
        ext_modules = cythonize(extensions, exclude = None, nthreads = 20, quiet = True, build_dir = './build',
                                language_level = 2, compiler_directives = compiler_directives))

# 删除py和pyc文件
for remove_file in remove_files:

    if os.path.exists(remove_file):
        os.remove(remove_file)
```

- 执行命令： python setup.py build_ext --inplace
- 结果：最后生成.so文件，删除中间结果。
- 重点提一下，在编译flask代码时，遇到问题，报错：参数不够（大体意思是这样，错误未截图），在compiler_directives中添加： {always_allow_keywords:True}

