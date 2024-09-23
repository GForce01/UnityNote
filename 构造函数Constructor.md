类的**构造函数**是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。

构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于为某些成员变量设置初始值。有时候一个类不需要运行Start方法，这时候构造函数就很有用。

如果将构造函数设置为private则可以确保类仅可在内部被创建，在单例模式尤为重要。

例子：
```cs
public class Example
{
    private int _value;

    public Example(int initialValue)//这里就是构造函数，在创建时执行
    {
        _value = initialValue;
    }

    public int ShowValue
    {
        return _value;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Calculator calc = new Calculator(10); // 调用构造函数创建实例
        int result = calc.ShowValue(5); // 通过实例调用
        Console.WriteLine(result); // 输出：15
    }
}
```
