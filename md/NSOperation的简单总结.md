1. NSOperation 是一个抽象类在实现多线程的时候我们需要用它的子类```NSBlockOperation``` 和 ```NSInvocationOperation```
2. 简单使用:

* NSBlockOperation 	

  ```
  		    
  NSBlockOperation *blockOpe = [NSBlockOperation blockOperationWithBlock:^{
        
        for(int i= 0;i<1000;i++) {
         	NSLog(@"blockOpe---%@",[NSThread 								currentThread]);
        }
    }];
    [blockOpe addExecutionBlock:^{
        NSLog(@"blockOpe+++++%@",[NSThread currentThread]);
    }];
    
    [blockOpe addExecutionBlock:^{
        NSLog(@"blockOpe+++++----%@",[NSThread currentThread]);
    }];


    [blockOpe start];

  	
  ```
* NSInvocationOperation
 	
    ```
        NSInvocationOperation *invo = [[NSInvocationOperation alloc] initWithTarget:self
                                                                       selector:@selector(invoHandle) object:nil];
    
    invo.queuePriority = NSOperationQueuePriorityVeryHigh;
    [invo start];

    
    ```
执行打印的话全部都是在主线程执行的，

  ```
    [blockOpe addExecutionBlock:^{
        NSLog(@"blockOpe+++++%@",[NSThread currentThread]);
    }];

  ```
是在子线程执行一个block代码块是一个单独的子线程
3 如果想要执行多线程需要```NSOperationQueue```来操作

	```
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    [queue addOperation:blockOpe];
    [queue addOperation:invo];
    queue.maxConcurrentOperationCount = 10;
	
	```
	*注意* : 添加之前单独的operation不能调用start方法的


4 使用 ```- (void)addDependency:(NSOperation *)op```来添加operation之间的依赖关系类似GCD中的Group可以进行多个线程请求顺序的保证以及最后主线程的UI更新


5 类属性 ```@property (class, readonly, strong) NSOperationQueue *mainQueue```

6 一定要在将操作添加到操作队列中之前添加操作依赖