### 事件是什么
事件是一个广播系统，在分离逻辑与表现（解耦合）从而提升代码的整洁性和可管理性中有很大作用。

事件系统的基础本质上是对[[委托Delegate]]的运用，不过委托系统用于事件系统时有一个问题-为了方便从代码外部订阅委托，委托本身需要是***public static***，但是这也意味着委托本身可以被从外部激活甚至修改，这有悖于事件系统简化耦合的初衷也容易犯错。为此，可以加入event关键字[event](关键字(修饰符).md#Event事件)，event本身是一种特殊的多播委托，除了只能从内部触发以外没有太大区别。

可以说event是一种委托的用法的子集。

就如同事件这个名称一样，事件本身无论是否有人关注都会发生，但是不同的人获悉事件发生后会有不同的反应。事件在发生后会向所有订阅者广播，收到广播的对象各自进行操作。

### 基础事件怎么用
##### 首先定义事件
	event EventHandler OnMyEvent;

##### 事件需要订阅者，因此需要添加订阅
	objectWithEvent.OnMyEvent += SomeFunction;

##### 触发事件
	OnMyEvent?.Invoke(this, EventArgs.Empty);
这里需要两个参数Object sender（发出者） 和 EventArgs，[?.](特殊语法（语法糖）#?. **空条件操作符（null-conditional operator）**)运算符用来检测事件是否成立（存在订阅者，!=null）以防止发生错误。

##### 退订事件
	objectWithEvent.OnMyEvent -= SomeFunction;

### EventHandler
EventHandler<TEventArgs\> 是一个预定义的委托，专门用来处理事件。它的定义如下：
	public delegate void EventHandler<TEventArgs\>(object sender, TEventArgs e);

### 传递参数
event触发后需要传递一个EventArgs类参数，如果不需要参数的话可以使用EventArgs.Empty。
但是如果需要自定义传递的参数的话则需要新建一个继承EventArgs的类，如：
	public class MyEventArgs : EventArgs
    {
        public int num;
    }
相应的，在调用的时候
	int count = 2;
	OnMyEvent?.Invoke(this, ***new*** MyEventArgs{num = count});
**注意这里需要使用new新建实例**。

还有就是，在定义事件的时候可以进行约束，限定只能使用某一特定EventArgs。
	public event EventHandler<MyEventArgs\> MyEvent;

### Delegate与Action
//Action is same as 'public deligate void', events can only be invoked inside the class and external methods can only subscribe to it

### UnityEvent
UnityEvent是Unity对C#Event的改进，主要好处是可以序列化，也就是可以在编辑器中看到，方便操作。当然还有些[其他好处](https://mycroftcooper.github.io/2021/03/21/Unity%E4%B8%AD%E7%9A%84%E4%BA%8B%E4%BB%B6/#3-3-UnityEvent%E7%9A%84%E4%BD%BF%E7%94%A8)。
在使用UnityEvent的时候要先在脚本开头引用
	using UnityEngine.Events;
之后和其他event并无太大区别。

### Lambda
记住一些简单的事件处理可以使用[Lambda表达式](特殊语法与语法糖.md#>=Lambda表达式])解决，以下是一段简单的在事件完成时获取EventArgs并设置结束状态的代码：
	OnCompleted += (sender, args) =>
            {
                eventArgs = args;
                isCompleted = true;
            };
