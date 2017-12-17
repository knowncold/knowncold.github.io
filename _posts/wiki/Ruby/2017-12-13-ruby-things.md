---
title: ruby
layout: page
category: wiki
hidden: true
---

单引号中不转义

```ruby
print("Hello\n")

print "Hello\n"

print "H", "e", "llo\n"


puts "Hello"        # 不会转义
puts "He", "llo"    # 自带两个换行
```

```ruby
# encoding:utf-8
=begin
comment block

=end

if a>=10 then
    print "aaa"
else
    print "bbb"
end

while i <= 10
    print "ccc"
    i = i + 1
end

100.times do
    print "ddd"
end
```

```bash
irb --simple--prompt
```

数学
```ruby
Math.sin(3.1415)

include Math

sqrt(10000)
```

变量嵌入字符串
```ruby
puts "area = #{ value }"
```
