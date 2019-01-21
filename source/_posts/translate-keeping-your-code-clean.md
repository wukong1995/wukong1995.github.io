---
title: 翻译：保持你的代码洁净
date: 2019-01-18 18:38:57
tags: [翻译, javascript]
categories: [翻译]
description: keeping your code clean
---

[原文链接](https://codeburst.io/keeping-your-code-clean-d30bcffd1a10)

我在座位上安顿下来，与我的团队成员一起解决问题。我说"我们必须赢的这场比赛"。在两天内埋头开发一个工作原型，大家的好胜心都被激发，都在争夺前三名。

几分钟后，其中的一个高级工程师走到我的办公桌前，脸上露出一丝不满，喃喃自语：你的代码不清晰，很乱！这是我迈向clean code旅程的开始。


clean code？嗯，这对于我来说并不奇怪，但是如果代码正常工作，这真的很重要吗。是的，它确实很重要，一千次。

在这次活动之前，我曾担任几年的软件工程师。我已经构建了应用，但是我刚刚被告知了让我代码不一样的东西。

我的问题很简单：我专注于完成工作，目的是编写有效的代码，反过来又招致了技术债务。

#### clean code 的方式
当你读完[clean code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)的所有章节时，就不会发生这种情况。它需要知识和不断的实践，你必须学习原理，模式和实践。这是艰苦的工作需要数年，但你可以从今天开始。

无论你怎么去clean你的code，总有一件或两件你能学到的事情使code变得更干净。

学习的最佳方式之一是阅读专家的书籍或者帖子，你应该在你的twitter中有他们的推特流，听取他们的交谈，在github上follow他们，学习他们的代码是如何写和组织的。

>Your growth is limited as an engineer if you do not constantly learn from experts in your field.


#### 保持你的函数短小精悍
这可能是1337篇文章中的一篇，来强调保持函数尽可能短，人们很容易在这里弄错。

> clean code不仅仅是写短的方法，而是编写清晰表达意图的代码。

当一个函数太长了，这说明它做的太多了，阅读者可能会无法完全解读它的功能，一个函数应该做一件事。

```php
if($order->contains($status){
   //do something with order
}

function contains($status){
  $order_statuses=['accepted','delivered','rejected','processed'];
    if (in_array($status, $order_statuses)) {
          return true;
       }
      return false;
 }
```

我们可以重写它来使contains更清楚：
```php
function contains($status){
  return in_array($status, $this->config->statuses);
}
```

现在，contains不仅仅更简短，而且还能解耦。

#### 变量和函数名字肯以体现其功能
为函数命名是乏味的，但是这绝对物有所值。当代码改变时，你可以不用更新注释。
```php
$date =date('Y-m-d'); //Ofcourse, it's a date but too generic!
$orderCreationDate =date('Y-m-d'); //cleaner code
```

#### 避免使用if和switch
就个人而言，我花了一段时间来掌握这一点。你怎么能告诉我避免我的最爱呢？事实上，大多数的条件语句可以很容易的提取到单独的函数和类中。这并不是说你永远不应该使用if和switch语句，但在某些情况下可以避免。

这有一个很好的例子：
```php
class BookOrder
{

    public function process()
    {
        switch ($this->book->type) {
            case 'audiocopy':
                return $this->processAudioBook();
                break;
            case 'downloadablecopy':
                return $this->processDownloadableCopy();
                break;
            case 'paperbookcopy':
                return $this->processPaperBookCopy();
                break;
            default:
        }
    }

}
```

更整洁、更易于维护的方法是：
```php
interface  IBookOrder {

    public function process();
}
class AudioBookOrder implements IBookOrder :void {

    public function process()
    {
        // TODO: Implement process() method.
    }
}
class PaperBookOrder implements IBookOrder: void {

    public function process()
    {
        // TODO: Implement process() method.
    }
}
class DownloadableBookOrder implements IBookOrder: void {

    public function process()
    {
        // TODO: Implement process() method.
    }
}
```

#### 避免心理映射
clean code应该是更易于阅读，理解并且不应该留下任何猜测空间。

>It is not the language that makes a program look simple, but the programmer who makes the language appear simple.
 Robert C. Martin

以下代码检查客户能否可以提取一定数额的资金，它有效但是很乱。
```php
if($this->user->balance  > $amount && $this->user->activeLoan===0){
   $this->user->balance -=$amount; // withdraw amount;
}
```

让我们把它变得更清晰：
```php
if($this->hasNoActiveLoan() && $this->canWithdrawAmount($amount)){
   $this->withdraw($amount);
}

public function hasNoActiveLoan(){
  return $this->user->activeLoan===0;
}
public function canWithdrawAmount(float $amount){
   return $this->user->balance > $amount;
}
public function withdraw(float $amount){
  $this->user->balance -=$amount;

}
```

这不仅仅更容易理解而且更容易测试了。

#### 理解和应用S.O.L.I.D原则
S.O.L.I.D是由Robert C Martin定义的面向对象编程的前五个原则的首字母缩写。使用这些原则，你可以编写低耦合、高内聚和封装很好的代码。这些原则密切相关。

#### 不要太为难自己
想知道这一点为什么这个在名单上？陷入清洁代码的世界是很容易，想要一天吸收一切。悲伤的是：需要时间，数月，数年和奉献精神。必须学习和实践这些原则，但这一切都始于决定让事情变得更加清洁。

