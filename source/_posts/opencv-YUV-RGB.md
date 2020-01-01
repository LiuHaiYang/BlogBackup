---
title: （转）opencv-YUV-RGB
date: 2018-10-30 10:48:21
tags: python
---

#### YUV --> RGB OpenCV（Python）

#### YUV
 - YUV是一种颜色编码方法
 - Y 分量表示颜色的亮度（luminance），单取出 Y 分量就是图像的灰度图；U、V 分量表示颜色色度或者浓度（Chrominance）
 - YUV 图像有两种编码格式：
	- 紧缩格式（packed formats）：Y、U、V 三通道像素值依次排列，即 Y0 U0 V0 Y1 U1 V1 ...
	- 平面格式（planar formats）：先排列 Y 分量的所有像素值，再排列 U 分量的，最后排列 V 分量的

#### Note：平面模式适合采样（subsample）
 - 由于人眼对于亮度的敏感度远大于色度，所以减少 U、V 分量并不会过多损害图像的质量，从而减少了数据量，达到压缩的目的
 - YUV444 表示图像完全采样，没有压缩，紧缩格式
 - YUV420p 表示图像 2:1 的水平取样，垂直 2：1 采样，即每 4 个 Y 分量对应一个 U、V 分量，平面模式

#### Note：YUV420p（有时简称为 YUV420） 是常见的图像格式，接下来进行的 YUV 和 RGB 的转换操作都是针对 420p 格式的

---

#### YUV 和 RGB 之间的转换公式如下：

	R = Y + 1.4075 * (V - 128)
	G = Y - 0.3455 * (U - 128) - (0.7169 * (V - 128))
	B = Y + 1.7790 * (U - 128)

	Y = R *  .299000 + G *  .587000 + B *  .114000
	U = R * -.168736 + G * -.331264 + B *  .500000 + 128
	V = R *  .500000 + G * -.418688 + B * -.081312 + 128

---
##### YUV ---> RGB:
 - python 读取YUV： `https://blog.csdn.net/fall221/article/details/8490818`

  		# -*- coding: utf-8 -*-
		"""
		Created on Thu Jan 10 10:48:00 2013
		@author: Chen Ming
		"""
		 
		from numpy import *
		import Image 
		screenLevels = 255.0
		         
		def yuv_import(filename,dims,numfrm,startfrm):
		    fp=open(filename,'rb')
		    blk_size = prod(dims) *3/2
		    fp.seek(blk_size*startfrm,0)
		    Y=[]
		    U=[]
		    V=[]
		    print dims[0]
		    print dims[1]
		    d00=dims[0]//2
		    d01=dims[1]//2
		    print d00
		    print d01
		    Yt=zeros((dims[0],dims[1]),uint8,'C')
		    Ut=zeros((d00,d01),uint8,'C')
		    Vt=zeros((d00,d01),uint8,'C')
		    for i in range(numfrm):
		        for m in range(dims[0]):
		            for n in range(dims[1]):
		                #print m,n
		                Yt[m,n]=ord(fp.read(1))
		        for m in range(d00):
		            for n in range(d01):
		                Ut[m,n]=ord(fp.read(1))
		        for m in range(d00):
		            for n in range(d01):
		                Vt[m,n]=ord(fp.read(1))
		        Y=Y+[Yt]
		        U=U+[Ut]
		        V=V+[Vt]
		    fp.close()
		    return (Y,U,V)
		if __name__ == '__main__':
		    data=yuv_import('E:\\new\\test\\ballroom\\ballroom_0.yuv',(480,640),1,0)
		    #print data
		    #im=array2image(array(data[0][0]))
		    YY=data[0][0]
		    print YY.shape
		    for m in range(2):
		        print m,': ', YY[m,:]
		 
		    im=Image.fromstring('L',(640,480),YY.tostring())
		    im.show()
		    im.save('f:\\a.jpg')

#### 完整代码

		# -*- coding: utf-8 -*-
		import cv2
		import numpy as np
		def read_YUV420(image_path, rows, cols):
		    """
		    读取YUV文件，解析为Y, U, V图像
		    :param image_path: YUV图像路径
		    :param rows: 给定高
		    :param cols: 给定宽
		    :return: 列表，[Y, U, V]
		    """
		    # create Y
		    gray = np.zeros((rows, cols), np.uint8)
		    print type(gray)
		    print gray.shape

		    # create U,V
		    img_U = np.zeros((rows / 2, cols / 2), np.uint8)
		    print type(img_U)
		    print img_U.shape

		    img_V = np.zeros((rows / 2, cols / 2), np.uint8)
		    print type(img_V)
		    print img_V.shape

		    with open(image_path, 'rb') as reader:
		        for i in xrange(rows):
		            for j in xrange(cols):
		                gray[i, j] = ord(reader.read(1))

		        for i in xrange(rows / 2):
		            for j in xrange(cols / 2):
		                img_U[i, j] = ord(reader.read(1))

		        for i in xrange(rows / 2):
		            for j in xrange(cols / 2):
		                img_V[i, j] = ord(reader.read(1))

		    return [gray, img_U, img_V]


		def merge_YUV2RGB_v1(Y, U, V):
		    """
		    转换YUV图像为RGB格式（放大U、V）
		    :param Y: Y分量图像
		    :param U: U分量图像
		    :param V: V分量图像
		    :return: RGB格式图像
		    """
		    # Y分量图像比U、V分量图像大一倍，想要合并3个分量，需要先放大U、V分量和Y分量一样大小
		    enlarge_U = cv2.resize(U, (0, 0), fx=2.0, fy=2.0, interpolation=cv2.INTER_CUBIC)
		    enlarge_V = cv2.resize(V, (0, 0), fx=2.0, fy=2.0, interpolation=cv2.INTER_CUBIC)

		    # 合并YUV3通道
		    img_YUV = cv2.merge([Y, enlarge_U, enlarge_V])

		    dst = cv2.cvtColor(img_YUV, cv2.COLOR_YUV2BGR)
		    return dst


		def merge_YUV2RGB_v2(Y, U, V):
		    """
		    转换YUV图像为RGB格式（缩小Y）
		    :param Y: Y分量图像
		    :param U: U分量图像
		    :param V: V分量图像
		    :return: RGB格式图像
		    """
		    rows, cols = Y.shape[:2]

		    # 先缩小Y分量，合并3通道，转换为RGB格式图像后，再放大至原来大小
		    shrink_Y = cv2.resize(Y, (cols / 2, rows / 2), interpolation=cv2.INTER_AREA)

		    # 合并YUV3通道
		    img_YUV = cv2.merge([shrink_Y, U, V])

		    dst = cv2.cvtColor(img_YUV, cv2.COLOR_YUV2BGR)
		    cv2.COLOR_YUV2BGR_I420

		    # 放大
		    enlarge_dst = cv2.resize(dst, (0, 0), fx=2.0, fy=2.0, interpolation=cv2.INTER_CUBIC)
		    return enlarge_dst


		if __name__ == '__main__':
		    rows = 480
		    cols = 640
		    image_path = 'C:\\yuv\\jpgimage1_image_640_480.yuv'
		    Y, U, V = read_YUV420(image_path, rows, cols)
		    dst = merge_YUV2RGB_v1(Y, U, V)
		    cv2.imshow("dst", dst)
		    cv2.waitKey(0)

