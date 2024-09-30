# SWidget 与 SCompoundWidget

SWidget 和 SCompoundWidget 都是 Unreal Engine 中基于 Slate 框架的 UI 控件基类，但它们在设计上有一些关键区别。

## SWidget 的角色

SWidget 是所有 Slate 控件的基础类，它是 Slate 框架中所有 UI 元素的基类，提供了基本的功能，包括绘制、输入处理、大小计算和子控件的管理等。它是一个相对低级别的控件，主要作用是为更高级的控件提供基础设施。

轻量级：SWidget 的主要目的是提供绘制和事件处理的接口。如果你需要完全自定义控件的行为，可以继承自 SWidget。

手动管理子控件：SWidget 通常不直接管理子控件（如容器或复杂控件），需要自己实现 GetChildren()、OnArrangeChildren() 等函数。

### SRichTextBlock

SRichTextBlock 由 Swidget 派生而来。

## SCompoundWidget 的角色

SCompoundWidget 是继承自 SWidget 的高级类，它是一个可以包含其他子控件的容器类。SCompoundWidget 提供了方便的接口来管理一个单一的子控件树（多个子控件的集合），使其非常适合用来构建复杂的组合控件。

### SCompoundWidget 的特点：

支持子控件：SCompoundWidget 主要设计用来管理子控件，它简化了子控件的构建和管理过程。典型的 SCompoundWidget 允许你使用 ChildSlot 来设置子控件。

组合控件：SCompoundWidget 是用来创建容器或组合控件的基础类。通过 SCompoundWidget，你可以将多个控件组合到一起，比如将按钮、文本框、进度条等组合到一个控件中。

简化布局：SCompoundWidget 提供了 ChildSlot，用于指定控件的子控件及其布局方式，简化了子控件的排列、绘制和布局。

