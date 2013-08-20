---
layout: post
title: "rails3 在线文档－－实时更新"
date: 2013-08-20 19:21
comments: true
categories: rails
---

*	是否是开发环境?
```ruby
if Rails.env.development?
```

*	智能判断单复数
```ruby
pluralize(count, "error")
```

*	上传文件名的话，用 userid + fileid + timestamp 取 sha1 就能解决唯一性了

*	把数字转换为字节(用于精确判断文件或图片大小)
```ruby

1.kilobyte #千字节
#=>1024
5.megabytes  #上传文件限制字节数５Ｍ
#=> 5242880
```

*	判断表单是新建还是修改的
```ruby
User.new.new_record?
#=>true
User.first.new_record?
#=>false
```
<!--more-->

*	友好显示时间
```ruby
time_ago_in_words(model.created_at)
```

*	在GET方法中使用stale?语句
```ruby
def show  
  @list_item = @list.list_items.find( params[ :id ] )  
  if stale?( :etag => @list_item, :last_modified => @list_item.updated_at.utc, :public => true )  
    respond_with( @list_item )  
  end  
end

=begin
stale?语句会通过响应发送回一个etag与一个last_modified日期。如果下一个请求是相同的URL，
那么浏览器会把这个etag和last_modified日期发送给服务器。然后stale?方法会对这两个参数进行分析，
如果内容相同，则返回304，如果出现参数值不同，那么说明有新的内容，这样返回200。
=end
```

*	成生项目时如果用mongoid则：
```ruby
rails new app_name --skip-active-record
```

*	解析html
```ruby
raw ('string')
'string'.html_safe
```

*	rails3日期选择框:
```ruby
<%= date_select :variable, :attribute, options %>
<%= datetime_select :variable, :attribute, options %>
```

*	格式化字符串
```ruby
"Person".tableize      # => "people"
"Invoice".tableize     # => "invoices"
"InvoiceLine".tableize # => "invoice_lines"

```
