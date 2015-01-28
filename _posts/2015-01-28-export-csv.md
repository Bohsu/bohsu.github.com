---
layout: post
title: Rails导出csv及显示中文乱码处理
date: 2014-12-26 15:27:31
---

* 导出csv如下{% highlight ruby %}
require 'csv'
    def to_csv(users)
      CSV.generate(:col_sep => '\t', :row_sep => '\r\n') do |csv|
        csv << ["登录名称", "真实姓名", "组织机构"]
        users.each do |user|
          csv << [user.login_name, user.real_name, user.categories_str]
        end
      end.encode('gb2312', :invalid => :replace, :undef => :replace, :replace => "?")
    end{% endhighlight %}

* 如上代码，Excel不识别utf8的文件头，在office excel中显示中文乱码处理{% highlight ruby %}
  encode('gb2312', :invalid => :replace, :undef => :replace, :replace => "?"){% endhighlight %}



使用参考:[https://ruby-china.org/topics/14306](https://ruby-china.org/topics/14306)
