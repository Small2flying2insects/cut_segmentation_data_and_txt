# cut_segmentation_data_and_txt
最近做实例分割遇到一个问题：原始训练集样本长宽比差异太大（400*3000左右），即使开启矩形训练效果依然不尽人意。

在排除标记质量的前提下，理论上模型接受的图像大小640左右，所以写了一下图像处理的文件。
目前代码实现：将实例分割数据集按照x轴切割，同时处理对应的txt。对切割之后的图像根据窗宽窗位增强图像对比度。

本代码实验对象是：北京理工大学智能焊接团队开源的3675张标注焊缝X射线图像数据集---SWRD数据集。下载链接：https://pan.baidu.com/s/1oAMwLqcGmr7hPdMo0rl9qw?pwd=2ae8

处理之前的数据：
![image text](https://github.com/Small2flying2insects/cut_segmentation_data_and_txt/blob/main/%E5%8E%9F%E5%A7%8B%E5%9B%BE%E5%83%8F.png)

处理之后的图像：

![image text](https://github.com/Small2flying2insects/cut_segmentation_data_and_txt/blob/main/%E6%89%B9%E9%87%8F%E5%9B%BE%E5%83%8F.png)
![image text](https://github.com/Small2flying2insects/cut_segmentation_data_and_txt/blob/main/%E5%8D%95%E4%B8%80%E6%A0%B7%E6%9C%AC%E5%B1%95%E7%A4%BA.png)

思路如下：
需要考虑的问题
1、边缘标记的处理

处理方法：考虑当前点下一个点与边缘线的交叉情况。一共分为7小种（4大种）情况。

（1）无交点

（2）与切割图像两条线都有交点

（2.1）当前点在左边，下一个点在右边

（2.2）当前点在右边，下一个点在左边

（3）与切割图像左边缘线有交点

（3.1）当前点在边缘线左边，下一个点在边缘线右边

（3.2）当前点在边缘线右边，下一个点在边缘线左边

（4）与切割图像右边缘线有交点

（4.1）当前点在边缘线左边，下一个点在边缘线右边

（3.2）当前点在边缘线右边，下一个点在边缘线左边
2、怎么判断交点位置

处理方法：根据当前点和下一个点的坐标以及边缘线的x坐标求y坐标，当y的值在两个点之间时，即为有效值，否则无意义。

思路示意图:
![image text](https://github.com/Small2flying2insects/cut_segmentation_data_and_txt/blob/main/%E6%80%9D%E8%B7%AF.jpeg)
