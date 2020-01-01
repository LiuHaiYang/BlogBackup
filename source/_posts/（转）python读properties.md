---
title: （转）python读properties
date: 2018-11-28 21:49:36
tags: python
---
#### 随记

今天开发中遇到这样的问题，因为公司规定不能直接获取数据库密码，在自动部署过程中会把数据库密码文件自动部署到项目的根目录下，但是是一个表文件（其实这个也是自己按要求解谜生成的），以前读取配置信息文件都是`.py` `.ini` ... 但就是没这个文件，Java中记得读去过，比较好读，但是python还真没写过，然后搜了很多，有很多的水货，最后尝试这个还是可用的，再次分享记录下来。

`Util.py`

	class Properties(object):
 
    def __init__(self, fileName):
        self.fileName = fileName
        self.properties = {}
 
    def __getDict(self,strName,dictName,value):
 
        if(strName.find('.')>0):
            k = strName.split('.')[0]
            dictName.setdefault(k,{})
            return self.__getDict(strName[len(k)+1:],dictName[k],value)
        else:
            dictName[strName] = value
            return
    def getProperties(self):
        try:
            pro_file = open(self.fileName, 'Ur')
            for line in pro_file.readlines():
                line = line.strip().replace('\n', '')
                if line.find("#")!=-1:
                    line=line[0:line.find('#')]
                if line.find('=') > 0:
                    strs = line.split('=')
                    strs[1]= line[len(strs[0])+1:]
                    self.__getDict(strs[0].strip(),self.properties,strs[1].strip())
        except Exception, e:
            raise e
        else:
            pro_file.close()
        return self.properties

`important.properties`：

	host=0.0.0.0
	port=3306
	user=root
	...

`read_p.py`

	from Util import Properties
	dictProperties=Properties("filename.properties").getProperties()
	print(dictProperties)
	print(dictProperties['host'])
	#读取到的是字典格式，按字典的方法获取对应的值

---
##### 每日一图
![image](https://images.pexels.com/photos/1047540/pexels-photo-1047540.jpeg)


