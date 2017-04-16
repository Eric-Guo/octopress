---
layout: post
title: "Explore Rails 5.1 new feature via an simple project"
date: 2017-04-16T21:01:57+08:00
comments: true
external-url: 
categories: [Rails, Yarn, IntegrationTest]
---

Rails 5.1即将发布，虽然同Rails 5相比，这只是一个0.1的小版本，但我看来，Rails 5.1的新的功能却非常重要：在JavaScript世界建构和模块管理工具突飞猛进了3～4年后（grunt, gulp, browserify, npm, webpack, yarn），一向喜欢稳定的Rubyist们（喜欢折腾的都跑去javascript, go和Elixir了。。），在大神[DHH](https://github.com/dhh)和女神[Liceth](https://github.com/Liceth)以及其他众神的帮助下，迎来了和node.js国的最终和解：[Yarn](https://github.com/rails/rails/pull/26836)和[Webpack](https://github.com/rails/rails/pull/27288)已经正式成为了[主厨推荐](https://ruby-china.org/wiki/the-rails-doctrine#主厨精选)。

本文和其他介绍[Rails 5.1 Whatsnew](http://blog.michelada.io/whats-new-in-rails-51)稍有不同的地方在于，我试图通过一个新的项目探索Rails 5.1的特点。

[product hunt](https://github.com/Eric-Guo/product_hunt)是一个功能简单的Rails应用，仅仅实现了产品的维护，但在功能方面，探索了yarn下对新javascript框架的使用，还有集成测试。

具体来说，使用了比较小众的[milligram CSS框架](https://milligram.github.io)，输入自动提示使用了零依赖的[awesomplete](http://leaverou.github.io/awesomplete/)，使用基于Chrome的集成测试。

## Yarn

Yarn同npm相比，优点很多，yml格式的yarn.lock文件显示依赖关系清晰易读，安装速度快，完全不需要再使用asset pipe line对已有的javascript模块进行二次封装，省事省力，省开新gem。环境准备也相当容易，在国内（必须）用[淘宝源](https://npm.taobao.org)即可。

```bash
brew install yarn
yarn config set registry 'https://registry.npm.taobao.org'
```

设置完毕后，就可以直接使用yarn添加依赖了。

```bash
yarn add milligram
```

使用CSS前端框架，直接引用即可：

```css
/* app/assets/stylesheets/application.css */
*= require milligram
```

```sass
// app/assets/stylesheets/milligram.sass
@import milligram/src/Color
@import milligram/src/Base
@import milligram/src/Button
```

使用javascript库多做一步：

```javascript
// app/assets/config/manifest.js
//= link awesomplete/awesomplete.js
```

```erb
<%# app/views/products/_form.html.erb %>
<%= javascript_include_tag "awesomplete", async: true -%>
```

## Webpack

Webpack是可选的，如果是[majestic monolith](https://m.signalvnoise.com/the-majestic-monolith-29166d022228)的Rails应用的话，肯定不会用，但是现代的前后端分离风潮下，至少Rails 5.1让这个前后端分离变得无比容易，比之前的[react_on_rails](https://github.com/shakacode/react_on_rails)方案容易好多。

## jQuery的依赖去除

由于Javascript一众MVC框架的崛起，Rails 5.1在前端方案上给予了开发者更多的选择权，我试了在没有jquery的情况下开发使用[vanilla-js](http://vanilla-js.com)开发，虽然时不时还需要查一下[对应jQuery的用法](http://youmightnotneedjquery.com)，但这样做还是值得的，微信小程序的卖点就是即用即走，但如果能将网页做到轻量，何尝不是即用即走？况且网页发布还不用走审核流程。

在Rails下使用vanilla js其实也不是什么都没得用，[rails-ujs](https://github.com/rails/rails/tree/master/actionview/app/assets/javascripts)始终是存在的，所以还是可以直接使用Rails.ajax方法来发起远程调用：

```javascript
window.addEventListener('input', function (e) {
  if (e.target.id == "product_name") {
    Rails.ajax({
      type:'GET',
      url: e.target.getAttribute('data-url'),
      dataType: 'script'
    });
  }
}, false);
```

## System Test

Rails 5.1对集成测试也有了官方方案，现在出厂即可跑集成测试。集成测试在测试自动提示这样的特性的时候还是很方便的，官方出厂的默认配置基于selenium-webdriver驱动的Chrome，使用之前需要安装驱动：

```bash
brew install chromedriver
```

```ruby
require 'application_system_test_case'

class NewProductTest < ApplicationSystemTestCase
  test 'create a new product' do
    visit '/products/new'

    fill_in 'product_name', with: 'L'
    page.has_selector?('ul > li > mark')
    fill_in 'product_name', with: 'Le Wagon'
    page.has_no_selector?('ul > li > mark')
    fill_in 'product_tagline', with: 'Change your life: Learn to code'
    click_button 'Create Product'

    # Should be redirected to Home with new product
    assert_equal product_path(Product.last), page.current_path
    assert page.has_content?('Change your life: Learn to code')
  end
end
```

这里第一次在product_name填入'L'，应该有自动完成的提示出现，完全输入后，则没有，这些都可以通过Capybara的浏览器DSL做检测，从而在集成测试中一并测试好，相比之前的单个controller测试，效率提高不少。

## 其他特性。

其他的特性：Encrypted secrets、Parameterized mailers、Direct & resolved routes等，参见[Rails 5.1 Release notes](https://github.com/rails/rails/blob/master/guides/source/5_1_release_notes.md#encrypted-secrets)，演示项目中没有包括这些功能，这里就略过了。

## 本文不足

测试方案还想使用[phantomjs和poltergeist](https://github.com/Eric-Guo/product_hunt/commit/8f5df2358e7ee9c9a65d0412e0299e4f14659564)，但是始终没有成功，有兴趣的同学可以尝试并提Pull Request。

