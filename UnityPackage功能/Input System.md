## Input System结构与概念
### Input System简介
Input System主要面向需要在多种不同控制方案间切换的需求，比起老输入系统来说Input System最大的优势在于没有通过代码将行为与特定按键刚性连接，而是通过一系列绑定关系将多个操作映射到一个行为上。
### Input System中的概念
![[ConceptsOverview.svg]]
1. 用户-说的就是你，玩家，别多想。
2. 设备-指物理意义上的输入设备。
3. 控制-指设备上的某个特定部分。
4. 交互-针对某个控制的一系列不同操作，比如单击或长按等。
5. 动作-指经过某些操作后触发的行为，是操作的直接影响目标。
6. 动作方法-动作方法是动作的具体实现逻辑。
7. 动作资产Action Asset-一种特殊的资产类型，自带UI，内部储存了控制方案，映射表，以及动作组等信息。
8. 嵌入动作Embedded Actions-指直接在MonoBehaviours脚本中定义而非通过动作资产定义的动作。会失去创建动作组，映射表以及控制方案的便利特性。
9. 绑定-动作和控制之间的关联。

## Input System的几种工作架构
### 直接使用脚本读取设备状态
直接在脚本中读取设备输入，方法类似于老输入系统，操作与逻辑硬性绑定，完全没有用到Input System的特色，不推荐使用
```cs
void Update()
    {
        var gamepad = Gamepad.current;
        if (gamepad == null)
        {
            return; // No gamepad connected.
        }

        if (gamepad.rightTrigger.wasPressedThisFrame)
        {
            // 'Use' code here
        }

        Vector2 move = gamepad.leftStick.ReadValue();
        {
            // 'Move' code here
        }
    }
```
### 使用嵌入动作
跳过Input Action Asset直接在脚本中声明公开的***InputAction***变量，如此一来可以相对简便地享受到Input System的动作绑定的便利性，但是这样无法进行动作分组等操作。具体用法上来说，需要先声明Action变量并在Inspector中完成设置，在代码中需要先[激活](#激活与停止)动作，之后可以直接
###
###

## 输入种类

## 行为种类

## Input System Scripting & API
在一切使用Input System逻辑的代码中请务必记得在开头添加引用
```cs
using UnityEngine.InputSystem;
```

### 激活与停止

### 读取Action数值
读取Action数值一般有两种方法，一种是直接循环拉取数值，另一种是使用