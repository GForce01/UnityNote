[https://blog.csdn.net/GG_and_DD/article/details/126917358](https://blog.csdn.net/GG_and_DD/article/details/126917358) 

[Unity有五种旋转方式](https://blog.csdn.net/zxy13826134783/article/details/79461816)

### 欧拉角——旋转角度
transform.eulerAngles = Vector3(x,y,z);
本地欧拉角
transform.localEulerAngles = Vector3(x,y,z);
unity欧拉角按照zxy的顺序分别旋转得到最终结果，因此旋转顺序不同结果不同

### Rotation
transform.Rotation
transform.localRotation
Rotation记录着物体的旋转信息，本身是四元数Quaternion形式要注意，平时不要打这个的主意，想要Slerp的时候会用到
Quaternion的记录方式为（x,y,z,w）[-1,1]

### Rotate
transform.Rotate
要正经旋转东西请用这个！！输入量为**角度**

### Quaternion与Rotation

有时需要用到一个Forward和一个Upward向量定义rotation

Z与Forward方向相同，X垂直于Forward与Upward组成的平面，最后Y垂直于ZX平面，因此Upward并不一定要与Forward垂直，XY方向根据右手螺旋定理判断

比如Transform.LookAT(Transform target, Vector3 WorldUp)中最终y轴会用上述逻辑尽量接近WorldUp


在Quaternion旋转中，两个点lerp结果会是一道直线，slerp则是弧线