---
title: 树莓派上的Make并行计算
layout: page
category: tutorial
---

> 我们已经习惯于听到说树莓派改革了原有的教育和创客社区，但事实上树莓派2的开发过程中，他同时参与了计算机历史上另一个改革浪潮：从单核计算机到多核计算机的转变。  
> 这会在最根本的程度上改变我们写程序的思维，树莓派2和3上面都搭载着一个四核的处理器，四核也就意味着可以同时运行四个任务；理论上来说，对于同一个程序而言这让我们可以有四倍于过去的速度，但是实际上我们很难去使用去真正地发挥这个多出来的计算资源。在这篇文章中，我们会稍稍地介绍一个最简单的使用树莓派四核性能的并行计算的方法。

> 翻译自*MagPi 52*

## 三核处理器的早餐

在我们开始写程序之前，我们先来看一下我们每天都会做的一件事：做早饭。如果我们尝试着去描述一下这个做早饭过程，那么他看起来可能是右图这样的一个列表。

这个列表呢，他是一个很明显的顺序执行的程序，他包括了所有做早饭的必要的工作，但是还遗漏了一些重要的事情。  
事实上，我们永远不会像这个列表一样照着这个顺序一个一个任务地做下去，我们会同时地做几件事情，比如我们可以同时煮水和烤面包片；当然也会有一些事情不能同时做，比如只有当你煮完水才能泡茶喝。

![](http://o73wiy9vn.bkt.clouddn.com/make-on-rpi-1.png)

所以我们可以换一种方式来描述这个做早饭的任务，就像下面这个图一样：

![](http://o73wiy9vn.bkt.clouddn.com/make-on-rpi-2.png)

我们从上而下的做这些小任务，当每一个上面的任务做完之后，我们就能接着做他下面的那个任务，显然地，这样做起来速度会比前一种列表的顺序快很多。  
当然，不是所有的任务都可以分解成小任务，也不是所有的小任务可以这么并行地来完成。

## 使用Make

那么在树莓派2和3上，我们怎么来做到类似的分解任务，然后并行运行呢？

`Raspbian`这个操作系统上，原生的就有一个程序叫做`Make`，这个程序按照一个有依赖的任务列表，按一定的顺序来执行每项任务，恰好他支持在树莓派上的并行计算。  
`Make`这个程序本身一开始被设计为给C语言或者C++语言这些需要编译源代码的程序语言提供各种依赖和编译的顺序，而现在他也支持执行各种只要能够被他描述的有依赖性的任务。  
想要使用`Make`，我们需要编写一个`makefile`来描述每项任务之间的依赖，作为一个例子，我们会写一个程序，用来把一堆图片的缩略图拼凑成一个大图，这个程序可以用来处理大量的图片文件，比如在一个服务器上处理大量图片，我们就能粗略地查看一下这些缩略图了。  
除了`Make`，我们还要使用一个叫做`ImageMagick`的程序来转换图片，用下面的`Bash`代码来确保我们的程序里面已经有`ImageMagick`和`Make`了:

```
sudo apt-get update
sudo apt-get install make imagemagick
```

我们打算先使用Make来生成每张原始图片的缩略图，最后再把他们拼凑起来，这个程序的主要工作就在于生成缩略图，依赖图如下所示：

![](http://o73wiy9vn.bkt.clouddn.com/make-on-rpi-3.png)

这个图展示了缩略图、原始图和最后的拼图之间的关系，因为每个缩略图的生成过程都是独立进行的，所以我们可以让树莓派的四个核同时投入生成缩略图的这个工作，然后当缩略图都生成完全的时候，就可以进行下一步生成拼图了。

## Makefile的编写规则

Makefile文件是我们用来描述所有任务和他们之间的依赖关系的，首先，他定义了任务相关的原始图片和缩略图的列表，然后我们在图里定义了两个依赖规则。  
第一个规则描述了如何从原始图片生成缩略图:`fullsize`这个文件夹里面所有的图片都要通过`convert`这个命令来转换成`thumbs`文件夹里面的同名文件。需要注意的是，包含命令的那几行必须要用tab键的缩进开头  
第二个规则是明确了最后的拼图必须依赖于前面的所有缩略图，使用了`montage`命令来创建拼图并显示。我们已经告诉Make哪些命令用来缩小尺寸，那些命令用来拼凑图片，但是我们还没有明确一个执行的顺序，Make会自己计算出一个合适的顺序，毕竟我们已经从一个单一顺序的程序描述转换成描述并行任务和互相的依赖了，系统已经能自己想出最合适的最高效的执行顺序了。

```makefile
# Thumbnail size in pixels
SIZE = 128x128
 
# The list of original photos to use (fullsize/* refers to all files
# in the directory fullsize)
ORIGINALS = $(wildcard fullsize/*)
 
# Use the list of originals to build a list of thumbnails (this takes
# the list of originals and changes the prefix on each file from
# 'fullsize' to 'thumbs')
THUMBS = $(ORIGINALS:fullsize/%=thumbs/%)
 
# RULE 1: Generate each thumbnail from its original using the convert
# utility from ImageMagick, rotating the image if necessary
thumbs/% : fullsize/%
        convert $< -thumbnail $(SIZE) -auto-orient $@
 
# RULE 2: Combine all the thumbnails into the montage and display it
montage.jpeg: $(THUMBS)
        montage $(THUMBS) montage.jpeg
        display montage.jpeg &
 
# Clean up all thumbnails and delete the montage
clean:
        $(RM) thumbs/* montage.jpeg
```

最简单的运行方法就是运行`make`

这会启动`Make`程序，然后读取我们在`makefile`里面写完了的相应规则，并开始按照正确的顺序执行命令；但是，`Make`一开始会假定可用的处理器核心只有一个，也就是说仍然一个一个的慢慢地执行下去。  
当然我们可以很方便告诉`Make`我们有四个核：

```
make -j4
```

这个命令明确了`Make`会同时执行四个任务，所以通过这种方法的话，我们的程序速度会是之前的四倍左右，现在让我们在`fullsize`文件夹里面添加一些新的图片然后运行`Make`吧，它可以检查每个文件的时间戳，确定哪些工作是必要，哪些工作是不需要做的（假如之前生成过一次缩略图，第二次就不需要重复了），这也就意味着，在`Make`运行到一半的时候，我们可以随意的中断，然后下一次继续。  
运行`make clean`就会清空所有的缩略图和拼图。
