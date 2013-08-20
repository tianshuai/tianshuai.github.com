---
layout: post
title: "Nokogiri抓取失败后重试代码"
date: 2013-03-28 13:58
comments: true
categories: ruby 
---

非常喜欢Nokogiri的简洁，尤其是根据css和xpath查找元素。有时又觉得Nokogiri太简洁了，连抓取失败重试的机制都没有。可惜在景德镇，网络经常丢包，真是郁闷。
所以写这段代码，以后肯定常用。

```ruby
begin
  doc = Nokogiri::HTML(open(url).read.strip)
rescue Exception => ex
  log.error "Error: #{ex}"
  retry
end
```
Tips: retry可以跳回begin
这段代码将打印log并一直重试直到成功。估计这样写不大合适，因为一旦发生一个小错误，将会导致死循环。比较好的做法是，循环10次，如果都失败就放弃。

```ruby
#定义常量，最多循环10次
MAX_ATTEMPTS = 10

doc = nil
begin
  doc = Nokogiri::HTML(open(url).read.strip)
rescue Exception => ex
  log.error "Error: #{ex}"
  attempts = attempts + 1
  retry if(attempts < MAX_ATTEMPTS)
end

if(doc.nil?)
  # 尝试10次后都失败，在这里处理一下。
  # 以免后面处理doc时抛空指针异常
end
```
原文链接：http://rubyer.me/blog/537/
