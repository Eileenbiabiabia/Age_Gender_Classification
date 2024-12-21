# 项目架构
|——————————————code————-——————————————Step1_EDA.ipynb 数据预处理文件 
|                      |
|                      |——————————————Step2_training.ipynb 训练文件(训练resnet/mobilenetv3)
|                      |
|                      |——————————————Step3_mobilenetv3_training_only_age_5fold.ipynb 训练文件(只训练mobilenetv3进行年龄预测，五折交叉验证)
|                      |
|                      |——————————————Step4_testing.ipynb 测试文件(从网上找图片测试)
|                      |
|                      |——————————————附_准确率提取.ipynb （可视化用）
|
|——————————————log——————-——————————————fold0_train_log.txt     //五折训练数据
|                       |                      
|                       |——————————————fold1_train_log.txt          
|                       |                      
|                       |——————————————fold2_train_log.txt          
|                       |                      
|                       |——————————————fold3_train_log.txt          
|                       |                      
|                       |——————————————fold4_train_log.txt  
|
|—————————————README.md //项目说明

# 项目说明：
本项目的数据集是Audience

运行环境：pytorch+python 3.8

使用网络:resnet50 mobilenetv3(https://github.com/rwightman/pytorch-image-models)

分类器：将年龄分为8类，性别分为3类

运行步骤：
1.首先运行Step1_EDA.ipynb文件，进行数据预处理。统计数据基本信息，将图片进行分类，生成label.json文件
  年龄分类：0-2, 3-6, 8-13, 15-20, 22-32, 34-43, 45-53, 55-100（共8类）

2.运行Step2_training.ipynb
  先不采用五折交叉验证，随机划分验证集占比0.1
  1）在训练前调用albumentations进行了快速数据增强，效果良好

  2）在resnet50网络上训练，batchsize=8,
    最终结果gender acc 92.66%, age acc 57.45%，运行时间：60min，FLOP:85188107,参数量:24821467
    训练精度低于公开值，改进方向是训练更多epoch可能结果会更高；也可以调大batch_size值，原batch_size=256,因电脑内存限制，故只让batchsize=8

  3）还在resnet50上训练网络，改变训练策略为one-cycle,令batch_size=16，
    最终结果为 epoch 9, gender acc 95.12%, age acc 68.27%，运行时间:53min
    因为超出范围，只跑了9个epoch，但从结果来看，与上一个比精度显著提高。此方法的参数量与上一个相同。

  4）在mobilenetv3上进行训练，batch_size=16，
     最终结果为gender acc 96.68%, age acc 82.28%,运行时间48min，FLOP:6415043,参数量:3688163
     此网络结果最好，且参数量和运行时间都较少
  
  5）目前结果mobilenetv3上的结果最好,精度最高，参数量最少，可以尝试在这个网络上面做主要修改

3.运行Step3_mobilenetv3_training_only_age_5fold.ipynb 
  1）删除对于性别的预测
  2）只微调mobilenet_v3网络
  3）采用五折交叉验证

4.运行Step4_testing.ipynb，测试网络鲁棒性

#Written by Ruoyu Wu 2022-12-18





