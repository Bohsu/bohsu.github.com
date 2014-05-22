---
layout: post
title: "ruby on rails .nil?, .empty?, .blank?, .present?区别"
date: 2014-02-28  17:27:31
---

1. 区别

>.nil? 和 .empty? 是ruby的方法。.blank? 是rails的方法.   
>.nil? 判断对象是否存在(nil),不存在的对象都是nil.  
>.empty? 对象已经存在,判断是否为空,比如一个字符串是否为空串,或者一个数组中是否有值.  
>.blank? 相当于同时满足 .nil? 和 .empty? 。railsAPI中的解释是如果对象是: false, empty, 空白字符. 比如说： "", " ", nil , [], 和{}都算是blank.(object.blank? 相当于 object.nil? || object.empty?).  
>.present? 方法就是blank?方法的相反，判断是否存在，因此present?方法与!blank?方法两者表达的意思是一样的.  

                                                                                          
2. 示例  

* .nil?{% highlight ruby %}
nil.nil?        => true
false.nil?      => false
1.nil?           => false
0.nil?           => false
"".nil?          => false
[].nil?          => false{% endhighlight %}      

* .empty?{% highlight ruby %}
"".empty?          => true 
"abc".empty?       => false 
[].empty?          => true 
[1, 2, 3].empty?   => false
1.empty?           => NoMethodError  #说明 empty? 方法不能用于整数{% endhighlight %}      

* .blank?{% highlight ruby %}
true.blank?        =>false
false.blank?       =>true
"true".blank?      =>false
"".blank?          =>true
"\n".blank?        =>true
'\n'.blank?        =>false
'true'.blank?      =>false 
''.blank?          =>true
1.blank?           =>false
[].blank?          =>true 
[1].blank?         =>false{% endhighlight %}  

