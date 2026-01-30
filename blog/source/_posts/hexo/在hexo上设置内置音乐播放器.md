---
title: 在hexo上设置内置音乐播放器
date: 2023-09-02 23:34:03
tags:
- 个人博客搭建
categories:
cover:
---

## 前言

先进行一个说明，本篇文章将会进行几个说明，对于内置的音乐播放器，我们将会说明两种情况，一种就是使用html语言去添加一个iframe的窗口，相当于是再次添加一个窗口。与你整个页面无关了，第二种方式就是，采用插件aplayer插件。

## iframe

```HTML
 <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=29751583&auto=1&height=66"></iframe>
```



在你浏览器页面中，html文件中进行一个插入即可，直接进行使用。比较方便易于理解，就不过多解释了。缺点就是响应时间较慢。

## aplayer

这个插件，写者仍然也是比较懵的，跟着一些大佬们多问多查，方才稍微学会了一些些基本应用，我推荐去github去查这个插件的使用方式，或者在一些

[大佬]: https://butterfly.js.org/posts/507c070f/	"的文章"

来进行一些学习

这个步骤大概是

	1. 我们需要先创建一个md文件 作为音乐播放器的文档生成
	1. 然后输入一些md语言 来生成我们需要的音乐播放器的框架
	1. 成功

### 创建

这就不用多说说明了 hexo n xxxx 随机创建一个都可以了

### 代码

```markdown
{% meting "000PeZCQ1i4XVs" "tencent" "artist" "theme:#3F51B5" "mutex:true" "preload:auto" %}
```

***这段代码将会通过插件自动成出一段对应的html语言***

```html
<div id="aplayer-uxAIfEUs" class="aplayer aplayer-tag-marker meting-tag-marker" data-id="000PeZCQ1i4XVs" data-server="tencent" data-type="artist" data-mode="circulation" data-autoplay="false" data-mutex="true" data-listmaxheight="340px" data-preload="auto" data-theme="#3F51B5"></div>
```

如果我们不使用 插件 则我们需要将这个对应的html代码，直接复制到html源文件里，因为少了一步插件中的转义的部分，我们就需要自己来进行了

```HTML
<div class="aplayer" data-id="000PeZCQ1i4XVs" data-server="tencent" data-type="artist" data-mutex="true" data-preload="auto" data-theme="#3F51B5"></div>
```

### 關閉 asset_inject
此步驟適用於安裝了 hexo-tag-aplayer 插件的人

由於需要全局都插入 aplayer 和 meting 資源，為了防止插入重複的資源，需要把 asset_inject 設為 false

在 Hexo 的_config.yml配置文件中设置

```BASH
aplayer:
  meting: true
  asset_inject: false
```

## 開啟主題的 aplayerInject
在主題的配置文件中，enable 設為 true 和 per_page 設為 true

```
# Inject the css and script (aplayer/meting)
aplayerInject:
  enable: true
  per_page: true
```

插入 Aplayer html
为了适配 hexo-tag-aplayer，主題內置的 Meting js 仍为 1.2 版本，并非最新的 2.x 版本。

Aplayer html 例子：

```HTML
<div class="aplayer no-destroy" data-id="60198" data-server="netease" data-type="playlist" data-fixed="true" data-autoplay="true"> </div>
```

1. 

<div class="aplayer no-destroy" data-id="60198" data-server="netease" data-type="playlist" data-fixed="true" data-autoplay="true"> </div>
參數解釋

|       option       | default  |                         description                          |
| :----------------: | :------: | :----------------------------------------------------------: |
|      data-id       | require  |      song id / playlist id / album id / search keyword       |
|    data-server     | require  |    music platform: netease, tencent, kugou, xiami, baidu     |
|     data-type      | require  |            song, playlist, album, search, artist             |
|     data-fixed     |  false   |                      enable fixed mode                       |
|     data-mini      |  false   |                       enable mini mode                       |
|   data-autoplay    |  false   |                        audio autoplay                        |
|     data-theme     | #2980b9  |                          main color                          |
|     data-loop      |   all    |        player loop play, values: 'all', 'one', 'none'        |
|     data-order     |   list   |         player play order, values: 'list', 'random'          |
|    data-preload    |   auto   |              values: 'none', 'metadata', 'auto'              |
|    data-volume     |   0.7    | default volume, notice that player will remember user setting, default volume will not work after user set volume themselves |
|     data-mutex     |   true   | prevent to play multiple player at the same time, pause other players when this player start play |
|    data-lrctype    |    0     |                          lyric type                          |
|  data-listfolded   |  false   |         indicate whether list should folded at first         |
| data-listmaxheight |  340px   |                       list max height                        |
|  data-storagename  | metingjs |          localStorage key that store player setting          |


​		
<!--require 代表着這些參數是必須要使用的，其它的參數則可以根據自己需要配置。-->

<!--配置全局吸底，data-fixed 和 data-mini 也必須配置，配置為 true-->

<!--如果使用 Pjax，則在 class 裏需添加 no-destroy，這樣防止切換頁面時 Aplayer 被銷毀-->

把 aplayer代碼 插入到主題配置文件的 inject.bottom 去

```
inject:
  head:
  bottom:
```

運行 Hexo 就可以看到網頁左下角出現了 Aplayer

最後，如果你想切換頁面時，音樂不會中斷。請把主題配置文件的 pjax 設為 true
