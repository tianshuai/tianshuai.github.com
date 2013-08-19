---
layout: post
title: "ruby 进阶"
date: 2013-03-28 14:48
comments: true
categories: 
---

1.下载网页中图片
```ruby
require 'net/http'
Net::HTTP.start("www.google.com.hk") { |http|
  resp = http.get("/images/srpr/nav_logo27.png")
  open("D:/test.png", "wb") { |file|
    file.write(resp.body)
   }
}
puts "OK"
```
<!--more-->
2.Nokogiri 中文乱码解决技巧
Nokogiri在抓取网页操作性和速度方面的确非常棒，但中文乱码一直让很多人头痛，老宋最近在写一个抓取器的时候做了一个总结，汇总成6条提示，如果你也遇到乱码问题，不妨试一下:)

提示1：在文件头指定程序编码
在***.rb头上添加,注意：一定要在第一行，中间添加无效

```ruby
#coding: utf-8
提示2:url有中文要进行urlencode编码

url=URI.escape("http://g.cn?q=中国")
提示3：显式设置要抓取目标的编码

doc = Nokogiri::HTML.parse(open("http://rubyer.me/"), nil, "UTF-8")
提示4: 使用Iconv.iconv(to_encoding,from_encoding,str_to_conv)手动转换编码

puts Iconv.iconv("UTF-8", "GBK", doc)
提示5: 如果不确定目标是什么编码，Ruby1.9开始可以用String类内置的encoding来得到编码。

puts Iconv.iconv("UTF-8", doc.to_s.encoding.to_s, doc)
提示6: 使用//IGNORE忽略无法转换的字符

Iconv.iconv("UTF-8//IGNORE", "GBK//IGNORE", doc)
```

法一、结合meta_encoding和Iconv
```ruby
#coding: utf-8
require 'open-uri'
require 'nokogiri'
require 'iconv'

#这个url是百度一个快照的地址，直接拿来做例子。
url = "http://cache.baidu.com/c?m=9f65cb4a8c8507ed4fece763105690365203c0743ca08f426284cd15c6790a120131b6e667690d44809e222615ea141cbcff&p=817bc45b87934eac5fa8c7710a0d&user=baidu&fm=sc&query=site%3Arubyer%2Eme&qid=b351e91d4dc50477&p1=2"

response = open(url).read
response = Nokogiri::HTML.parse(response)
response = Iconv.conv("utf-8", "gb2312", response)
puts respons
```
执行结果：

  ?7?4  cache_spider git:(master) ?7?1 ruby temp.rb
  temp.rb:11:in `conv': "\xF8?0?0\xB5?0?4\xB4?0?2?0?7\xC3档)\r\n\r\n\r\n"... (Iconv::IllegalSequence) from temp.rb:11:in `<main>'

法二、直接猜编码为utf-8

```ruby
# 省略url前代码
response = open(url).read
response = Nokogiri::HTML.parse(response,nil,"utf-8")
puts response
```
能输出HTML，但基本乱码，类似：

  <p>?0?6?0?8?0?5°?0?9?0?8model?0?0?0?6?0?2?0?1?0?6?0?3?0?0?0?8?0?6?0?3?0?0±?0?8?0?5?0?1¨?0?1?0?5?0?4?0?7?0?0?0?2migration?0?8?0?7?0?8?0?8?0?3?0?0?0?5?0?1?0?7?0?7?0?9?0?9ruby-china?0?8?0?2?0?8 ?0?7?0?0?0?5?0?2ó?0?0?0?4?0?6?0?1?0?2?0?5?0?5ù?0?4?0?7?0?6?0?4?0?1?0?4?0?5¨?0?6?0?4?0?8?0?5?0?6?0?6?0?7?0?9?0?8±?0?3á±?0?6?0?5?0?3?0?0é·?0?6?0?5?0?1?0?3?0?1?0?8?0?5·?0?3?0?8?0?5model?0?8?0?7?0?2?0?3?0?8?0?8?0?3?0?5?0?7?0?3?0?8?0?5?0?0?0?3?0?3?0?6?0?2?0?7?0?8?0?2model?0?0í?0?4?0?7?0?9?0?9before_create?0?5?0?1?0?6?0?3?0?2ó?0?8?0?9?0?4?0?4rake db:s...  </p>

法三、直接猜编码为utf-8
```ruby
# 省略url前代码
response = open(url)
response = Nokogiri::HTML.parse(response,nil,"gb2312")
#response = Nokogiri::HTML.parse(response)
#response = Iconv.conv("utf-8", response.meta_encoding, response)
puts response
```
输出：

 ?7?4  cache_spider git:(master) ?7?1 ruby temp.rb
 output error : unknown encoding gb2312

法四、试了以上3种方法都失败后，我才想自己处理编码。
```ruby
def convert_encoding(source_encoding, destination_encoding, str)
  ec = Encoding::Converter.new(source_encoding, destination_encoding)
  begin
    ec.convert(str)
  rescue Encoding::UndefinedConversionError
    p $!.error_char.dump
    p $!.error_char.encoding
  rescue Encoding::InvalidByteSequenceError
    p $!
    p $!.error_bytes.dump  if $!.error_bytes
    p $!.readagain_bytes.dump if $!.readagain_bytes
  end
  str
end
```
试了gbk, utf-8, gb2312各种组合，都不完美。
```ruby
html = open(url).read
html.force_encoding("gbk")
html.encode!("utf-8")
doc = Nokogiri::HTML.parse html
doc.css("body")
```
 
判断一个段文本是否是UTF-8编码：

```ruby
class String  
 def utf8?    
　　　unpack('U*') rescue return false    
 　　true    
　end  
end  
```

哈希数组排序
test.sort_by {|t| t[:x]}.inject([]) {|r,h| r<<h if !r.last||r.last[:x]!=h[:x]; r}
