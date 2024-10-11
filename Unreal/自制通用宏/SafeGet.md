# SafeGet
在UE编程中会出现大量的判空语句，使用改宏可以简化代码，使代码更加清晰。
```c++
/**
 * @ lry
 * @brief 安全地调用对象的成员函数。
 * 
 * 如果对象为空，则返回 nullptr。否则，调用对象的指定成员函数。
 * 
 * @param Object 指向对象的指针
 * @param Function 要调用的成员函数
 */
template<typename T, typename MemberFunc>
inline auto SafeGet(T* Object, MemberFunc Function) -> decltype((Object->*Function)())
{
	using ReturnType = decltype((Object->*Function)());
	return (Object) ? (Object->*Function)() : ReturnType();
}
/**
 * @ lry
 * @brief 安全地调用对象的带参数的成员函数。
 * 
 * 增加传参功能
 * 
 * @param Object 指向对象的指针
 * @param Function 要调用的成员函数
 */
template<typename T, typename MemberFunc, typename... Args>
inline auto SafeGet(T* Object, MemberFunc Function, Args&&... args) -> decltype((Object->*Function)(std::forward<Args>(args)...))
{
	return (Object && Function) ? (Object->*Function)(std::forward<Args>(args)...) : nullptr;
}
```