---
layout: post
title: "Rails后台任务Sidekiq的使用"
date: 2014-06-27 17:10:00
---

###使用sidekiq需先安装redis 

1. 新建或者进入存放redis的目录，下载，解压和安装： {% highlight bash %}
$ wget http://download.redis.io/releases/redis-2.8.11.tar.gz
$ tar xzf redis-2.8.11.tar.gz
$ cd redis-2.8.11
$ make
$ sudo make install   #这时Redis的可执行文件被放到了/usr/local/bin{% endhighlight %}

2. 下载配置文件和init启动脚本： {% highlight bash %}
$ wget https://github.com/ijonas/dotfiles/raw/master/etc/init.d/redis-server
$ wget https://github.com/ijonas/dotfiles/raw/master/etc/redis.conf
$ sudo mv redis-server /etc/init.d/redis-server
$ sudo chmod +x /etc/init.d/redis-server
$ sudo mv redis.conf /etc/redis.conf
{% endhighlight %}

3. 初始化用户和日志路径  
第一次启动Redis前，建议为Redis单独建立一个用户，并新建data和日志文件夹{% highlight bash %}
$ sudo useradd redis
$ sudo mkdir -p /var/lib/redis
$ sudo mkdir -p /var/log/redis
$ sudo chown redis.redis /var/lib/redis
$ sudo chown redis.redis /var/log/redis
{% endhighlight %}  

4. 进设置开机自动启动，关机自动关闭{% highlight bash %}
$ sudo update-rc.d redis-server defaults
{% endhighlight %}

5. 启动Redis{% highlight bash %} $ sudo /etc/init.d/redis-server start{% endhighlight %}


###rails使用sidekiq 
1. 添加gem包，并bundle install {% highlight bash %}
# 后台任务
gem 'sidekiq'
# 和sidekiq的monitor相关
gem 'slim'
gem 'sinatra', '>= 1.3.0', :require => nil{% endhighlight %}

2. 创建Worker: app/workers/your_worker.rb{% highlight ruby %}
class YourWorker  
  include Sidekiq::Worker  
  def perform(name)  
    puts 'Doing hard work'   
  end  
end{% endhighlight %}  

3. 在controller或者model中调用YourWorker{% highlight bash %}
YourWorker.perform_async('yggc'){% endhighlight %}

4. 在config/routes.rb中添加监控页面路由，访问/sidekiq{% highlight bash %} 
require 'sidekiq/web'
# 后台任务页面
mount Sidekiq::Web => '/sidekiq'{% endhighlight %}

5. 在config/中添加sidekiq.yml

6. 启动服务{% highlight bash %}
$ bundle exec sidekiq -C config/sidekiq.yml 调试启动方式
$ bundle exec sidekiq -C config/sidekiq.yml -d 后台启动方式
$ bundle exec sidekiq -C config/sidekiq.yml -d -e production 指定环境启动{% endhighlight %}

7. 停止服务{% highlight bash %}
bundle exec sidekiqctl stop tmp/pids/sidekiq.pid{% endhighlight %}

参考:  
redis:  
[http://www.redis.cn/download.html](http://www.redis.cn/download.html)  
[http://rubyer.me/blog/638](http://rubyer.me/blog/638)  

sidekiq:  
[http://mperham.github.io/sidekiq](http://mperham.github.io/sidekiq) 
[http://railscasts.com/episodes/366-sidekiq](http://railscasts.com/episodes/366-sidekiq) 
[https://github.com/mperham/sidekiq/wiki](https://github.com/mperham/sidekiq/wiki) 


