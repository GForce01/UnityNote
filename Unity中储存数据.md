### Dictionary字典
在 C# 中，`Dictionary<TKey, TValue>` 是一个**键值对（Key-Value Pair）数据结构**，其中：

- `TKey` 是键（Key），通常是 **字符串**（`string`）。
- `TValue` 是值（Value），可以是任何数据类型，比如 **字符串（`string`）、整数（`int`）、对象（`object`）等**。
#### Json
一种方便的储存数据结构，形式如下：
```Json
{
	"Key1(String)": "Value1(Object)",
	"Key2(String)": {
		"Key3(String)": "Value3(Object)"
	}
}
//Value2是个嵌套在内的字典，也就是{"Key3(String)": "Value3(Object)"}整个都是Value2
```
JSON **本质上就是** `Dictionary<string, object>` 的一种表示形式，但是Unity中禁止直接将Json作为对象，但是字典可以直接**映射**为Json。
Json可以被储存在String中，其与字典互相映射的方法如下：
```cs
string json = JsonConvert.SerializeObject(person, Formatting.Indented);
Console.WriteLine(json);
```

### Class