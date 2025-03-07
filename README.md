# Mirai点歌插件
用mirai搜索音乐平台并分享音乐卡片。  
如果没有你常用的音乐平台或者你喜欢的音乐外观，欢迎发issue或者pr。    
如果有什么新的功能建议或者bug，也可以发issue，我会尽快查看更新。  
目前支持2.7+和0.5.2的API。旧版源代码在分支[console-0.5.2](https://github.com/khjxiaogu/MiraiSongPlugin/tree/console-0.5.2)   
![GitHub All Releases](https://img.shields.io/github/downloads/khjxiaogu/MiraiSongPlugin/total?label=%E4%B8%8B%E8%BD%BD%E9%87%8F&style=social)
![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/khjxiaogu/MiraiSongPlugin?label=%E5%BD%93%E5%89%8D%E7%89%88%E6%9C%AC)
![GitHub stars](https://img.shields.io/github/stars/khjxiaogu/MiraiSongPlugin?style=social)
# 文档目录
- [特色](#特色)
- [注意](#注意)
- [声明](#声明)
- [测试&食用方法](#基本使用方法)
- [指令列表](#默认指令列表)
- [配置方法](#配置项)
- - [自定义指令](#自定义指令)
- [权限系统](#权限系统)
- [配置指令](#插件指令)
# 特色
- 支持各大国内音乐平台。
- 使用纯java编写，无需安装其他外围的音乐平台支持库。
- 直接适配mirai-core库，不容易受到API更新的影响，同时支持更多灵活的API。
- 支持通过配置文件添加自定义的指令和覆盖现有指令[传送门](#自定义指令)。同时也可以配置部分提示信息，实现高度的自定义。
- 内置权限系统，可以灵活配置权限[传送门](#权限系统)。
- 支持高度自定义，采用MVC模式，添加音乐源和外观成本极低 [javadoc](https://khjxiaogu.github.io/MiraiSongPlugin/)。
# 注意
- 由于tx有某种风控机制，新注册的账号发送XML卡片可能出现发不出去的情况，请用电脑QQ之类的登录几天再发，具体时间取决于TX机制，与程序无关，望周知。
- 本插件均用新旧版本的Mirai测试过，如果不能运行请先确认你mirai为最新版本。   
- 语音功能需要调用命令行，可能有一些问题。  
- 由于各大音乐平台随时可能更新API，导致API失效，所以不保证所有API拿到手的时候均能正常使用。
- 本插件不会提供账户登录Cookie的功能，也不会设法跳过音乐平台的VIP限制。
- 部分音乐平台可能会只允许中国大陆的服务器使用该服务，如果要使用境外的服务器请自行准备代理等。
# 声明
- 本插件仅作为学习交流等使用，请勿用于盈利，否则法律后果自负。
- 本插件提供的所有API均来源于公开资料，如有侵权请发邮件或者issue联系进行删除。
- 使用该插件带来的任何后果均由使用者承担，作者概不负责。
- 如果遇到本插件任何问题，请发issue询问，其他平台概不受理。
- silk4j版本支持来自开源项目[silk4j](https://github.com/mzdluo123/silk4j)，请遵守开源协议。
### 测试可用的mirai-console版本
#### 0.5.2版本
- 0.5.2
#### 1.0版本
- 1.0-M3
- 1.0-M4
- 1.0-M4-dev3
- 1.1.0
#### 2.0版本    
- 2.0-M2-1-dev-1
- 2.7-M1-dev-5
- 2.7-M1
### 测试可用的mirai-core版本
1.2.1-2.7(每个发行版本都测试过)
# 基本使用方法
0. 从[Release](https://github.com/khjxiaogu/MiraiSongPlugin/releases)下载
1. 放置于plugins文件夹
2. 安装ffmpeg(如果不需要用语音功能可以跳过这步):<br>Windows:下载[ffmpeg](https://www.gyan.dev/ffmpeg/builds/ffmpeg-git-full.7z)，解压后把ffmpeg.exe放置于mirai同一目录<br>
Linux:   
```
  sudo apt install ffmpeg
  sudo apt install libavcodec-extra
```
3. 运行mirai，登录机器人
4. 在机器人所在群聊发送“#音乐 test”，机器人返回分享标签即为安装成功。
# 默认指令列表  
 
“#音乐 关键词”
-----
自动搜索所有源以找出来找最佳音频来源    

“#语音 关键词”  
-----
自动搜索所有源，以语音信息的形式发出

“#外链 关键词”  
-----
自动搜索所有源，以外链信息的形式发出

“#QQ 关键词”  
-----
搜索QQ音乐  

“#网易 关键词”  
-----
搜索网易云音乐  

“#网易电台 关键词”
-----
搜索网易云电台，一般来说是直接选择找到的第一个节目，但是关键词可以以 “电台名称|节目名称”的格式指定电台节目

“#酷狗 关键词”
-----
搜索酷狗音乐  

“#千千 关键词”
-----
搜索千千音乐（百度音乐）  

“#点歌 来源 外观 关键词”
-----
实验性API  
高度自定义的点歌方法  
|参数|值范围|用途|
|------|------|------|
|来源|QQ音乐<br>酷狗<br>千千<br>喜马拉雅<br>网易<br>网易HQ<br>网易电台<br>网易电台节目<br>Bilibili<br>本地|设定搜索歌曲的来源|
|外观|LightApp:小程序分享<br>Mirai:采用Mirai的MusicShare卡片，如果不存在则fallback为XML卡片<br>XML:卡片分享<br>Share:普通分享(不能播放)<br>Message:以纯信息形式分享，可以很方便取得音乐的各种链接。<br>AMR:AMR语音，需要配置好`ffmpeg_path`<br>Silk:SILK语音，需要同时配置好`silkenc_path`和`ffmpeg_path`，由于tx限流，质量可能很差（不推荐使用）|设定分享出来的音乐的外观|
# 歌曲API说明
- 网易电台和网易电台节目的区别是网易电台不能用“电台名称&#124;节目名称”的格式。
- 网易HQ的API和QQ音乐API均可能有时间限制，长时间后播放链接会失效。
- bilibili的API由于有请求校验，所以只支持语音播放，卡片分享只支持点击卡片打开网页进行播放。
# 配置项
配置文件位于`data/点歌插件`
|名称|介绍|
|-----|-----|
|`silkenc_path`|silk编码器文件位置[windows二进制](https://github.com/khjxiaogu/MiraiSongPlugin/blob/master/silk_v3_encoder.exe)|
|`ffmpeg_path`|ffmpeg编码器文件位置[ffmpeg github](https://github.com/FFmpeg/FFmpeg)|
|`amrqualityshift`|如果语音文件过大时进行的处理，缺省默认为false。<br>设置值：true/不断降低码率直到刚好能够发送，比较消耗性能 false/直接裁剪音频文件大小为1M|
|`amrwb`|是否启用amr_wb模式，缺省默认为true。<br>设置值：true/启用amr_wb，音质会比较好，但是电脑qq可能不能正常播放，手机qq进度条显示异常。 false/关闭amr_wb，此时`amrqualityshift`强制为false，音质会比较差，但是显示和播放都正常。|
|`use_custom_ffmpeg_command`|是否启用自定义ffmpeg指令，如果启用，上述的amr配置和ffmpeg路径配置将被忽略。|
|`custom_ffmpeg_command`|自定义ffmpeg命令，会把%input%替换为输入文件绝对路径，%output%替换为输出文件绝对路径。|
|`admins`|管理员QQ列表，对应QQ号可以使用插件设置命令`/msp`|
|`hintsongnotfound`|找不到歌曲文本提示|
|`hintcarderror`|分享失败文本提示|
|`hintnotemplate`|找不到卡片文本提示|
|`hintsourcenotfound`|找不到源文本提示|
|`enable_local`|全部搜索时是否包含本地搜索，默认不包含。|
|`verbose`|是否启用命令执行输出，缺省默认为true。<br>设置值：true/执行语音操作时在控制台输出详细信息 false/不输出信息，直接执行|
|`adddefault`|是否添加默认指令，缺省默认为true。<br>设置值：true/添加readme所述的指令列表 false/不添加任何指令|
|`extracommands`|通过配置添加新指令的列表，可以完全自定义指令。详见[后文](#自定义指令)|
# 自定义指令
范例：
```
extracommands: 
  "#语音": #指令名称
    source: all #搜索来源
    card: AMR #分享外观
  "#分享": 
    source: QQ音乐 #搜索来源
    card: Share #分享外观
```
|参数|值范围|用途|
|------|------|------|
|source|QQ音乐<br>网易<br>网易HQ<br>酷狗<br>千千<br>喜马拉雅<br>本地<br>Bilibili<br>all|设定搜索歌曲的来源<br>注意：all为搜索全部平台。|
|card|LightApp:小程序分享<br>Mirai:采用Mirai的MusicShare卡片，如果不存在则fallback为XML卡片<br>XML:卡片分享<br>Share:普通分享(不能播放)<br>Message:以纯信息形式分享，可以很方便取得音乐的各种链接。<br>AMR:AMR语音，需要配置好`ffmpeg_path`<br>Silk:SILK语音，需要同时配置好`silkenc_path`和`ffmpeg_path`，由于tx限流，质量可能很差（不推荐使用）|设定分享出来的音乐的外观|  

如果不需要原版的指令，可以设置配置项`adddefault`为false。  
原版的指令设置，仅供参考。__#点歌是特殊程序实现的，无法通过配置实现！__  
```
  "#音乐": 
    source: all
    card: Mirai
  "#语音": #指令名称
    source: all #搜索来源
    card: AMR #分享外观
  "#外链": 
    source: all
    card: Message
  "#QQ": 
    source: QQ音乐
    card: Mirai
  "#网易": 
    source: 网易
    card: Mirai
  "#酷狗": 
    source: 酷狗
    card: Mirai
  "#千千": 
    source: 千千
    card: XML
  "#网易电台": 
    source: 网易电台节目
    card: Mirai
```
# 权限系统
权限系统可以用于配置各个机器人、群和好友的权限。  
权限文件位于`data/点歌插件`目录下。
其中`global.permission`为默认权限配置，`<机器人qq号>.permission`为机器人单独配置项。
如果机器人不存在单独配置，则会使用默认全局配置。
首先引入下权限DSL：  
### 基础语法
权限DSL一行为一个语句，语法如下  
`[设置]<对象>[@<场所>]`  
其中中括号括起来表示不必须。尖括号括起来表示必须。  
`[设置]`为表示是否允许，`+`为允许，`-`为不允许，由于本插件是黑名单制度，默认写入后不允许，所以如果不填设置项表示不允许。  
`<对象>`表示权限作用于的对象，此选项是必须的。  
`[@<场所>]`表示权限实体使用指令的场所，@不能省略，不填表示在所有场所。  
关于实体和场所请参照后面的内容。  
### 注释
注释以#开始，可以在一行开始或者最后使用。
### 对象
对象可以是对应的QQ号，或者是如下几个关键字之一：
|符号|意义|
|------|------|
|`owner`|群主|
|`admin`|管理|
|`admins`|群主或者管理|
|`member`|无权限的群员|
|`members`|所有的群员|
|`friend`|好友，也适用于群聊中的好友|
|`stranger`|陌生人，也适用于群聊中的陌生人|
|`*`|所有|
### 场所
场所可以是群号，或者是如下几个关键字之一：
|符号|意义|
|------|------|
|`owner`|机器人任群主的群|
|`admin`|机器人任管理的群|
|`admins`|机器人任群主或者管理的群|
|`member`|机器人为群员的群|
|`*`|所有|
||省略则相当于写了`@*`|
### 总体含义
意义很显然，实体在(@)场所里面的权限为所设置的内容。  
以下是几个例子和解释：
```
+admins@* #表示所有群管理均可以使用
-member@* #表示所有群员均不可以使用
123456 #表示qq号为123456的人不可以使用
-* #表示默认所有人都不可以使用
+admins@admins #表示机器人是管理或者群主的群里的群主或者管理允许使用
+123456@233333 #表示允许群233333里的123456使用
-stranger #表示所有不是好友的对象
```
### 优先级
权限文件的优先级如下：
1. 具体的语句，即`+123456@233333`这种对象和场所均用qq号码指定的情况。
2. 对象为关键字但是场所用qq号指定的语句，如`+admins@233333`。
3. 对象为通配符`*`但是场所用qq号指定的语句，如`+*@233333`。
4. 场所为关键字但是对象用qq号指定的语句，如`123456@admins`。
5. 场所和对象均为关键字的语句，如`admins@admins`。
6. 对象为通配符`*`且场所为关键字的语句，如`+*@admins`。
7. 对象为qq号但场所未指定或者用通配符`*`的语句，如`-123456`。
8. 对象为关键字但是场所未指定或者用通配符`*`的语句，如`-stranger`。
9. 对象为通配符`*`且场所未指定或者用通配符`*`的语句，如`-*`。   

另外，如果有同等优先级的两个语句，写在后面的更加优先。如果出现了冲突的语句，以写在后面的为准。  
优先级规律可以概况为：__越详细的语句越优先，同级语句后写优先。__  
如，如果想要设置陌生人不允许私聊指令，只允许群聊指令，可以设置：  
```
-*
+friend
+members
```
即只允许好友和群内成员使用。  
如果想设置只有部分群可以使用，可以设置：
```
-*
+*@233333
+*@666666
+*@25252
```
即只允许这三个群里面的所有成员使用。
# 插件指令
插件有唯一而且不可覆盖的指令：`/msp`，本指令可用于配置插件配置，本指令只能在聊天中使用，请在admins里面设置管理员方可使用。
有如下子命令
|子命令|用法|功能|
|------|------|------|
|reload|`/msp reload`|重载插件的权限与配置文件|
|setperm|`/msp setperm <权限DSL>`|往本机器人权限文件里面添加这个权限语句，同时应用这个权限语句|
|setgperm|`/msp setgperm <权限DSL>`|往全局权限文件里面添加这个权限语句，同时应用这个权限语句|
|buildperm|`/msp buildperm`|整理导出权限文件，把目前已有的权限配置导出到文件并覆盖文件里面的配置|

为了性能和便利，用设置权限语句后仅仅是在最后加入新的语句，不会解决冲突和排序，用buildperm子命令即可排序权限语句同时解决冲突。
# 关于本地搜索(实验性)
本地搜索要在配置里面启用，否则搜索全部源不会搜索此项！  
插件运行之后会在mirai目录下产生`SongPluginLocal.json`文件，打开它就可以看到编辑范例为：  
```
[
{"title":"标题",
"desc":"副标题",
"previewUrl":"专辑图片url",
"musicUrl":"音乐播放url",
"jumpUrl":"点击跳转url",
"source":"本地"
}
]
```
格式为json数组，每个成员都必须是对象，最后不能有多余的,。如果指定了本地搜索或者全部搜索，那么就会通过标题的相似度筛选出要搜索的歌，并把所有参数填入分享卡片中分享。  
__#注意！这里的url必须是可以从外部访问的，请自备服务器或者云盘等，不能直接指定本地文件路径！__
