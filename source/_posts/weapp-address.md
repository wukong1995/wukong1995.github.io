---
title: 小程序二级地址选择器
date: 2018-12-11 20:50:15
tags: [小程序]
categories: [小程序]
description: 打造属于自己的地址选择器
---

今天的任务里面有一个表单，表单里面有地址选择器，小程序官方的组件提供的是国内三级地址选择器，但是有些地方还需要海外的地址，地址又不需要这么详细，到二级足矣...

我的想法是共有两了picker,第一个选择国家，如果选择的是中国的话，第二个输入框为省市picker，如果是海外，第二个为输入框。

那么第二个输入框的数据从哪来呢？是否是需要请求后端？想了想延迟，还是从前端来吧，数据的来源就是某宝了，数据也是最新的。数据格式我存储成下面这样，向后端传值的时候，就传代码就可以了。

```js
export const province = [
  { id: "110000", name: "北京" },
  // ...
}

export const city = {
  "110000": [{
    "id": "110100",
    "name": "北京"
  }],
  "110100": [{
    "id": "110101",
    "name": "东城"
  }, {
    // ....
  },
    // ...
  ],
  // .....
}
```

其中有一个很坑的地方是，在picker的bingchange函数中，如果直接使用event中的detail值直接赋值，会会发现你的数据从number变成string类型，我在第二个显示输入框还是picker的时候，死活显示不出来，都已经给设计要planb了😂，我说是小程序的锅吗后来搜了一圈，也没发现类似问题...最后使用Number强制类型转换了一下。

下面是部分小程序的代码了：
```html
<picker name="city" bindchange="bindCountryChange" value="{{countryIndex}}" range="{{country}}">
<text>{{country[countryIndex]}} </text>
</picker>

<view class="{{countryIndex === 0 ? '' : 'is-hidden'}}">
    <picker
      name="external"
      mode="multiSelector"
      range-key="name"
      bindchange="bindCityChange"
      bindcolumnchange="bindCityColumnChange"
      value="{{cityIndex}}"
      range="{{city}}"
    >
      <view class="u-text-limit--one" name="internal">
        {{city[0][cityIndex[0]].name}}{{city[1][cityIndex[1]].name}}
      </view>
    </picker>
</view>
<view class="{{countryIndex === 0 ? 'is-hidden' : ''}}">
    <input type="text" name="internal" placeholder="请输入城市" />
</view>
```

```js
bindCityChange: function(event) {
    const index = Number(event.detail.value);
    this.setData({
      cityIndex: index
    })
},

bindCityColumnChange(e) {
    const data = {
      city: this.data.city,
      cityIndex: this.data.cityIndex
    }
    data.cityIndex[e.detail.column] = e.detail.value
    if (e.detail.column === 0) {
      const provinceIndex = data.cityIndex[0];
      const provinceId = data.city[0][provinceIndex].id;
      data.city[1] = city[provinceId];
      data.cityIndex[1] = 0;
    }
    this.setData(data)
},
```
