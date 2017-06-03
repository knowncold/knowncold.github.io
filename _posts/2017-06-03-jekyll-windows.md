---
title: Windows使用Jekyll
layout: page
category: wiki
---
## 通过Bash来安装使用Jekyll

```Bash
sudo apt-get update -y && sudo apt-get upgrade -y

sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.3 ruby2.3-dev build-essential

sudo gem update

sudo gem install jekyll bundler
gem install jekyll-paginate

cd blog
jekyll serve
```

## 参考

[https://jekyllrb.com/docs/windows/](https://jekyllrb.com/docs/windows/)
