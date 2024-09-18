# TArray

TArray 是 Unreal Engine (UE) 中用于存储和管理元素的动态数组模板类。它类似于标准库中的 std::vector，但针对 Unreal Engine 的特定需求进行了优化和扩展。TArray 提供了一组强大的功能，使得在游戏开发中进行数组操作非常方便。它既可以用于简单的数值数组，也可以存储复杂的 UObject 派生类型。

## TArray 的特点

动态大小：TArray 是一个动态数组，可以根据需要自动调整其大小。当添加或删除元素时，数组会自动扩展或收缩。

内存连续性：TArray 在内存中以连续块存储数据，这使得它在遍历和随机访问元素时非常高效。类似于 std::vector，它可以利用 CPU 缓存优化性能。

丰富的API：TArray 提供了大量的成员函数来操作数组，包括添加、删除、插入、排序、查找等。这些函数使得 TArray 在 UE 中非常通用。

类型安全：TArray 是一个模板类，可以存储任何类型的数据（包括基本类型和用户定义的类），并且提供了类型安全的接口。

与引擎集成：TArray 与 Unreal Engine 的反射系统和垃圾回收（GC）系统良好集成。当 TArray 存储 UObject 指针时，UE 的 GC 系统能够跟踪这些指针，从而正确管理 UObject 的生命周期。

## 常用API

### 构造

默认构造函数：
```c++
TArray<int32> MyArray;
```
带容量的构造函数：
```c++
TArray<int32> MyArray;
MyArray.Reserve(10); // 预分配容量，避免频繁的内存分配
```

### 元素访问

operator[]：通过索引访问元素。
```cpp
int32 FirstElement = MyArray[0]; // 获取第一个元素
MyArray[1] = 100; // 设置第二个元素的值
```

GetData：获取指向内部数据的指针。
```c++
int32* DataPtr = MyArray.GetData();
```

### 插入

Add：在数组末尾添加元素。

```cpp
MyArray.Add(42);
```

AddUnique：在数组末尾添加唯一元素（如果数组中不存在相同的元素）。

```cpp
MyArray.AddUnique(42);
```

Insert：在指定位置插入元素。

```cpp
MyArray.Insert(15, 1); // 在索引1处插入15
```

Emplace：在数组末尾构造元素，避免额外的拷贝。

```cpp
MyArray.Emplace(100);
```



