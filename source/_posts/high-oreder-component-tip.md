---
title: HOC在react中的调用顺序
date: 2018-09-20 19:17:42
tags: ['react']
categories: ['react']
---

#### question
今天遇到了一个问题：写了一个高阶组件，使用的时候竟然告诉我props is required, but it is undefined???
```js
// FormWrapper.jsx
const FormWrapper = initialState => Component => {
  class FormInnerWrapper extends React.Component {
    state = initialState

    checkFeildValid = (value, type) => {
      // ...
    }

    changeValue = (value, type) => {
      // ...
    }

    isFormValidate = () => {
      // ...
    }

    render() {
      return <Component data={this.state} changeValue={this.changeValue} isFormValidate={this.isFormValidate} {...this.props} />;
    }
  }

  return FormInnerWrapper;
};

FormWrapper.propTypes = {
  initialState: PropTypes.object.isRequired,
};

export default FormWrapper;

// MyForm.jsx
@FormWrapper({
  xxx: 'xxx',
  xxxx: 'xxx'
})
class MyForm extends React.Component {
  // ...
}

MyForm.propTypes = {
  isFormValidate: PropTypes.func.isRequired,
  data: PropTypes.object.isRequired,
  changeValue: PropTypes.func.isRequired,
};
export default MyForm;
```

react给我三个warning：
```js
The prop `data` is marked as required in FormInnerWrapper, but it is undefined.
The prop `isFormValidate` is marked as required in FormInnerWrapper, but it is undefined.
The prop `changeValue` is marked as required in FormInnerWrapper, but it is undefined.
```

#### debug
我检查了代码，发现我没有对FormInnerWrapper定义propTypes， 我只对MyForm进行了propTypes定义。我尝试给MyForm加上defaultProps,其中data的default值为null，在调试的时候发现data的值永远为null, 不会去改变🤔️。发生了什么？

#### plan B
于是我尝试了下面的格式：
```js
// MyForm.jsx
export default FormWrapper({
  xxx: 'xxx',
  xxxx: 'xxx'
})(MyForm);

```
发现没有warning。


#### why
装饰器的写法是从左到右执行？？？
第二种写法比较符合正常的调用顺序？？
我需要再去查decorator与HOC的具体区别...
PS: 上次的immutable data疑问，在今天终于得到了解决，下一篇见～



