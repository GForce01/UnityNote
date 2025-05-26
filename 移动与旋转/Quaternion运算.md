### 使用Quaternion进行旋转运算
使用Quaternion进行旋转的运算为**相乘**
例如Q1\*Q2就是指在Q1基础上旋转Q2

而想要去除特定旋转的方式则为：
 Quaternion2.inverse\* Quaternion1

***四元数乘法不满足交换律！！！***


在Quaternion旋转中，两个点lerp结果会是一道直线，slerp则是弧线
