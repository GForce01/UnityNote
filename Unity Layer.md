在Unity中，每个游戏对象（GameObject）都可以分配到一个层（Layer），层的索引从0到31，总共有32个层。层可以用于各种目的，例如碰撞检测、渲染和物理交互。

## Layer与Layer Mask
Layer的类型是一个常规整型，而Layer Mask则是一个二进制位掩码。***在Unity api的逻辑运算中使用的是Layer Mask而不是Layer***，直接将Layer数值作为输入会出错：`比如想要对第9层的物体进行RayCast，如果直接将9传入Physics.Raycast中的话实际上是传入了0b00001001（0b开头代表此数为二进制），`