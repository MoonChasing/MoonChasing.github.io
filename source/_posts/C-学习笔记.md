---
title: C++学习笔记
date: 2019-03-09 23:21:52
tags: C++, 学习笔记
categories: C++
---

*  String 字符串字符编码格式及其前缀

  ```c++
  wchar_t title[] = L"Chief Astrogator";  // w_char string
  char16_t name[] = u"Felonia Ripova";    // char_16 string
  char32_t car[] = U"Humber Super Snipe"; // char_32 string
  ```

  UTF-8:  前缀u8

  ```C++
  string a = u8"UTF_8 Encoding";
  ```

  Raw String: \不再转义， 前缀R, 使用()限界

  ```c++
  cout << R"(Jim "King" Tutt uses "\n" instead of endl.)" << '\n';
  
  cout << R"+*("(Who wouldn't?)", she whispered.)+*" << endl; //为了输出), 
  //输出为"(Who wouldn't?)", she whispered.
  ```

* C++ 11 结构体初始化可忽略 =

  ```C++
  struct inflatable   // structure declaration
  {char name[20];
   float volume;
   double price;
  };
  
  inflatable guest ={"Glorious Gloria",  // name value
                     1.88,               // volume value
                     29.99               // price value
                    };
  
  inflatable duck {"Daphne", 0.12, 9.98};  // can omit the = in C++11
  ```

  