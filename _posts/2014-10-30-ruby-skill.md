---
layout: post
title: "Ruby 技巧"
date: 2014-09-25 17:20:00
---

1. map(&:attribute) 遍历数组内对象的属性，实际调用的是 Symbol#to_proc {% highlight ruby %}
infos = Info.all
names = infos.map(&:name)   #等同于infos.map { |info| info.name }，返回结果是Array
{% endhighlight %}

2. try 无此对象会返回nil不会抛出异常 {% highlight ruby %}
User.try(:find, 1)
@person.try(:name)
{% endhighlight %}

3. send 向对象传递一个消息，消息是调用方法。是直接调用方法的另外一种方式  {% highlight ruby %}
d.send(:b,"bob")  #调用对象d的方法b，参数是"bob"
d   #调用对象d的方法c。
{% endhighlight %}  

4. tap 构建一个方法想返回一个 String / Array / Hash 之类结果{% highlight ruby %}
[].tap {|i| i << "abc"}   #["abc"]
''.tap {|i| i << do_some_thing }
{% endhighlight %}

5. 格式化浮点数字的精度显示 {% highlight ruby %}
money = 9.5
"%.2f" % money # => "9.50"
{% endhighlight %}

6. 抛出异常:可以在一行代码中直接抛异常而不中断当前调用栈{% highlight ruby %}
h = { :age => 10 }
h[:name].downcase # ERROR
h[:name].downcase rescue "No name" # => "No name"
{% endhighlight %}
