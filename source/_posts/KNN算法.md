---
title: KNN算法
tags: 算法
abbrlink: 39151
date: 2017-05-28 09:09:06
---
k-Nearest Neighbors算法（简称KNN算法）
核心思想：“近朱者赤近墨者黑”，
由你的邻居推断出你的类别。
KNN计算步骤：①算距离 ②找邻居 ③做分类
什么是k-Nearest Neighbors？
   KNN(K Nearest Neighbors,K近邻)是一种基于实例的学习，通过计算新数据与训练数据特征值之间的距离，然后取K(K>=1,k为奇数)个距离最近的邻居进行分类判定（投票法）或者回归。
距离种类：欧氏距离，余弦距离，汉明距离，曼哈顿距离。
实例：KNN的模型是整个训练数据集。当我们需要预测一个新实例的某个属性A时，KNN算法会搜索训练数据集找到K个最相似的实例。这些相似实例的A属性会被总结归纳，作为新实例A属性的预测。
如何判断实例之间的相似程度，这需要具体看数据的类型。对于实数数据，可以使用欧式距离。对于分类或者二进制数据，可以使用汉明距离。
KNN算法是如何工作的？
KNN算法属于基于实例的、竞争学习与懒惰学习算法。
基于实例的算法：算法模型使用数据实例，并依据这些实例做出预测的算法。KNN算法是基于实例算法中的一个极端例子，因为所有的训练数据都成了算法模型的一部分。
竞争学习逻辑：它内部使用模型元素（数据实例）之间的竞争来作出预测。数据实例之间的相似性计算导致每一个数据实例都竞争去“赢”，或者说竞争去和需要预测的元素相似，再或者说竞争为预测结果做贡献。
懒惰学习算法：因为一直到需要进行预测操作了，它才会建立一个预测模型，它总是最后需要出结果的时候才开始干活。
KNN很有用，因为它不对数据做任何的假设，同时使用一致的规则在任意两个数据实例之间计算距离。也因为如此，它被称作非参数，或者非线性算法，因为它没有假设一个模型函数。
KNN的优点和缺点：
    算法优点：
          简单，易于理解，容易实现，通过对K的选择可具备丢噪音数据的健壮性。
     算法缺点：
          需要大量空间储存所有已知实例，算法复杂度高（需要比较所有已知实例与要分类的实例），当其样本分布不平衡时，比如其中一类样本过大（实例数量过多）占主导的时候，新的未知实例容易被归类为这个主导样本，因为这类样本实例的数量过大，但这个新的未知实例实际并木接近目标样本
 	改进版本
      考虑距离，根据距离加上权重，比如: 1/d (d: 距离）


如何用Python实现KNN（实现步骤分为如下6步）
1.	载入数据：①从TXT中读取数据，训练数据集和测试数据集。
		def loadDataset(trainfilename, testfilename ,length,trainingSet=[],testSet=[]):
			with open(trainfilename,'r') as csvfile:
				traininglines = csv.reader(csvfile)
				dataset = list(traininglines)
				for x in range(len(dataset)):
					for y in range(length):
						dataset[x][y] = float(dataset[x][y])
					trainingSet.append(dataset[x])
			with open(testfilename, 'r') as csvfile:
				testlines = csv.reader(csvfile)
				dataset = list(testlines)
				for x in range(len(dataset)):
					for y in range(length):
						dataset[x][y] = float(dataset[x][y])
					testSet.append(dataset[x])

②：从CSV中读取数据，训练数据集和测试数据集。
		def loaddata(trainfilename,testfilename,index):#数据载入

			traindata = pd.DataFrame(pd.read_csv(trainfilename,header=0,index_col=index ))
			testdata = pd.DataFrame(pd.read_excel(testfilename,header=0,index_col=index ))
			return traindata,testdata


2.	数据清洗：
		def dataclean(filename,index,label):
			data = loaddata(filename,index)
			#数据表中的重复值
			repetition=data.duplicated()
			# print(repetition)
			data.drop_duplicates()
			print("数据中重复值已删除")

			#数据表中的空值
			datanull = data[label].isnull().value_counts()
			# print(datanull)
			data.dropna()
			print("数据中空值已删除！")


			#数据间的空格
			data[label1] = data[label1].map(str.strip)

			#大小写转换
			data[label1] = data[label,].map(str.title)
			#检查数据中关键字段 是否统一
			#是否全为字母
			data[label1].apply(lambda x: x.isdigit())
			#是否全为数字
			#data[label].apply(lambda x: x.isalnum())
			return data

3.	数据属性可视化：
		def plt_all(filename,lable1,lable2):
			font=FontProperties(fname=r'c:\windows\fonts\simsun.ttc',size=14)
			fontt=FontProperties(fname=r'c:\windows\fonts\simsun.ttc',size=16)
			plt.title(u'属性展示: '+lable1+u'和 '+lable2,fontproperties=fontt)
			plt.figtext(0.5,0.03,lable1,fontproperties=font)
			plt.figtext(0.03,0.5,lable2,fontproperties=font)
			# data= pd.read_csv(filename,index_col=index)
			inde = filename.index
			ind = []
			for i in inde:
				ind.append(i)
			news_ids = []
			for id in ind:
				if id not in news_ids:
					news_ids.append(id)
			for k in news_ids:
				# m = data.ix[k]
				m = filename.ix[k]
				x = m[lable1]
				y =  m[lable2]
				if k == news_ids[0]:
					l = plt.scatter(x, y,marker='>',c='r')
					l.set_label(k)
				elif  k == news_ids[1]:
					l = plt.scatter(x, y,marker='+', c='blue')
					l.set_label(k)
				elif k == news_ids[2]:
					l = plt.scatter(x, y, marker='<',c='g')
					l.set_label(k)
				else:
					l.set_label(k)
			plt.legend(loc=2)
			plt.grid(color='#95a5a6', linestyle='--', linewidth=1, axis='both', alpha=0.8)
			plt.show()

4.	相似度：计算两个数据实例之间的距离，欧氏距离。
		#如何计算euclideanDistance
		def euclideanDistance(instancel, instance2, length):
			distance = 0
			for x in range(length):
				distance += pow((instancel[x] - instance2[x]),2)
			return math.sqrt(distance)
5.	临近数据：确定最相近的N个实例。
		#返回最近的k个邻居
		def getNeighbors(trainingSet,testInstance,k):
			distances = []
			length = len(testInstance) -1
			for x  in  range(len(trainingSet)):
				dist = euclideanDistance(testInstance,trainingSet[x],length)
				distances.append((trainingSet[x],dist))
			distances.sort(key=operator.itemgetter(1))
			# plt.scatter(distances)

			neighbors = []
			for  x  in range(k):# 取前k 个距离
				neighbors.append(distances[x][0])
			return  neighbors
6.	结果：从这些实力中生成预测结果。
		#进行判断  分类  少数服从多数的法则
		def getResponse(neighbors):#最近的k个邻居
			classVotes = {}
			for x in range(len(neighbors)):
				response = neighbors[x][-1]
				if response in classVotes:
					classVotes[response] +=1
				else:
					classVotes[response] = 1
			sortedVotes = sorted(classVotes.items(),key=operator.itemgetter(1), reverse = True)
			return sortedVotes[0][0]#返回个数最多的是哪一个

##### 参考网站：
http://scikit-learn.org/stable/
http://www.tuicool.com/articles/aeuAZfV
https://zhuanlan.zhihu.com/p/23191325
http://www.tuicool.com/articles/Zjayiiu

