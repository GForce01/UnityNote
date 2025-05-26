### 使用Quaternion进行旋转运算
使用Quaternion进行旋转的运算为**相乘**，且Quaternion乘法为**右乘**，即从右向左依次应用。
由于在多个轴向都存在旋转时旋转顺序十分重要因此***四元数乘法不满足交换律！！！***
例如Q1\*Q2就是指先应用Q2旋转，再应用Q1旋转。
https://discussions.unity.com/t/quaternion-multiplication-order/119632

而想要去除特定旋转的方式则为：
 Quaternion2.inverse\* Quaternion1

在Quaternion旋转中，两个点lerp结果会是一道直线，slerp则是弧线

