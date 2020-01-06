---
title:  Next7.x升级及相关配置
date: 2019-08-24 15:23:37
categories:
- 个人博客
tags:
- Hexo
- Next
comments: true
---

## 前言
本想只给博客开个 评论 功能，查询了一些教程文章后发现，现在已经是Hexo v3.9、Next v7.x了，对应的，可能现在比较流行的插件并不兼容之前的旧版本，便索性升级博客，嵌入评价、浏览数、运行时间等插件，也根据自己的风格调整了页面样式

<!-- more -->

## 主题升级
去 Next 代码仓库中，下载最新包，千万别覆盖了之前的包。除了需要在 站点配置文件 修改主题名字，还需要修改语言类型： language: zh-CN

其他的可对照着之前包的 主题配置文件 进行修改，注意有一些插件是不用了或者升级了，要做相应的更改

## 样式更改
- 字体更改，可在  \source\css\_variables\base.styl 查询 font 相关进行更改
- 最简单的方法是，在 开发者模式 下调整样式，然后在代码中查找对应 类名 ，进行修改

## 开启站点内容搜索
- 安装 hexo-generator-searchdb 插件
```js
npm install hexo-generator-searchdb --save 
```
- 在 站点配置文件 添加
```js
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
- 将 主题配置文件 的 local_search enable设置为true


## 开启评论

因为其他评论系统已经下线或者是外国网站，选择了简单的valine

- 打开 主题配置文件，将 valine enable设置为true
- 前往 leancloud 注册账号，注册应用，名字随意
- 进入应用设置，复制 appid、appkey，粘贴到 主题配置文件 valine 的对应文字
- 如需邮件通知功能，把 valine 的 notify 设置成 true
- 默认的样式有点丑，可以在 \source\css\_variables\base.styl 里覆盖样式，需要 !important

## 置顶文章
- 安装 hexo-generator-index-pin-top 插件
```js
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```
- 在需要置顶的文章，添加 top: true/数字
```js
title: XXX
tags:
  - XXX
categories:
  - XXX
date: XXX
top: 100
```
- 可添加自定义置顶样式， 在 /layout/_macro/post.swig <div class="post-meta"> 下方添加
```js
{% if post.top %}
          <span class="post-meta-item">
            <span class="post-meta-item-icon">
                <i class="fa fa-thumb-tack"></i>
            </span>
            <span class="post-meta-item-text" style="font-weight: bold;">置顶</span>
            <span class="post-meta-divider">|</span>
          </span>
        {% endif %}
```

## 显示页脚访客统计
- 打开 主题配置文件 ，讲 busuanzi_count entable设置为 true
- 在 /layout/_partials/analytics/busuanzi-counter.swig 增加以下文本标记

```javascript
{% if theme.busuanzi_count.enable %}
<div class="busuanzi-count">
<script async src="https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>

{% if theme.busuanzi_count.total_visitors %}
  <span class="post-meta-item-icon">
    <i class="fa fa-{{ theme.busuanzi_count.total_visitors_icon }}"></i>
  </span>
  <span class="site-uv" title="{{ __('footer.total_visitors') }}">
    &nbsp;本站访客数&nbsp;<span class="busuanzi-value" id="busuanzi_value_site_uv"></span>人</span>	
  </span>
{% endif %}

{% if theme.busuanzi_count.total_visitors and theme.busuanzi_count.total_views %}
  <span class="post-meta-divider">|</span>
{% endif %}

{% if theme.busuanzi_count.total_views %}
  <span class="post-meta-item-icon">
    <i class="fa fa-{{ theme.busuanzi_count.total_views_icon }}"></i>
  </span>
  <span class="site-pv" title="{{ __('footer.total_views') }}">
    &nbsp;本站总访问量&nbsp;<span class="busuanzi-value" id="busuanzi_value_site_pv"></span>次</span>	
  </span>
{% endif %}
</div>
{% endif %}
```

## 显示字数统计、阅读时长
- 安装 hexo-symbols-count-time 插件
```javascript
npm install hexo-symbols-count-time --save
npm install eslint --save
```

- 在 站点配置文件 添加

```javascript
symbols_count_time:
  symbols: true                # 文章字数统计
  time: true                   # 文章阅读时长
  total_symbols: true          # 站点总字数统计
  total_time: true             # 站点总阅读时长
  exclude_codeblock: false     # 排除代码字数统计
```

- 默认是没有 中文提示 的，如需增加，前往 \themes\next\layout\_partials\footer.swig 修改

## 显示页脚运行时间
- 在 \themes\next\layout\_partials\footer.swig 增加

```javascript
{%- if config.symbols_count_time.total_time %}

--- 增加 ---
<div id="sitetime"></div>
<script language=javascript>
	function siteTime(){
		window.setTimeout("siteTime()", 1000);
		var seconds = 1000;
		var minutes = seconds * 60;
		var hours = minutes * 60;
		var days = hours * 24;
		var years = days * 365;
		var today = new Date();
		var todayYear = today.getFullYear();
		var todayMonth = today.getMonth()+1;
		var todayDate = today.getDate();
		var todayHour = today.getHours();
		var todayMinute = today.getMinutes();
		var todaySecond = today.getSeconds();
		/* Date.UTC() -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳)
		year - 作为date对象的年份，为4位年份值
		month - 0-11之间的整数，做为date对象的月份
		day - 1-31之间的整数，做为date对象的天数
		hours - 0(午夜24点)-23之间的整数，做为date对象的小时数
		minutes - 0-59之间的整数，做为date对象的分钟数
		seconds - 0-59之间的整数，做为date对象的秒数
		microseconds - 0-999之间的整数，做为date对象的毫秒数 */
		var t1 = Date.UTC(2017,08,12,15,15,47); //建站时间
		var t2 = Date.UTC(todayYear,todayMonth,todayDate,todayHour,todayMinute,todaySecond);
		var diff = t2-t1;
		var diffYears = Math.floor(diff/years);
		var diffDays = Math.floor((diff/days)-diffYears*365);
		var diffHours = Math.floor((diff-(diffYears*365+diffDays)*days)/hours);
		var diffMinutes = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours)/minutes);
		var diffSeconds = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours-diffMinutes*minutes)/seconds);
		document.getElementById("sitetime").innerHTML=" 已运行"+diffYears+" 年 "+diffDays+" 天 "+diffHours+" 小时 "+diffMinutes+" 分钟 "+diffSeconds+" 秒";
	}
	siteTime();
</script>
--- 增加 ---

{%- if theme.footer.powered.enable %}
```

## 阅读全文
- 在文章加 <!-- more -->
- 修改 主题配置文件 -> auto_excerpt -> length

## 404页面
原来小小的举动也可以为公益事业献出自己的一份爱心，何乐而不为呢？

-新建 404.html 页面，放到主题的 source 目录下
```html
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
  <link rel="stylesheet" type="text/css" href="https://qzone.qq.com/gy/404/style/404style.css">
</head>
<body>
  <script type="text/plain" src="http://www.qq.com/404/search_children.js"
          charset="utf-8" homePageUrl="/"
          homePageName="回到我的主页">
  </script>
  <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
  <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
</body>
</html>
```

## 代码高亮
之前一直好奇为啥代码区域没有高亮，原来是忘了设置语言类型
- 手动设置
- 代码设置

```js
​```js
```

## 去掉文章目录标题
- 修改 主题配置文件 -> toc -> number: false

## 控制页面文章数目
- 修改 站点配置文件 -> index_generator -> per_page