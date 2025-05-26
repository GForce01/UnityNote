### 使用Quaternion进行旋转运算
使用Quaternion进行旋转的运算为**相乘**，且Quaternion乘法为**右乘**，即从右向左依次应用。
由于在多个轴向都存在旋转时旋转顺序十分重要因此***四元数乘法不满足交换律！！！***
例如Q1\*Q2就是指先应用Q2旋转，再应用Q1旋转。

而想要去除特定旋转的方式则为：
 Quaternion2.inverse\* Quaternion1

#### 局部与全局
https://discussions.unity.com/t/quaternion-multiplication-order/119632 中对Global和Local旋转提出了独到见解：
```
The missing piece is local vs. global. If you have rotations `A*B` you can think of it as applying A as a global rotation to B. Or as applying B as a local rotation to A(*).

In other words, suppose R is the current rotation, and Ry is a small spin on Y, which you would like to apply. To apply it as if you’re using the global rotation tool, use `Ry*R`. To apply the spin as if using the rotation tool set to local, use `R*Ry`.

In other, other words. `A*B` is B applied on local A, or global A applied to B, depending on how you want to think of it. Yes, it is very confusing until you settle on the “right” way to think of it for any particular problem.
```

| 全局坐标系下旋转 Ry                                    | 局部坐标系下旋转 Ry |
| ---------------------------------------------- | ----------- |
| Ry * R                                         | R * Ry      |
| 可以理解为先执行好原本旋转R，之后再进行Ry运算，此时Ry发生在世界坐标系下不受自身旋转影响 |             |


> - **物体当前旋转是 R**
>     
> - **要应用一个新旋转 M**
>     
> - 那么：
>     
>     - `R * M` 表示在 **物体的局部坐标系下应用 M**
>         
>     - `M * R` 表示在 **世界坐标系下应用 M**
>


在Quaternion旋转中，两个点lerp结果会是一道直线，slerp则是弧线

