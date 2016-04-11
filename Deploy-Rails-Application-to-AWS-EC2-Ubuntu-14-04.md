title: "Deploy Rails Application to AWS EC2(Ubuntu 14.04)"

date: 2016-01-14 23:38:32

tags: 部署 rails ubuntu

categories: Ops

---

今天想和大家分享的是如何将自己的Rails项目部署到EC2 instance(Ubuntu14.04).

## 创建instance
首先我们需要创建一台EC2 instance作为我们的server，新注册的用户会有一年免费使用的优惠。这里我们创建了一个基于Ubuntu的instance。

---

登录AWS console，进入到EC2 Service的channel，点击左侧的instance.点击`Lunch Instance`,进入到创建Instance页面。

![create_instance_1](/img/create_instance._1.png)

这里我们选择Ubuntu14.04作为我们的Server.

在`Step 2: Choose an Instance Type`中，默认即可

在`Step 3: Configure Instance Details`时，我们设置如下值：

![config_instance.png](/img/config_instance.png)

在`Step 4: Add Storage`中，默认8G即可

在`Step 5: Tag Instance`中，设置我们instance的名字，这里我们设置为`ubuntu-server`

在`Step 6: Configure Security Group`中，设置Security Group，这里我们新创建一个。同时，`Add Rule` -> `HTTP`

![config-sercury-group](/img/config-sercury-group.png)

在`Step 7: Review`结束时，我们要选择或者创建一个`key pair`,用来登录你的`instance`,最后点击`Lunch Instance`,创建一个`Instance`

![review](/img/review.png)

回到我们的`EC2 Instance`的页面，可以看到我们的`Instance`已经创建成功了。下面可以看到我们的`Instance`的`public IP`

![instance_ip](/img/instance_ip.png)

## 登录到Instance

我们通过`ssh`的方式登录到这台server上,`fugang-key-pair-oregon.pem`是我们创建key pair时的文件，我们需要通过它来进行身份认证，才能登录到我们的Server上。

```
ssh -i ~/Desktop/fugang-key-pair-oregon.pem ubuntu@52.34.196.58
```

一般情况下，我们都会创建一个新的用户去部署我们的项目，这里我们创建`deploy`的用户。

```
sudo adduser deploy
sudo adduser deploy sudo
su deploy
```

## Install Ruby

首先我们安装一些`Ruby`依赖的包

```
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
```

我们使用[rbenv](https://github.com/rbenv/rbenv)的方式来安装Ruby，当然你也可以通过[Rvm](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-on-ubuntu-14-04-using-rvm)的方式.

```
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

rbenv install 2.1.5
rbenv global 2.1.5
ruby -v

echo "gem: --no-ri --no-rdoc" > ~/.gemrc
gem install bundler
```
## Install Nginx

我们使用[Phusion Passenger](https://www.phusionpassenger.com/library/)来帮助我们部署Rails程序，使用`Nginx`作为我们的服务器。

```
# Install our PGP key and HTTPs suport for ATP
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

# Add our APT repository
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

# Install Passenger + Nginx
sudo apt-get install -y nginx-extras passenger
```

我们现在已经安装了`nginx`和`passenger`,我们可以通过下面的命令启动/停止/重启我们的服务器。

```
sudo service nginx start/stop/restart
```
现在你可以在浏览器中打开输入你的ip，你应该可以看到`Nginx`的欢迎页面啦~

接下来，我们需要修改Nginx的配置文件，将passenger_ruby指向我们自己安装之后的ruby。

我们用vim打开`/etc/nginx/nginx.conf`文件，找到下面的两行配置，并作修改：

```
passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;

passenger_ruby /home/deploy/.rbenv/shims/ruby; # If you use rbenv
```

完成配置的修改之后，我们需要重启`nginx server`，使新配置生效。

## 数据库安装

这里我们使用[Postgres](www.postgresql.org)数据库作为我们生产环境的数据库。我们可以通过如下命令去安装它。

```
# install the postgres

sudo apt-get install postgresql postgresql-contrib libpq-dev

# setup postgres user

sudo su - postgres
createuser --pwprompt
exit
```

* 一定要记住自己设置的数据库的密码，等会要在server上的`my_app/current/config/database.yml`数据库配置文件内使用。

# Capistrano setup

[`Capistrano`](http://capistranorb.com/)是一个自动化部署应用的framework，它本身使用ruby语言写的，但是它可以支持任何一种语言的部署。

##添加`capistrano`到本地项目中

需要将下面的内容添加到你的Gemfile中

```
gem 'capistrano', '~> 3.1.0'
gem 'capistrano-bundler', '~> 1.1.2'
gem 'capistrano-rails', '~> 1.1.1'

gem 'capistrano-rbenv', github: "capistrano/rbenv"

```

添加完成之后我们需要运行`bundle install`去安装我们的依赖，然后执行

```
cap install STAGES=production
```

生成`capistrano`的配置文件

## 配置capistrano

我们需要修改`capistrano`的配置文件： Capfile

```
require 'capistrano/bundler'
require 'capistrano/rails'

require 'capistrano/rbenv'
set :rbenv_type, :user # or :system, depends on your rbenv setup
set :rbenv_ruby, '2.1.5'
```

接下来我们需要配置`config/deploy.rb`文件，配置一些部署时需要的变量。下面是我的配置，可以参考。

```
lock '3.4.0'

set :application, 'myblog'
set :repo_url, 'git@github.com:fgcui1204/myblog.git'
set :branch, 'master'

set :deploy_to, '/home/deploy/myblog'
set :deploy_user, 'deploy'
set :rbenv_type, :deploy
set :rbenv_ruby, "2.1.5"
set :rbenv_prefix, "RBENV_ROOT=#{fetch(:rbenv_path)} RBENV_VERSION=#{fetch(:rbenv_ruby)} #{fetch(:rbenv_path)}/bin/rbenv exec"
set :rbenv_map_bins, %w{rake gem bundle ruby rails}
set :rbenv_roles, :all # default value

set :linked_files, %w{config/database.yml}
set :linked_dirs, %w{bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system}

set :keep_releases, 5


set :linked_files, %w{config/database.yml config/secrets.yml}
set :linked_dirs, %w{bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system}

namespace :deploy do
  desc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      execute :touch, release_path.join('tmp/restart.txt')
    end
  end

  after :publishing, 'deploy:restart'
  after :finishing, 'deploy:cleanup'
end
```

接下来，我们需要配置我们的server的ip。打开`config/deploy/production.rb`

```
set :stage, :production
set :rails_env, :production
server '52.35.187.182', user: 'deploy', roles: %w{web app db}
```

## 最后

### 配置nginx，指向App

登录到`instance`上，我们需要配置`Nginx Server`， 当它响应时，指向我们的`server`。 打开`/etc/nginx/sites-enabled/default`
修改下面的内容：

```
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        server_name mydomain.com;
        passenger_enabled on;
        rails_env    production;
        root         /home/deploy/myblog/current/public;

        # redirect server error pages to the static page /50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
}
```

### 部署

将本地代码`push`到`github`上之后，执行`cap production deploy`会将程序部署到我们的`server`上。第一次部署的时候，你应该会看到一些错误，`No database`。因为`capistrano`不会
帮我们自动的创建数据库，我们我们需要等到`instance`上手动的去创建。创建成功之后，再次执行`cap production deploy`,重新部署我们的项目。


## 最后的最后
这篇博客主要是参考[Deploy Ruby On Rails on Ubuntu 14.04](https://gorails.com/deploy/ubuntu/14.04)来写的，自己也亲自手动做了一次。大家可以参考我的项目[配置](https://github.com/fgcui1204/myblog)
来进行操作.

如有问题，欢迎评论。
