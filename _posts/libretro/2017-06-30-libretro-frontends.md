---
title: Libretro 现代化的复古游戏平台
layout: page
category: tutorial
---

[Libretro](https://www.libretro.com/)是一个开源的模拟器平台，总体而言是一个前后端分离的大项目，一群能写模拟器的人按统一的API风格写模拟器核心，一群能写前端的人按照API写前端来调用核心，基于回调的方式，把回调得到的声音和每一帧图像通过某个平台上前端的合适方式渲染和播放出来。

在组织的[GitHub](https://github.com/libretro/)上可以看到很多相关的前端和后端核心的代码，后端的包括了不少复古的游戏机的模拟器，看这些代码库可以发现组织比较习惯于从某种游戏机的最好的模拟器fork过来，再改写API，以供前端调用，包括GBA、NES、PS等，在wiki上也能找到不少相关的核心开发的Guide。  
然而前端的相关的资料并不多，官方实现了两个版本的大型的前端，一个是RetroArch，一个是Lakka，两者的代码都非常之多...但是相关的开发Guide并没有找到，很难上手，想要把模拟器核心移植到安卓平台的难度很大，难度之一在于RetroArch虽然有Android版本，但是开发方式是C++和NDK那一套，整个代码库里面的Java代码少之可怜...

## 前端
> 在这个[版本库](https://github.com/Alcaro/misctoys/blob/master/libretrofrontlist.md)里面找到了资料

|name         | authors                  | status                   | technology, goals
|-------------|--------------------------|--------------------------|------------------
|bsnes-qt     | unknown (Themaister?)    | abandoned or canceled    | based on bsnes v072, used libsnes
|SSNES        | Themaister, Squarepusher | renamed, now RetroArch   | used libsnes
|lsnes        | Ilari, from TASVideos    | released, active?        | started as libsnes, unknown if it got upgraded
|LMSW         | Alcaro                   | released, abandoned      | [plugin for a level editor](http://s373.photobucket.com/component/Download-File?file=%2Falbums%2Foo178%2Falcaroops%2Fclip0003.mp4)
|RetroArch    | Squarepusher aka Twinaphex, Themaister | released, active | most fully featured, main driver for libretro expansions, most supported platforms, focused on HTPC-style setups
|XBMC / RetroPlayer  | garbear, others   | beta, active             | built inside a video player (but ffmpeg is a video player in libretro, so it's fair)
|ZMZ          | Alcaro                   | released, abandoned      | based on ZSNES' GUI
|minir        | Alcaro                   | no recent news           | win32/gtk3/other?, focused on WIMP setups
|nameless     | mudlord                  | canceled                 | WTL, statically linked core
|nameless     | 4chan         | no recent news, nor any news at all | unknown
|GNOME Games  | kekun, others?           | [going fairly well](https://wiki.gnome.org/Design/Playground/Games) | gtk3 only
|Arcan        | letoram                  | no recent news           | seems to be an entire window manager
|Phoenix      | Druage, athairus, others | active                   | Qt5, was previously a RetroArch launcher named Pantheon
|AnarchyArcade | SM_Sith_Lord            | [on Steam](http://store.steampowered.com/app/266430/) | Focused on virtual reality setups
|[NanoArch](https://github.com/heuripedes/nanoarch)  | heuripedes aka enygmata  | finished?  | extremely minimalist, intended to show how libretro works
|nameless     | Leiradel                 | ?                        | written in Lua
|[EmuVR](http://www.emuvr.net/about/) | website lags too much for me to figure that out | ? | another virtual reality setup, probably
|New Retro Arcade  | Digital Cyber Cherries  | released  | virtual reality again (doesn't acknowledge libretro, and costs $30, so not recommended)

其中比较推荐的是`NanoArch`，一千多行的代码确实能跑起来，其次是`minir`，代码量多了不少，但比起其他的还是容易的多了。
