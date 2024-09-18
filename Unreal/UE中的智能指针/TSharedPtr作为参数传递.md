# TsharedPtr 作为参数传递

将 TSharedPtr 传入函数时，会发生以下情况，取决于函数参数的声明方式（传值、传引用或传指针）。这些传递方式的行为主要影响到引用计数和性能。

## 传值 (Pass by Value)

当函数参数以值传递的方式接收 TSharedPtr 时，会增加引用计数。这是因为传值会创建一个新的 TSharedPtr，此时引用计数会递增。

创建 TSharedPtr 的副本，引用计数增加 1。当函数执行完毕时，局部 TSharedPtr 副本销毁，引用计数减少 1。

```c++
void SomeFunction(TSharedPtr<MyObject> Ptr)
{
    // 引用计数增加
}
TSharedPtr<MyObject> MyPtr = MakeShared<MyObject>();
SomeFunction(MyPtr);  // 引用计数增加到 2
// 当函数执行完毕，引用计数会恢复为 1
```

性能开销：传值会增加一次引用计数操作，虽然 TSharedPtr 的引用计数管理是非常高效的，但在某些性能敏感的场景下，传值可能带来额外的开销。

副本安全性：由于传值，函数内部的操作不会影响原始的 TSharedPtr（例如重置或修改指针不会影响外部 TSharedPtr），这种行为对函数的安全性有帮助。

## 传引用 (Pass by Reference)

当函数参数以引用传递的方式接收 TSharedPtr 时，不会增加引用计数。这是因为引用传递并不会创建 TSharedPtr 的副本，仅传递原始指针的引用。

不会改变引用计数。
函数内可以对 TSharedPtr 进行修改，影响外部的 TSharedPtr。

```c++
复制代码
void SomeFunction(const TSharedPtr<MyObject>& Ptr)
{
    // 不会增加引用计数
}
TSharedPtr<MyObject> MyPtr = MakeShared<MyObject>();
SomeFunction(MyPtr);  // 引用计数保持不变
```

性能：不会有额外的引用计数开销，性能更高。

修改安全性：传递 const TSharedPtr\<MyObject>& 表示函数不会修改 TSharedPtr 的状态（例如重置指针等），因此可以安全地传递引用。

## 传递 TSharedPtr 的 Move 语义 (Pass by Move)

当使用 MoveTemp 将 TSharedPtr 移动到函数中，表示不再需要原始 TSharedPtr。这会避免引用计数增加，但原始指针会被重置为 nullptr。

引用计数不会增加，直接移动智能指针的所有权。
原始 TSharedPtr 被置空。

```c++
void SomeFunction(TSharedPtr<MyObject> Ptr)
{
    // 直接获取所有权，原来的智能指针被置空
}

TSharedPtr<MyObject> MyPtr = MakeShared<MyObject>();
SomeFunction(MoveTemp(MyPtr));  // 引用计数保持不变，但 MyPtr 变为 nullptr
```

效率：不会有额外的引用计数操作，直接转移智能指针的所有权。

生命周期管理：原始智能指针被置空后不能再使用。