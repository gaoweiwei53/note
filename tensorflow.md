# 1 人工智能三大学派

 1. 行为主义
 2. 符号主义
 3. 连接主义：模仿神经元连接
 # 2 数据类型
 - tf.int, tf.float
	* tf.int32, tf.float32, tf.float64
- tf.bool
- tf.string
 # 3 张量生成
 - 创建一个张量
 **tf.constant(内容，dtype=)**
 - 将numpy数据类型转换为Tensor数据类型
 **tf.convert_to_tensor(数据名，dtype=)**
 - **tf.zeros(维度)**
 创建全为0的张量
 - **tf.ones()**
 创建全为1的张量
 - **tf.fill(维度，指定值)**
 创建全为指定值的张量
 - **tf.random.normal(维度，mean=均值， stddev=标准差)**
 生成正太分布的随机数，默认均值为0，标准差为1
 - **tf.random.truncated_normal(维度，mean=均值， stddev=标准差)**
 - **tf.random.uniform(维度，mincal=最小值， maxval= 最大值)**
 生成均匀分布随机数
 # 4 常用函数
 - **tf.cast(张量名， dtype= 数据类型)**
 强制tensor转换为该数据类型
 - **tf.reduce_min(张量名)**
 计算张量维度上的最小值
 - **tf.reduce_max(张量名)**
 计算张量维度上的最大值
 - **tf.reduce_mean(张量名， axis=操作轴)**
 计算张量沿着指定维度的平均值
 - **tf.reduce_sum(张量名， axis=操作轴)**
 计算张量沿着指定维度的和
 - **tf.Variable**
 将变量标记为可训练，被标记的变量会在反向传播中记录梯度信息。神经网络训练中，常用该函数标记训练参数
 - 四则运算：**tf.add, tf.subtract, tf.multiply, tf.divide**
 只有维度相同的张量才能进行四则运算
 - 平方，次方，开方：**tf.square, tf.pow, tf.sqrt**
 - 矩阵乘：**tf.matmul**
 - **tf.data.Dataset.from_tensor_slices**
 切分传入张量的第一维度，生成输入特征/标签对，构建数据集
 `data = tf.data.Dataset.from_tensor_slices((输入特征,标签))`
 - **tf.GradientTape**
`with`结构记录计算过程，gradient求出张量的梯度
```
with tf.GradientTape() as tape:
	若干个计算过程
grad = tape.gradient(函数, 对谁求导)
```
 example:
 ```
 with tf.GradientTape() as tape:
	 w = tf.Variable(tf.constant(3.0))
	 loss = tf.pow(w,2)
	 grad = tape.gradient(loss,w)
	 print(grad)
 ```
 - **enumerate**
 - **tf.one_hot**
 tf.one_hot(待转换数据， depth=几分类)
 - **tf.nn.softmax** 
 - **assign_sub**
 常用于参数的自更新，更新参数并返回，调用assign_sub之前，先用tf.Variable定义变量为可训练。
 - **tf.argmax** 返回张量延指定维度最大值的索引
 tf.argmax(张量名， axis =  操作轴)
 
 
 
