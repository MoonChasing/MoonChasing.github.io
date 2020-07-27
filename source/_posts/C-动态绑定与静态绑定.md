---
title: C++动态绑定与静态绑定
date: 2019-08-24 11:56:29
tags: C++
categories: C++
---

今天做题的时候遇到这样一个问题。

以下程序片段输出内容为：

```c++
class Demo {
public:
    Demo():count(0) {}
      ~Demo() {}
  
      void say(const std::string&msg) {
          fprintf(stderr,"%s\n", msg.c_str());
      }   
   private:
      int count;
};
  
int main(int argc, char **argv) {
    Demo* v = NULL;
    v->say("hello world");
}
```

<!--more-->答案为 "hello, world"。

这个问题涉及到了 C++ 中动态绑定和静态绑定的知识。静态绑定又名前期绑定， early binding，是在编译期就确定的，通过对象调用。而动态绑定又名后期绑定， late binding，是在运行时确定的，通过地址实现。

只有采用**“指针->函数()”**或**“引用变量.函数()”**的方式调用*C++*类中的**虚函数**才会执行动态绑定。对于*C++*中的非虚函数，因为其不具备动态绑定的特征，所以不管采用什么样的方式调用，都不会执行动态绑定。（本段及下面表格来自[阿波123](https://blog.csdn.net/livelylittlefish/article/details/2171521)的博客，感谢。）

<div class="table-box"><table class="MsoTableGrid" style="border-right:medium none;border-top:medium none;border-left:medium none;border-bottom:medium none;border-collapse:collapse;" cellspacing="0" cellpadding="0" border="1"><tbody><tr><td style="border-right:1.5pt double;border-top:1pt solid;background:#b3b3b3;border-left:1pt solid;width:89.4pt;border-bottom:1pt solid;" valign="top" width="119" rowspan="2">
            <div class="MsoNormal"><span lang="en-us" style="font-size:9pt;font-family:'宋体';" xml:lang="en-us"><font face="Times New Roman"><font><span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></font></font></span><p></p></div>
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>代码形式<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1.5pt double;border-top:1pt solid;background:#b3b3b3;border-left:#ece9d8;width:206.2pt;border-bottom:1pt solid;" valign="top" width="275" colspan="2">
            <div class="MsoNormal" style="text-align:center;" align="center"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>对于虚函数<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1pt solid;border-top:1pt solid;background:#b3b3b3;border-left:#ece9d8;width:197.1pt;border-bottom:1pt solid;" valign="top" width="263" colspan="2">
            <div class="MsoNormal" style="text-align:center;" align="center"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>对于非虚函数<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
        </tr><tr><td style="border-right:1pt solid;border-top:#ece9d8;background:#b3b3b3;border-left:#ece9d8;width:152.25pt;border-bottom:1pt solid;" valign="top" width="203">
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>作用<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1.5pt double;border-top:#ece9d8;background:#b3b3b3;border-left:#ece9d8;width:53.95pt;border-bottom:1pt solid;" valign="top" width="72">
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>绑定方式<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;background:#b3b3b3;border-left:#ece9d8;width:145.55pt;border-bottom:1pt solid;" valign="top" width="194">
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>作用<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;background:#b3b3b3;border-left:#ece9d8;width:51.55pt;border-bottom:1pt solid;" valign="top" width="69">
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>绑定方式<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
        </tr><tr><td style="border-right:1.5pt double;border-top:#ece9d8;border-left:1pt solid;width:89.4pt;border-bottom:1pt solid;" valign="top" width="119">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>类名<span lang="en-us" xml:lang="en-us">::</span>函数<span lang="en-us" xml:lang="en-us">()</span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:152.25pt;border-bottom:1pt solid;" valign="top" width="203">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用指定类的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1.5pt double;border-top:#ece9d8;border-left:#ece9d8;width:53.95pt;border-bottom:1pt solid;" valign="top" width="72">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>静态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:145.55pt;border-bottom:1pt solid;" valign="top" width="194">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用指定类的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:51.55pt;border-bottom:1pt solid;" valign="top" width="69">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>静态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
        </tr><tr><td style="border-right:1.5pt double;border-top:#ece9d8;border-left:1pt solid;width:89.4pt;border-bottom:1pt solid;" valign="top" width="119">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>对象名<span lang="en-us" xml:lang="en-us">.</span>函数<span lang="en-us" xml:lang="en-us">()</span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:152.25pt;border-bottom:1pt solid;" valign="top" width="203">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用指定对象的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1.5pt double;border-top:#ece9d8;border-left:#ece9d8;width:53.95pt;border-bottom:1pt solid;" valign="top" width="72">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>静态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:145.55pt;border-bottom:1pt solid;" valign="top" width="194">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用指定对象的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:51.55pt;border-bottom:1pt solid;" valign="top" width="69">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>静态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
        </tr><tr><td style="border-right:1.5pt double;border-top:#ece9d8;border-left:1pt solid;width:89.4pt;border-bottom:1pt solid;" valign="top" width="119">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>引用变量<span lang="en-us" xml:lang="en-us">.</span>函数<span lang="en-us" xml:lang="en-us">()</span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:152.25pt;border-bottom:1pt solid;" valign="top" width="203">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用<strong>被</strong>引用对象所属类的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1.5pt double;border-top:#ece9d8;border-left:#ece9d8;width:53.95pt;border-bottom:1pt solid;" valign="top" width="72">
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>动态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:145.55pt;border-bottom:1pt solid;" valign="top" width="194">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用引用变量所属类的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:51.55pt;border-bottom:1pt solid;" valign="top" width="69">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>静态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
        </tr><tr><td style="border-right:1.5pt double;border-top:#ece9d8;border-left:1pt solid;width:89.4pt;border-bottom:1pt solid;" valign="top" width="119">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>指针<span lang="en-us" xml:lang="en-us">-&gt;</span>函数<span lang="en-us" xml:lang="en-us">()</span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:152.25pt;border-bottom:1pt solid;" valign="top" width="203">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用<strong>被</strong>引用对象所属类的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1.5pt double;border-top:#ece9d8;border-left:#ece9d8;width:53.95pt;border-bottom:1pt solid;" valign="top" width="72">
            <div class="MsoNormal"><strong><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>动态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span></strong><p><strong></strong></p><strong></strong></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:145.55pt;border-bottom:1pt solid;" valign="top" width="194">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>调用指针变量所属类的指定函数<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
            <td style="border-right:1pt solid;border-top:#ece9d8;border-left:#ece9d8;width:51.55pt;border-bottom:1pt solid;" valign="top" width="69">
            <div class="MsoNormal"><span style="font-size:9pt;font-family:'宋体';"><font face="Times New Roman"><font>静态绑定<span lang="en-us" xml:lang="en-us"></span></font></font></span><p></p></div>
            </td>
        </tr></tbody></table></div>

本题中， `say()`方法为非虚函数，故进行静态绑定，即使 `v` 是空指针，也可以根据 `v` 的类型调用该函数。

这令我想起了之前阅 《Effective C++ 》 时的一些收获，一起写在这里。

1. 条款20: 宁以 pass-by-reference-to-const 替换 pass-by-value
   使用引用可以解决切割 (slicing) 问题。
2. 条款36: 绝不重新定义继承而来的 non-virtual 函数
   因为如果重新定义，根据上述静态绑定和动态绑定的不同，会绑定到不同的函数中，从而效果不一样。
3. 条款37: 绝不重新定义继承而来的缺少参数值
   由条款 36 重新定义 non-virtual 函数永远是错误的。对于 virtual 函数，其为动态绑定，而缺少参数值却是静态绑定！