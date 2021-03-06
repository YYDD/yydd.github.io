---
title: KVO & Delegate & Notification
date: 2016-08-18 14:24:19
tags:
---
这段时间在梳理一些内容，发现kvo、delegate、notification这三个可以归为一类。我将他们归为一类的原因主要是：他们都有一个特性经常会把自己的事情交给别人做！虽然说都是大家都是做着同一件事情，但是之前的差别或者说各自的优缺点还是蛮鲜明的。

在这里我不太想讲，他们各自的实现是怎么用的，主要讲下在如何场景下用比较合适以及我对于各个的看法。

那么先从简单的说起吧：

### Notfification
对于我来说，这三种里面，广播（notification）是最早接触到的一个。我记得刚开始接触的时候，用得乐此不疲。因为相对来说，它可以算作是彻彻底底的解耦合的，只需要发广播、监听广播就可以了。但是就是因为彻彻底底的解耦合，导致出现的问题是：对于排查问题的时候很艰难，因为你完全没办法预想到这个广播到底是在哪边接收到的，或者说到底是有多少个地方接收到的。我还记得一开始的时候我底层的网络库的封装全部使用广播的形式。反正后来在做修改的时候是吃尽了苦头，因为每次要找下一步执行到哪里，只能是拿着广播的那个字符串全局搜索...<!--more-->

所以自从那之后，我就尽可能的避免使用广播。不过在有些场景下还是需要使用广播的。比如说你需要在多处监听一个事情的发生，但是又不想或者说完全没必要只是为了监听这个事件的发生而有耦合。这个时候用广播是再合适不过了。举个例子吧：就比如说需要在某些页面监听在application里面回调的app进入后台的这个事件。这时候是比较适合用广播的。
对于notification的忠告，是尽可能的少用，为了代码结构的清晰，少用吧~


### Delegate
delegate，代理。顾名思义就是把自己要做的事情代理给别人来做。相信代理接触的最多的应该就是tableview的吧。对于一些view里面的控件按钮的事件把它代理出来什么的都是用delegate。对于delegate的过多的不多说了。只是在修饰delegate的属性的时候记得用“weak”，不然循环引用哦~~~


### KVO
最后来说下KVO，其实对于KVO，我一直觉得它的水很深。谈到KVO，肯定不能少了他的“兄弟”KVC。
KVC全称是Key Value Coding。通过名称就能够看出它可以让我们通过key的形式来访问property，通过这种形式来访问一个对象的属性时候会节省很多代码。其中coredata的NSManagedObject类的属性的访问也是通过kvc的形式来的。对于kvc的，我就不过多的做解释了。因为感觉如果正要讲kvc可以完完整整的写一大篇文章。

言归正传来讲KVO，其实我自己对于KVO是有一定的排斥心理的，因为KVO一不小心就会crash，比如说没有在适当的时候removeObserve，没什么可说的crash，又比如说在addObserve，添加了一个没有的属性，直接crash...所以用了KVO之后，对于被观察的那个属性还要确保它不会被修改。相对来说限制还是比较多的。

如果按我这样说来KVO那不是一无是处啦，这个当然不是啦。KVO有一点，我觉得是前面的那两种都无法做到的，就是你可以对于一个无源码的类的某个属性进行观察。有这个功能，可以让很多复杂的事情变得实现起来很简答。比如说我想监听一个字符串的变化，对于它的每次改变我可能都要有不同的操作。因为`NSString`这个类并不是开源的，所以我没办法对其实现类进行修改，但是能通过一定的途径知道它的属性，所以这个时候就可以用KVO，当然也只能用KVO来观察了吧。。。


### 总结
虽然对于Notification、Delegate、KVO三者都做了一定的介绍，并且发表了一点个人在实际使用过程中的看法。但是“存在既有意义！”所以每一种的存在肯定有他一定的特殊价值在那里，不然的话老早被苹果废弃掉了。在使用过程中选择自己拿手的，适合场景的来应用。千万不要为了使用而使用。





