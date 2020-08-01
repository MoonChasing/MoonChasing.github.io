---
title: 'C++ std::wcout输出乱码'
date: 2020-02-16 21:54:16
tags: C++
---

### 遇到问题

最近使用 C++ 遇到了中文处理问题，今天下午想测试 C++ 宽字符使用时，发现如下程序并不能如愿输出汉字。

```c++
int main()
{
    std::wstring wstr(L"你好");
    std::wcout << wstr << std::endl;
    return 0;
}
```

在网上多处搜索处理方法后仍不能解决，于是自己探索了一波。收获蛮大的，于是记录下。

> 该文章程序及结果皆在 Linux 环境下。

<!--more-->

### 结论

结论是我在一些程序中总结出来的，其中若有错误，希望指正。

1. Linux 下 `printf` 和 `wprintf` 不能混用，即同时使用。具体原因可自行搜索，网上有很多资料。
2. C++ 中默认情况下 `std::cout` 与 `std::wcout` 分别与  `printf` 和 `wprintf`  同步，维护共同缓冲区。为方便理解，我们可以假设 `std::cout` 调用 `printf`，`std::wcout` 调用 `wprintf`（这里只是假设，方便我们理解）。
3. 综合上述 1、2 两条，可知在开启 `sync_with_stdio` 情况下 `std::cout` 及 `std::wcout` 不能混用。 先使用 `std::cout` 后使用 `std::wcout` 会造成 `std::wcout` 输出乱码，先使用 `std::wcout` 的话 `std::cout` 压根不能输出东西。 关闭情况下两者可混用。
4. C 语言库中设置 `locale` 函数为 `setlocale()`， C++ 语言库中设置 `std::locale` 函数为 `std::locale::global()`。可认为 C 语言库和 C++ 库分别有不同的 `locale` 。`setlocale()` 函数仅设置 C 语言中的 `locale`，而 `std::locale::global()` 函数两者都会设置。
5. 开启 `sync_with_stdio` 情况下，可认为 `std::cout` 与 `std::wcout` 使用的是 C 语言中的 `locale`。而关闭情况下则使用 C++ 自己的 `std::locale`。
6. `std::wcout` 输出中文时依靠地区 `locale`，故我们在使用其输出中文之前要设置 `locale`。
7. 在开启 `sync_with_stdio` 情况下，想用 `std::wcout` 输出汉字要设置 C 语言中的 `locale`，使用两种方法都可以。而关闭情况下，只能使用 C++ 的方法，**且要在关闭同步之前设置**，也可以使用 `std::wcout.imbus( std::locale("") )` 单独设置。

### 其他收获

* `std::cout` 中 c 表示字符， `std::wcout` 中 wc 表示宽字符。

### 测试程序

这里展示部分测试程序及运行结果（Ubuntu 下），测试程序可根据结论中的几点自行结合测试。

#### 开启 sync_with_stdio （默认）

```c++
int main()
{
    std::wstring wstr = L"你能输出中文";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> ??????

“你能输出中文”共 6 个汉字，输出中共 6 个问号。 `wstr` 中有多少个汉字，输出中就多少个 ？

<hr>


```c++
int main()
{
    std::locale::global( std::locale("") );
    //setlocale() 与上句相替换结果一样
    std::wstring wstr = L"你能输出中文";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> 你能输出中文

<hr>


```c++
int main()
{
    std::cout << "你好" << std::endl;
    std::wstring wstr = L"你能输出中文？";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> 你好
>
> wcout 输出乱码

<hr>


```C++
int main()
{
    std::wstring wstr = L"你能输出中文？";
    std::wcout << wstr << std::endl;
    std::cout << "你好" << std::endl;
    std::cout << "你好" << std::endl;
    return 0;
}
```

> ??????

`cout` 并不会输出

<hr>


#### 关闭 sync_with_stdio

```c++
int main()
{
    std::ios::sync_with_stdio(false);
    // setlocale( LC_ALL, ""); //有没有这句输出都一样
    std::wstring wstr = L"你能输出中文？";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> 输出为空

<hr>


```c++
int main()
{
    std::ios::sync_with_stdio(false);
    std::locale::global( std::local("") );
    std::wstring wstr = L"你能输出中文？";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> 你能输出中文？

<hr>


```c++
int main()
{
    std::ios::sync_with_stdio(false);
    std::locale::global( std::local("") );		//删除此句结果相同
    std::cout << "你好" << std::endl;
    std::wstring wstr = L"你能输出中文？";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> 你好

<hr>


```c++
int main()
{
    std::ios::sync_with_stdio(false);
    std::wcout.imbue( std::locale("") );
    std::cout << "你好" << std::endl;
    std::wstring wstr = L"你能输出中文？";
    std::wcout << wstr << std::endl;
    return 0;
}
```

> 你好
>
> 你能输出中文？

<hr>



