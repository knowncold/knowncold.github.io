---
title : 修改Ubuntu的开机壁纸
layout : wiki_page
category : wiki
---
Ubuntu的开机画面有两种情况，一种是当你在桌面选择静态壁纸的时候，开机登录画面就是那一张壁纸。 
另一种是你选择了它的轮换壁纸，这时候即使你改好了轮换壁纸的图片，然而开机画面还是它自带的那个很丑的红色壁纸，修改方法:

	$ cd /usr/share/backgrounds

把你想要的图片复制到这个路径，似乎需要是png格式的，然后改成`warty-final-ubuntu.png` 这个名字
重启之后就可以看到效果
