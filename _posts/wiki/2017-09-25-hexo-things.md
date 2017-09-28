---
title: Hexo笔记
layout: page
category: wiki
---
## 安装Hexo
```bash
npm install -g hexo-cli
```

## 创建Hexo项目
```bash
hexo init blog
cd blog
npm install
```

## 常用命令
```bash
hexo g  # generate
hexo s  # 本地预览
hexo d  # 部署网站
```

### 日常新建文章

### 部署
`_config.yml`中写好相应的配置
```
deploy:
  type: git
  repo: git@github.com:xxx/xxx.github.io.git
  branch: master
```

安装一个扩展

```
npm install hexo-deployer-git --save
```

通过`hexo d`，就能自动push上去，配好相应的GitHub库的设置，就可以替代原有的gitpages。

## 安装插件

## 设置404和about页面

## 主题配置

## 参考
[https://hexo.io/docs/index.html](https://hexo.io/docs/index.html)
[https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)
