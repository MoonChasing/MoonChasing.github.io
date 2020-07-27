---
title: Hexo NexT主题古诗文排版设计
date: 2020-01-31 16:49:45
tags: [Hexo, NexT, 诗词]
---

一月初的时候在一个博客中看到了很漂亮的诗词排版，于是不会啥前端排版的我想要在自己博客中复现他。当时刚好是期末考试周，连复习的心情都么得了，狗头。

大概费了一上午的时间，终于算是实现了这个功能。当时实现好就去复习了，没有记录实现过程，今天补录下。由于刚才为了解决 Chrome 卡顿的问题删除了 Chrome 历史记录，所以这里不能提供我看到该排版的博客链接了，表示遗憾与感谢。<!--more-->

[排版展示页面](http://zhiyi.live/2020/01/02/博客文章古诗排版测试/)

想要在 NexT 主题中增添我们想要的样式，则我们应在 `Blog\themes\next\source\css\_custom\custom.styl` 文件中增添 CSS 样式代码。

代码如下，很多都是我复制的源博客的样式代码。

```css
blockquote.song 
{
    color: #404040;
            -webkit-font-smoothing: antialiased;
            font-feature-settings: "liga"1, "pnum"1, "kern"1;
            text-rendering: optimizeLegibility;
            text-size-adjust: 100%;
            font-family: Optima-Regular, "Source Han Serif SC", Manuale, "Consolas", "Noto Serif CJK SC", STFangsong, FangSong, FangSong_GB2312, "Songti SC", STSong, serif, PingFangSC-Light, PingFangTC-Light, sans-serif, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol !important;
            font-size: 100% !important;
            overflow-wrap: break-word;
            word-break: keep-all;
            line-height: 1.778;
            font-weight: 300;
            letter-spacing: 1px;
            margin-left: auto;
            margin-right: auto;
            z-index: -1;
            margin-top: 1.35em;
            margin-bottom: 1.35em;
            background: #fafafd;
            padding-top: 16px;
            border-left: 0;
            display: flex;
            justify-content: flex-end;
            padding-bottom: 0;
            border-left: solid 3px #5f6867;
}

blockquote.song p {
    font-size: 22px;
    overflow-wrap: break-word;
    word-break: break-word;
    font-weight: 300;
    letter-spacing: 1px;
    margin-left: auto;
    margin-right: auto;
    margin: 0;
    padding: 1em;
    color: #888;
    line-height: 1.6 !important;
    text-align: start;
    writing-mode: vertical-rl;
    -webkit-writing-mode: vertical-rl;
    -moz-writing-mode: vertical-rl;
    -o-writing-mode: vertical-rl;
    -ms-writing-mode: tb-rl;
    _writing-mode: tb-rl;
    min-height: 380px;
    height: auto;
    overflow: auto;
}

/* 古诗文标题 */
blockquote.song tt {
    font-family: Optima-Regular, "Source Han Serif SC", Manuale, "Consolas", "Noto Serif CJK SC", STFangsong, FangSong, FangSong_GB2312, "Songti SC", STSong, serif, PingFangSC-Light, PingFangTC-Light, sans-serif, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol !important;
    font-size: 24px;
    padding-top: 0.5em;
    padding-bottom: 0.5em;
    padding-right: 0.2em;
    font-weight: 450;
    border-top: 1px solid #404040;
    border-bottom: 1px solid #404040;
}

/* 作者 */
blockquote.song at {
    display: flex;
    color: #919191;
    padding-top: 4em;
    padding-bottom: 1.5em;
}

/* 滚动条 */
::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    background-color: #F5F5F5;
}

::-webkit-scrollbar {
    width: 4px;
    height: 4px;
    background-color: #F5F5F5;
}

::-webkit-scrollbar-thumb {
    background-color: #404040;
}
```

当我们想使用该样式时， 我们可以在 Markdown 文件中写如下 HTML 代码。

即我们设置了一个 class 为  `song` 的 `blockquote` 样式。

```html
<blockquote class="song">
<p>
            <tt>听颖师弹琴</tt><br>
            <at>韩愈</at><br>昵昵儿女语，<br>恩怨相尔汝。<br>划然变轩昂，<br>勇士赴敌场。<br>浮云柳絮无根蒂，<br>天地阔远随飞扬。<br>喧啾百鸟群，<br>忽见孤凤皇。<br>跻攀分寸不可上，<br>失势一落千丈强。<br>嗟余有两耳，<br>未省听丝篁。<br>自闻颖师弹，<br>起坐在一旁。<br>推手遽止之，<br>湿衣泪滂滂。<br>颖乎尔诚能，<br>无以冰炭置我肠！<br>
        </p>
</blockquote>
```

