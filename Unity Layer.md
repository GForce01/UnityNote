在Unity中，每个游戏对象（GameObject）都可以分配到一个层（Layer），层的索引从0到31，总共有32个层。层可以用于各种目的，例如碰撞检测、渲染和物理交互。

## Layer与Layer Mask
Layer的类型是一个常规整型，而Layer Mask则是一个二进制位掩码。***在Unity api的逻辑运算中使用的是Layer Mask而不是Layer***，直接将Layer数值作为输入会出错：`比如想要对第9层的物体进行RayCast，如果直接将9传入Physics.Raycast中的话实际上是传入了0b00001001（0b开头代表此数为二进制），对于unity来说这相当于一个在第0层和第3层数字为1的一个Layer Mask`
正确的操作需要用到[位运算](特殊语法与语法糖#位运算符)，事实上，Layer相关的操作大多需要用到位运算逻辑。
首先，将一个Layer转换为Layer Mask可以使用位移运算符<<，具体的逻辑是将1向左移动层数位以匹配Layer Mask的结构：
```cs
var objectLayerMask = 1 << 9;
//9为层数
//结果为0b10_0000_0000
```
