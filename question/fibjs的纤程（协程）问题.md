##问

最近在给v8加协程支持，碰到一个问题：  

在实现协程上下文切换时，恢复协程时，碰到过这种v8的断言失败：

```
#
# Fatal error in G:\Data\src\v8\src/objects-inl.h, line 7060
# CHECK_EQ(isolate_->relocatable_top(), this) failed
#   Expected: 0359F940
#   Found: 0376EB60
#
```     

我看v8代码是因为函数调用结束和开始内部的一个FunctionCallbackArguments custom（xxx）继承自v8的Relocatable，它构造和析构必须保证在同一个栈上；

但协程上下文切换恢复的时候，难以保证此断言（例如，sleep内部挂起协程，之后，执行另外一个协程的sleep，这个会挂起当前协程并跳回之前的那个协程，然后最外面的协程sleep结束的时候——就是协程恢复的时候——以上断言失败）。

我想问fibjs是怎么处理这个问题的，我看了fibjs的代码似乎没有什么特殊处理？   

##答 
v8 支持多线程运行，但是同一时间只允许一个线程进入 v8 虚拟机。纤程便是利用了这一特性。v8 为了保证这一点，需要在多线程离开 vm 和恢复 vm 的时候通知  v8 保存和恢复现场。相应的 api 你可以查阅文档。我就不告诉你了，直接告诉你印象不深。


##续问
感谢回复；不过我用的是单线程，这个也需要显示的同步吗？v8::Locker？   

##答
纤程切换堆栈在 v8 看起来就是换了线程啊。