### 事件是什么
事件是一个广播系统，在分离逻辑与表现（解耦合）从而提升代码的整洁性和可管理性中有很大作用。
就如同事件这个名称一样，事件本身无论是否有人关注都会发生，但是不同的人获悉事件发生后会有不同的反应。事件在发生后会向所有订阅者广播，收到广播的对象各自进行操作。

Delegate与Event的关系

事件怎么用
首先定义事件
	event EventHandler OnMyEvent;
事件需要订阅者，因此需要添加订阅
	objectWithEvent.OnMyEvent += SomeFunction;
触发事件
	OnMyEvent?.Invoke(this, EventArgs.Empty);

EventHandler

传递参数
event触发后需要传递一个EventArgs类参数，如果不需要参数的话可以


UnityEvent