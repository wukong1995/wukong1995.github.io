---
title: 哇哇，我的第一个task
date: 2018-12-21 23:00:43
tags: [rails]
categories: [rails]
description: 这是我的第一个写的task
---


最近有一些数据迁移，心中已有蓝图，但是不知如何下手，请教了后端的小哥哥，写出了我的第一个task，
```ruby
namespace :migrate do
  desc 'migrate info to apply'
  task :to_apply => :environment do
    Membership.all.each do |membership|
      user = membership.user
      apply = Apply.new(
        user_id: user.id,
        # ....
      );
      apply.save!(validate: false)
    end
  end
end
```

本来是想写一个数据库的migrate，征求了后端的意见，选择了写一个task，migrate在被合并的时候，这次的数据迁移可能会被吃掉，于是写了一个task，一个简单的数据迁移...