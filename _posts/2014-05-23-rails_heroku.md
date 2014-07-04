---
layout: post
title: 使用Heroku部署Rails应用
date: 2014-05-23 18:05:00
---

###项目准备 

* 添加gem,并budle install{% highlight ruby %}
group :production do
      gem 'pg'
      gem 'rails_12factor'
end{% endhighlight %}  

###部署Heroku  

1. 注册帐号  
Heroku注册地址：https://id.heroku.com/signup

2. 安装 Heroku {% highlight bash %}
$ sudo gem install heroku{% endhighlight %}

3. 添加keys
  * 创建SSH keys,参考：https://help.github.com/articles/generating-ssh-keys
  * 如果已存在直接将其添加到heroku{% highlight bash %}$ heroku keys:add{% endhighlight %}  

4. 进入部署目录，clone并发布，避免开发时影响线上效果{% highlight bash %}
$ heroku login  
$ git clone URL{% endhighlight %}

5. 创建heroku{% highlight bash %} $ heroku create sinoxbwy{% endhighlight %}

6. 设置PostgreSQL {% highlight bash %}
$ heroku config #获取数据库地址  
#DATABASE_URL: postgres://XXXXX:XXXX.compute1.amazonaws.com:5432/XXXX{% endhighlight %}
  部署后无法连接数据库的问题解决参考:[这里](https://devcenter.heroku.com/articles/pre-provision-database)  

7. 发布到Heroku{% highlight bash %}
$ git push heroku master{% endhighlight %}

8. 部署{% highlight bash %}
$ heroku run rake db:migrate  
$ heroku ps:scale web=1  
$ heroku ps  
$ heroku open{% endhighlight %}

9. 查看log && 进入console{% highlight bash %} 
$ heroku logs  #查看logs
$ heroku run rails console #进入console{% endhighlight %}

参考:  
[https://devcenter.heroku.com/articles/getting-started-with-rails3](https://devcenter.heroku.com/articles/getting-started-with-rails3)  
[https://devcenter.heroku.com/articles/pre-provision-database](https://devcenter.heroku.com/articles/pre-provision-database)