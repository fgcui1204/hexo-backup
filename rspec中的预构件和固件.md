title: "rspec中的预构件和固件"

date: 2016-01-31 21:36:42

tags: rspec

categories: rspec

---

## 固件
固件：Rails 默认提供了快速生成示例数据的工具,叫做“固件”。
    固件是一个yaml格式的文件，可以用来生成示例数据。
    例如下面是一个`Contact`model的固件：

```
# contact.yml
aaron: 
  firstname: "Aaron" 
  lastname: "Sumner" 
  email: "aaron@everydayrails.com"
john: 
  firstname: "John" 
  lastname: "Doe" 
  email: "johndoe@nobody.org"
```
在测试中，我们使用contact(:aaron),就会得到一个联系人。

固件有两个问题我是极力避免的:
- 固件中的数据很容易被破坏(这意味着要花费与编写测试和应用代码相等的时间维护测试数据);
- Rails 在把固件中的数据存入“测试数据库”时会跳过 Active Record层。这意味着很多重要的操作,例如模型的数据验证,会被忽略。这样可不好。

## 预构件

预构件：简单灵活地创建测试数据。`Factory Girl`这个工具不像固件那么容易破坏,方便使用,生成的测试数据也很可靠。
缺点
-  会降低测试速度

## 在应用中使用预构件
在spec文件夹下创建`factories/contacts.rb`并写入下面的代码
```
FactoryGirl.define do
    factory :contact do
        firstname "John"
        lastname "Doe"
        sequence(:email) { |n| "johndoe#{n}@example.com"}
    end
end
```
** sequence是`FactoryGirl`提供的一个功能，每调用一次，n的值都会+1**

*** 只要模型中有唯一性验证，都可以使用sequence(序列) ***

测试代码：

```
require 'rails_helper'

describe Contact do  
  it 'has a valid build' do 
    expect(FactoryGirl.build(:contact)).to be_valid
  end  
  it 'is invalid without a firstname' do
    contact = FactoryGirl.build(:contact, firstname: nil)                       contact.valid?
    expect(contact.errors[:firstname]).to include("can't be blank")  
  end    

  it 'is invalid with a duplicate email address' do    
    FactoryGirl.create(:contact, email: 'fgcui@outlook.com')  
    contact = FactoryGirl.build(:contact,email:'fgcui@outlook.com')
    contact.valid?     
    expect(contact.errors[:email]).to include("has already been taken")
  end
end
```
*** FactoryGirl.build 的 作 用 是 在 内 存 中 存 储 一 个 新 测 试 对 象;FactoryGirl.create 的作用是把对象永久存储到应用的测试数据库中。***

## 生成更真实的虚拟数据-Faker

这里我们介绍一个虚拟数据生成工具[Faker](https://github.com/stympy/faker).
使用Faker之后我们的`factories`的代码就变成了下面的样纸。它会生成看起来更真实的测试数据
```
FactoryGirl.define do
    factory :contact do
       firstname { Faker::Name.first_name } 
       lastname { Faker::Name.last_name } 
       email { Faker::Internet.email }
    end
end
```





