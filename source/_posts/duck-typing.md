---
title: 今天聊一聊duck-typing
date: 2020-08-08 22:00:21
tags: [编程]
categories: [编程, 语言]
description: If it walks like a duck and it quacks like a duck, then it must be a duck
---

duck typing是动态类型的一种风格。If it walks like a duck and it quacks like a duck, then it must be a duck. 我只关心它的行为，不用考虑它是什么类型。

现在常见的语言分为静态语言和动态语言。例如java，强类型语言，类型需要一一对应，如果类型定义大仙不一致的地方，那么连编译一关都过不去。又例如javascript，只要没有语法错误，你就可以运行起来，至于错误只能在运行的时候才能被发现。

静态语言的类型定义，是一个辅助信息，对于最终的程序执行来说，是没有用的，因为不论是什么代码都会被编译成机器码执行。所以在开发阶段，对于静态语言来说，你想把精力集中去解决问题的时候，还需要去考虑类型的定义。

思考一个场景，你有一个打印日志的方法，这个方法有两个参数：输出对象和输出信息。 现在out的行为仅仅是将msg输出
```c
void log_outs(ostream out, msg)
```

现在我想将msg输出到一个文件中，应该要怎么做呢？因为第一个变量已经指定了类型，你无法使用它，只能copy一下再去写一个新的方法。

上面的两个操作有共同的行为，只是需要处理的对象是不一样的。我使用ruby写一个例子
```ruby
class Duck
  def run
    print "鸭子在跑"
  end

  def quack
    print "鸭子在叫"
  end
end

class Person
  def run
    print "模仿鸭子在跑"
  end

  def quack
    print "模仿鸭子在叫"
  end
end

def call_duck(duck)
  duck.run()
  duck.quack()
end

def game()
  duck = Duck.new
  person = Person.new
  call_duck(duck)
  call_duck(person)
end

game()
```

call_duck只负责调用第一个变量上的方法，我不需要知道或者定义参数的类型。

鸭子类型的场景很多，在静态类型中对应的应该是多态了。
```java
class Caller<T extends CallMe> {
    final T callee;
    Caller(T callee) {
        this.callee = callee;
    }
    public void go() {
        callee.call();  // should work now
    }
}

interface CallMe {
    void call();
}

class Foo implements CallMe {
    public void call() { System.out.print("Foo"); }
}

class Bar implements CallMe {
    public void call() { System.out.print("Bar"); }
}

public class Main {
    public static void main(String args[]) {
        Caller<Foo> f = new Caller<>(new Foo());
        Caller<Bar> b = new Caller<>(new Bar());
        f.go();
        b.go();
        System.out.println();
    }
}
```

参考：
* 松本行弘的程序世界
* [https://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B](https://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)
* [https://stackoverflow.com/questions/46270804/using-java-and-cs-generics-to-simulate-duck-typing](https://stackoverflow.com/questions/46270804/using-java-and-cs-generics-to-simulate-duck-typing)

