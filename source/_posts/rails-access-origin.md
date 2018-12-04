---
date: 2018-07-06 13:26:33
title: set access-origin in rails
tags: [rails]
description: 在rails里面如何设置access-origin
categories: [rails]
---


记录一下，在rails里面如何设置access-origin....

```ruby
gem 'rack-cors', :require => 'rack/cors'


config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'
    resource '*', :headers => :any, :methods => [:get, :post, :options]
  end
end
```
