---
title: img 标签图片居中显示
date: 2020-04-27 11:48:37
categories:
- Css3
comments: false
---
使用 `object-fit 属性`

```css
.img {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover; // 保证图片覆盖容器的宽高
}
```

<!-- more -->

### 其它值

- fill：中文释义“填充”。默认值。替换内容拉伸填满整个contentbox，不保证保持原有的比例。

- contain：中文释义“包含”。保持原有尺寸比例。保证替换内容尺寸一定可以在容器里面放得下。因此，此参数可能会在容器内留下空白。

- cover：中文释义“覆盖”。保持原有尺寸比例。保证替换内容尺寸一定大于容器尺寸，宽度和高度至少有一个和容器一致。因此，此参数可能会让替换内容部分区域不可见。

- none：中文释义“无”。保持原有尺寸比例。同时保持替换内容原始尺寸大小。

- scale-down：中文释义“降低”。就好像依次设置了none或contain, 最终呈现的是尺寸比较小的那个。