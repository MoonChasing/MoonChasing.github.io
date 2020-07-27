---
title: Typora Ash 主题 html编辑框背景优化
date: 2019-12-24 17:15:29
tags: [Typora, 优化]
---



`Typora` 的 `Ash` 主题实在是太爽了，但唯一一点美中不足就是我们在 md 中编写 html 格式时， html 编辑框的背景颜色和文字颜色几乎一样，看起来十分的晃眼，带来了不便。

要解决这个问题，我们在 `ash.css` 文件的 `:root` 下增加一行格式即好。

```css
--rawblock-edit-panel-bd: #555;
```

因为 `Ash` 主题的背景为 <span style='color:#666;'>#666</span>，于是我选择了<span style='color:#555;'> #555</span> 与它搭配。





