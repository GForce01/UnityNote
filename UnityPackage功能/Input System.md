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
10. Action Maps-一组动作，比如游戏的主界面和游戏界面需要两套不同的操作时就可以各自设置一个Action Map

## Input System的几种工作架构
Input System一共有四种工作流程架构，各自拥有不同的灵活性和抽象性。
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
跳过Input Action Asset直接在脚本中声明公开的***InputAction***变量，如此一来可以相对简便地享受到Input System的动作绑定的便利性，但是这样无法进行动作分组等操作。
嵌入动作声明方法如下：
```cs
public InputAction myAction;
```
具体用法上来说，需要先声明Action变量并在Inspector中完成设置，在代码中需要先[激活](#激活与停止)动作，若要读取输入数值则要么***循环调用***信息要么为事件***订阅Handler***。
### 使用Actions Asset
在编辑器中右键新建一个Actions Asset并在其UI中设置。比起使用和代码刚性绑定的嵌入动作，使用Actions Asset设置的控制方案可以在全军内的多个脚本中通用。而且这样还允许使用Action Maps和Control Schemes功能。
在直接使用Actions Asset时，也有两种方法：

一. 引用Actions Asset
1. 首先需要声明一个公开的InputActionsAsset变量并在编辑器中将创建好的Actions Asset拖入，对于需要频繁或循环调用的输入也需要声明一个变量储存
```cs
public InputActionAsset actions;

private InputAction moveAction;
```
2. 之后可以通过string的方式访问对应名称的动作
```cs
moveAction = actions.FindActionMap("gameplay").FindAction("move");

actions.FindActionMap("gameplay").FindAction("jump").performed += OnJump;
```

二. 通过生成的C#封装访问Actions Asset
1. 在编辑器中选择Actions Asset并勾选Generate C# Class，这会生成一个同名的C#文件。
2. 在代码中创建一个Actions C#类的实例
```cs
//假设生成的Actions类名称叫做ExampleActions
ExampleActions actions;

void Awake(){
	actions = new ExampleActions();
}
```
3. 之后就可以在脚本中通过***Actions类.Action Maps名称.Action名称***的方法访问到对应动作
```cs
//Actions类.Action Maps名称.Action名称
Vector2 moveVector = actions.gameplay.move.ReadValue<Vector2>();
```
### 使用[[Input System#Player Input|Player Input]]组件
最后一种方法是使用Player Input组件，一般来说组件应该与处理动作逻辑的脚本放在同一个物体下。使用组件可以减少很多代码量，但是相应的需要更多编辑器内操作同时在debug的时候会有些困难，需要自己取舍。

## 数据种类
一共有三种动作种类：
1. Value
2. Button
3. Passthrough
前两张不必多说，按键类的操作一般是button，
## 控制种类

## 组件
### Player Input


## Input System Scripting & API
在一切使用Input System逻辑的代码中请务必记得在开头添加引用
```cs
using UnityEngine.InputSystem;
```
### 激活与禁用
在使用特定动作之前需要先将其激活（Enable），可以单独激活某个动作也可以一次激活整个Action Map：
```cs
//假设在gameplayActions这个map下有一个lookAction动作
lookAction.Enable();
gameplayActions.Enable();
```
相应的，当不需要控制的时候需要将其禁用（Disable）
```cs
lookAction.Disable();
gameplayActions.Disable();
```
禁用十分重要，尤其是在与动作相关连的逻辑可能被禁用的时候，否则可能会引起报错
### 读取Action数值
读取Action数值一般有两种方法，一种是直接循环拉取数值，另一种是为事件添加[订阅](事件Events#事件需要订阅者，因此需要添加订阅)。
#### 循环查询（Polling）
一般来说在update中运行，不过也可以自己写一个循环[[协程]]。
```cs
public void Update()
{
    Vector2 moveAmount = moveAction.ReadValue<Vector2>();
}
```
如果需要判断当前帧是否发生操作可以使用：
```cs
InputAction.WasPerformedThisFrame()
```
特别地，对于类行为Button的动作，有三种：
```cs
InputAction.IsPressed()
InputAction.WasPressedThisFrame()
InputAction.WasReleasedThisFrame()
```
#### 订阅Handler
一般对于按钮之类的一次性操作可以直接订阅。
每个动作流程会产生的事件有三种：
1. action.started
2. action.performed
3. action.canceled
其中performed会视动作的数值类型决定触发时机。对于Button类动作，仅会在读数第一次超过阈值的时候触发，对于数值类型的动作，每次数值变化且变化后与默认数值不同的时候都会触发performed。
```cs
jumpAction.performed += ctx => { OnJump(ctx); };
```
Action Event的回调事件会输入一个CallbackContext，也就是上文中的ctx，因此在定义动作逻辑时无论是否会用到都需要在签名中留下相应的输入。
```cs
public void OnJump(InputAction.CallbackContext context)
{
    // jump code goes here.
}
```
如果需要读取context的内容，则可以使用ReadValue<TValue>()方法。
