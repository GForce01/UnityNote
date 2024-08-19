**简而言之，委托delegate本身并不是完整的方法，只是一个方法的容器。**
一个经典的委托声明代码如下：
	delegate void MyDelegate();
可以看出，这里定义delegate的方法虽然和定义方法很像，但是它只定义了方法的返回值和参数类型（在这里都是空）却没有定义具体的实现方法。因此，委托是一种抽象方法。

形象一些来形容，想象整个程序是从零开始建立一个工厂，委托相当于 **“流水线”这一个抽象概念**。第一步的委托声明目的是定义一条标准的“流水线”应该有什么基本**特征**：
	delegate int AssemblyLine(int material1, int material 2);

如果想开工，只知道“流水线”是什么是没有用的，因为现在的流水线只存在于想象中，在开工前要先**建立几条真实存在的流水线**。也就是定义委托的**实例化**：
	AssemblyLine firstAssemblyLine;
	AssemblyLine secondAssemblyLine;
*注意这里实例化的方法和给变量赋值的语法相同，只需要传入制定的方法名称且**不带括号***。

现在流水线建立好了，却没有具体的工作任务。委托与直接声明方法最大的不同是它**灵活可变**，直接声明的方法是一条买来的预制机器，一台机器只能干固定一件事，但是流水线可以随时重新组装以**适应不同任务**。因此想要调动流水线，要先给每条流水线**指派具体任务**，视指派方法可分为单播（singlecast）和多播(multicast)：
	firstAssemblyLine  = Addition;
	secondAssemblyLine += Division;
	secondAssemblyLine += Multiplication;
*这里Addition，Division以及Multiplication都是自定义的方法，在传入的时候同样**不带括号***。

现在万事俱备，下命令吧，厂长！
不过稍等，虽然你自信的话直接下令开工也行，但是最好还是顺带检查一遍，以防有生产线压根没领到任务，毕竟生产安全大于天嘛，具体来说有两种方法：
	//第一种方法
	if(firstAssemblyLine != null){
		firstAssemblyLine(1, 1);
	}
	//第二种方法
	secondAssemblyLine?.Invoke(6, 2);
至于这两种方法嘛，无非是靠嗓子吼还是用喇叭-没大差的，不过要是厂子（C#）版本比较老的话还是靠嗓子（第一种）会更保险。

最后，当收工的时候记得把已经拆除的流水线除名
	firstAssemblyLine  -= Addition;
	secondAssemblyLine -= Division;
	secondAssemblyLine -= Multiplication;

委托有很多的作用，最常见的就是[事件](事件Events)、回调以及将函数作为参数传递。上文的例子中展示的是一个事件用法的例子，在[[事件Events]]中使用的event是一种特殊的委托，