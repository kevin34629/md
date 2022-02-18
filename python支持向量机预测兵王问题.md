# 问题描述

> 国际象棋中，一方一个王一个兵，另一方一个王，用SVM来解决是和局还是将死的结果

# 数据预处理

数据量为28056

* 原始数据片段

  ~~~
  d,4,h,7,g,7,draw
  d,4,h,7,h,6,draw
  d,4,h,7,h,8,draw
  d,4,h,8,g,7,draw
  d,4,h,8,h,7,draw
  c,1,a,3,a,1,zero
  c,1,a,4,a,1,zero
  c,1,a,5,a,1,zero
  c,1,a,6,a,1,zero
  ~~~

  前面6个字段代表三个旗子的坐标，最后一个字段代表最终的情况，其中draw代表和局，其余代表将死所要的理论步数。由于SVM无法对字符串进行计算，所以所有的训练和测试数据都需要转化为**数值**。

* 处理后的数据片段

  ~~~
  4,4,8,7,7,7,1
  4,4,8,7,8,6,1
  4,4,8,7,8,8,1
  4,4,8,8,7,7,1
  4,4,8,8,8,7,1
  3,1,1,3,1,1,-1
  3,1,1,4,1,1,-1
  3,1,1,5,1,1,-1
  3,1,1,6,1,1,-1
  ~~~

  其中此处用使用**1**作为**和局**的标签，**-1**作为**将死**的标签

# 数据导入

> 导入数据到SVM时需保证其为**矩阵**
>
> 此处使用**pandas**模块中的**DdtaFrame**

~~~python
data=pd.read_csv('./krkopt.data',header=None)	#header=None为表中没有列索引
data.dropna(inplace=True)	#inplace=True为就地执行修改，不创建副本
~~~

# 数据归一化

>对数据进行归一化有助于提高训练速度和准确度

~~~python
ss=StandardScaler() #创建归一化容器
ss.fit(data.iloc[:,:6]) #适配容器，此处只适配前六个字段，及不对标签进行适配
~~~

其中StandarScale的公式为

**x<sub>i-new</sub>=$\frac{x_i-avg(x)}{\delta^2}$**

等效于

~~~python
for i in range(5):
    data[i] = (data[i]-data[i].mean())/data[i].std() 
~~~

> **通过归一化训练的SVM在进行预测时需使用同一个容器对数据进行处理**，因此需要对适配后的归一化容器进行保存

~~~python
file=open('.\StandardScaler.container','wb+')
pickle.dump(ss,file)    #保存容器
file.close()
~~~

> 大多数时候并不需要对标签进行归一化，且**<font color=#F00001>对标签进行归一化大多数时候对准确率有负影响</font>**，但在本次中仍然对标签进行归一化

~~~python
data=pd.DataFrame(ss.fit_transform(data))   #适配并计算
~~~

# 拆分数据表

~~~python
x_t, x, y_t, y = model_selection.train_test_split(data.iloc[:,:6], data[6].astype(int), test_size=0.8) #拆分数据，其中80%的数据用于训练
~~~

# 训练SVM

## 创建SVM

> 由于输入数据为坐标，其在计算中权重相等，因此创建时采用balanced模式，同时使用在大多数情况下都表现优良的高斯核函数

~~~python
svc = svm.SVC(kernel='rbf', class_weight='balanced')	#rbf：高斯核函数，balanced平衡权重
~~~

## 调参

> 训练过程即为对SVM超平面公式中中**C**及**gamma**两个参数进行求解
>
> 研究表明**C**与**gamma**的取值区间分别为**[$2^{-5}$,$2^{15}$]**及**[$2^{-15}$,$2^{3}$]**
>
> 在实际计算中采用网格搜索及交叉验证的方式进行求解
>
> 其中**C**的搜索梯度为$2^2$，**gamma**的梯度为2
>
> 在交叉验证中，折数越大，训练时间越长，准确率越高

~~~python
C_range=np.logspace(-5,15,11,base=2)    #参数预设参数C，构造从2的-5次方到2的15次方且共11项的等比数列
gamma_range=np.logspace(-15,3,19,base=2)
param_grid=[{'kernel':['rbf'],'C':C_range,'gamma':gamma_range}] #定义调参网格
gird=GridSearchCV(svc, param_grid, cv=5)    #5折交叉验证
clf=gird.fit(x_t,y_t)   #搜索最优参数
~~~

# 测试通过率

~~~python
score=gird.score(x,y)	#该方法返回测试结果的正确率
print(score)
~~~

在本次测试中，通过率为99.3%

# 保存训练好的模型

~~~python
joblib.dump(clf,'./program.svm')
~~~

# SVM及容器的调用

~~~python
file=open('.\program.svm','rb')
model=joblib.load(file) #加载svm
file.close()
file=open('.\StandardScaler.container','rb')
container=pickle.load(file) #加载容器
file.close()

list=[
[4,4,8,7,8,8],
[4,4,8,8,7,7],
[4,4,8,8,8,7],
[3,1,1,3,1,1],
[3,1,1,4,1,1],
[3,1,1,5,1,1],
[3,1,1,6,1,1]]  #测试数据

data=pd.DataFrame(list) #转换为矩阵
lable=container.transform(data) #处理数据
lable=pd.DataFrame(lable)   #transform的返回值并不是一个矩阵，因此通过该方法转换回矩阵
result=model.predict(lable) #启动预测
print(result)
~~~