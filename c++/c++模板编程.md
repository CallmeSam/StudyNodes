功能：比较大小
```c++
template  <typename T>
int compare(const T& a, const T& b)
{
    if (std::less<T>() (a , b)) return 1;
    if (std::less<T>()(b, a)) return -1;
    return 0;
}
```

---

功能：迭代器查找
```c++
template <typename T, typename N>
T t_find(T range_a, T range_b, const N& val)
{
    for (auto iter = range_a; iter != range_b; iter++)
    {
        if (*iter == val)
            return iter;
    }
    return range_b;
}
```
---

功能：数组类型
```c++
template <typename Arr, size_t N>
Arr* t_begin(Arr(&a)[N])
{
    return a;
}

template <typename T>
auto inline t_begin(const T& a)->decltype(a.begin())
{
    return a.begin();
}

template<typename T, ssize_t N>
constexpr ssize_t arraySize(T(&arr)[N])
{
    return N;
}
```
