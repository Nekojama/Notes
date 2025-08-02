---
publish: true
icon: LiCheck
---

<div class="printable-title-container">

```dataviewjs
const repoUrl = "https://github.com/Nekojama/Notes";
const fileName = dv.current().file.name;
const mtime = dv.current().file.mtime.toFormat("yy-MM-dd");
const fullHtml = `
<div class="printable-title-container">
  <h1 class="printable-title">${fileName}</h1>
  <div class="custom-divider"></div>
  <a href="${repoUrl}" target="_blank" class="printable-version-info">
    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
    <span>检查更新</span>
    <span class="version-date-badge">${mtime}</span>
  </a>
</div>
`;
dv.paragraph(fullHtml);
```
</div>

你是否有过这样耳虫犯了的时候:当你全神贯注地写笔记时,耳边却突然响起了某首歌的旋律?或者,你在写的某篇笔记一定要搭配某首歌食用?  
为了解决这样的问题,我们可以利用Obsidian支持HTML的特性,在我们的笔记中使用`<iframe>`插入音乐.

>[!faq] 什么是`<iframe>`?
>`<iframe>`是一个用于在当前页面内插入HTML页面的元素.
>如果你想了解更多,参见[HTML iframe tag](https://www.w3schools.com/tags/tag_iframe.asp).

下面我们进入在Obsidian中插入音乐的实战.
**注意**:由于个人目前音乐软件只用网易云,所以下面都是用**网易云音乐**作为例子.

## 在Obsidian中插入一般单曲
1. **获得单曲的外链**
	进入网易云的网页界面:[网易云音乐](https://music.163.com/),登录,点开你想插入到笔记内的音乐的播放页面.接着你应该可以看见**生成外链播放器**的字样,点进去便能找到外链播放器的**HTML代码**,尺寸任选.这就是我们需要的,但是需要一定的更改.
	我们在这里选用シーズ的Monochromatic作为例子.
2. **将HTML代码进行些许修改**
	 此时你得到的代码应该是这样的(为了美观做了排版):
	 ```html
	 <iframe 
	 frameborder="no" 
	 border="0" 
	 marginwidth="0" 
	 marginheight="0" 
	 width=330 
	 height=86 
	 src="//music.163.com/outchain/player?type=2&id=2635627532&auto=1&height=66">
	 </iframe>
	```
	如果你尝试直接将这个代码复制到Obsidian中使用,那么就会出现播放器不显示的问题.这是因为 `src` 使用协议相对路径 `//music.163.com/...`,在本地文件环境(`file://` 协议)下会被解析为 `file://music.163.com`,这显然是无效的.
	不过,我们先认识一下代码的结构,然后再针对它进行修改.下面是`<iframe>`自身的属性:
	
	|属性|值|说明|
	|---|---|---|
	|`width`|330|嵌入内容宽度（像素）|
	|`height`|86|嵌入内容高度（像素）|
	|`frameborder`|"no"|隐藏边框|
	|`border`|"0"|边框宽度为0|
	|`marginwidth`|"0"|内容与边框左右无间距|
	|`marginheight`|"0"|内容与边框上下无间距|
	|`src`|[URL](//music.163.com/outchain/player?type=2&id=2635627532&auto=1&height=66)|嵌入内容的资源地址|
	然后是我们的外链的属性:
	
	|参数|值|说明|
	|---|---|---|
	|`type`|2|播放器样式类型|
	|`id`|2635627532|歌曲/歌单ID|
	|`auto`|1|自动播放|
	|`height`|66|播放器内部高度（像素）|
	
	首先,我们应该在URL前添上`https:`,以使其成为网页的地址:`http://music.163.com/outchain/player?type=2&id=2635627532&auto=1&height=66`.在浏览器里打开[修改好的地址](http://music.163.com/outchain/player?type=2&id=2635627532&auto=1&height=66),如果能正常播放,我们就成功了99%了.接下来是一些易用性的更改:
	- 添加`style="display: block;"`到代码中,这会使得播放器与文字间距缩小.
	- 将`auto=1`更改为`auto=0`,实现不自动播放.这样做是因为Obsidian的编辑模式和阅览模式是分开的,打开自动播放会导致重复的情况.
	
	做完这些工作,我们就能得到一个可以插入到Obsidian内的外链播放器了,效果如下:
	
	<iframe
	style="display: block;"
	frameborder="no" 
	border="0" 
	marginwidth="0" 
	marginheight="0" 
	width=330 
	height=86 
	src="https://music.163.com/outchain/player?type=2&id=2635627532&auto=0&height=66">
	</iframe>

可是当我们想要将歌单,或者有些没有办法直接生成外链的单曲插入到笔记中时,我们又该怎么做呢?方法很简单:只需要生成**播客**的外链就可以了.具体方法和一般单曲的操作几乎无异,只是生成单曲时需要提供对应的**单曲的id**,在此不多赘述.

另外,外链播放器除了插入音乐,还可以插入视频等等HTML页面,例如[十六夜カズヤP](https://space.bilibili.com/1357137?spm_id_from=333.1391.0.0)的:
[4K HDR「SOS」(黛冬優子 solo)【偶像大师闪耀色彩 Song for Prism MV】](https://www.bilibili.com/video/BV1vC4y1M7aS/?share_source=copy_web&vd_source=aa9b1263fec292f540c16325a7886838).(如有侵权我会删除)
<iframe 
  src="https://player.bilibili.com/player.html?isOutside=true&aid=750097381&bvid=BV1vC4y1M7aS&cid=1374646097&p=1&autoplay=false" 
  width="800" 
  height="450" 
  scrolling="no" 
  border="0" 
  frameborder="no" 
  framespacing="0" 
  allowfullscreen="true">
</iframe>
但是这样得到的外链播放器是没有登陆也没办法登陆的,清晰度最多只有360P.

