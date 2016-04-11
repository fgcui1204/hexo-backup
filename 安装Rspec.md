title: "安装Rspec"

date: 2016-01-31 15:34:47

tags: rails rspec

categories: rspec
---
## 安装rspec
项目中需要使用rspec，首先安装rspec。

在Gemfile中添加下面的gem依赖。

```
group :development, :test do  
  gem 'rspec-rails', '~> 3.1.0'  
  gem 'factory_girl_rails', '~> 4.4.1'
end
group :test do  
  gem 'faker', '~> 1.4.3'  
  gem 'capybara', '~> 2.4.3'  
  gem 'database_cleaner', '~> 1.3.0'  
  gem 'launchy', '~> 2.4.2'  
  gem 'selenium-webdriver', '~> 2.43.0'
end
```
## 创建测试数据库
在config/database.yml文件中有
```
test: 
  adapter: sqlite3
  database: db/test.sqlite3
  pool: 5
  timeout: 5000
```
检查完之后，执行
```
rake db:create:all
```
会帮我们创建测试数据库
## 设置rspec
通过下面的命令设置rspec，创建rspec文件夹
```
rails generate rspec:install
```
