---
title: python+opencv视频入门处理
date: 2018-10-24 14:46:21
tags: python
---

#### 工作中用到了opencv，使用其对视频的处理，把视频每一帧的处理为图片，然后使用项目中的AI接口对其进行处理，实现视频实时的人体关键点的处理和人脸识别的功能。初期延迟大，卡顿的较大，效果不是很好。 

---
环境： `Python3 opencv 3`

#### 入门代码记录

		# coding=utf-8
		import os
		import numpy as np
		from PIL import Image, ImageDraw
		import cv2
		import requests
		import json

		videoCapture = cv2.VideoCapture(0)
		fps = videoCapture.get(cv2.CAP_PROP_FPS)
		size = (int(videoCapture.get(cv2.CAP_PROP_FRAME_WIDTH)),
		        int(videoCapture.get(cv2.CAP_PROP_FRAME_HEIGHT)))
		fourcc = cv2.VideoWriter_fourcc('M','J','P','G')

		while 1:
		    ret, img = videoCapture.read()
		    # frame_resize = cv2.resize(img, (640, 360), interpolation=cv2.INTER_AREA)
		    frame_resize = cv2.resize(img, (size[0], size[1]), interpolation=cv2.INTER_AREA)
		    cv2.imwrite("", frame_resize)

		    # 鉴别
		    try:
		    	#AI 人脸识别接口
		        url = ''
		        data = {}

		        new_img = open("", 'rb')
		        f = {"image": new_img}
		        result = requests.post(url, files=f, data=data)
		        requestdata = result.json()
		        fontface = cv2.FONT_HERSHEY_SIMPLEX
		        fontscale = 1
		        fontcolor = (255, 255, 255)
		        #识别后的视频中文字说明
		        #接口中数据处理
		        for per_info in requestdata['resultInfo']:
		            if per_info['score'] > 0.6:
		                cv2.putText(img, 'hello world!', (
		                int(x), int(y)),fontface, fontscale, fontcolor)
		    except Exception as e:
		        pass
		    #人体关键点 同理 画线

		    cv2.imshow('video', img)
		    key = cv2.waitKey(1)
		    if key == ord('q'):
		        break
		videoCapture.release()
		cv2.destroyAllWindows()