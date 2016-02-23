title: 为什么 iOS 开发中，控件一般为 weak 而不是 strong？
date: 2015-08-10 23:42:38
categories: iOS
tags:
---
参考自：[http://www.zhihu.com/question/29927614?sort=created](http://www.zhihu.com/question/29927614?sort=created)


因为控件他爹( view.superview )已经揪着它的小辫了( strong reference )，你( viewController )眼瞅着( weak reference )就好了。

当然，如果你想在 view 从 superview 里面 remove 掉之后还继续持有的话，还是要用 strong 的( 你也揪着它的小辫， 这样如果他爹松手了它也跑不了 )。