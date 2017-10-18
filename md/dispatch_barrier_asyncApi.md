```dispatch_barrier_async / dispatch_barrier_sync```
barrier是栅栏的意思使用这个API可以使得多个异步的请求分离


```
    dispatch_queue_t customQueue = dispatch_queue_create("liuxiaoer", DISPATCH_QUEUE_CONCURRENT);
    
    dispatch_async(customQueue, ^{
        NSLog(@"1111111%@",[NSThread currentThread]);
    });
    
    dispatch_async(customQueue, ^{
        NSLog(@"2222222%@",[NSThread currentThread]);
    });
    
    
    dispatch_barrier_sync(customQueue, ^{
        NSLog(@"55555%@",[NSThread currentThread]);
    });
    
    dispatch_async(customQueue, ^{
        NSLog(@"33333%@",[NSThread currentThread]);
    });
    
    dispatch_async(customQueue, ^{
        NSLog(@"44444%@",[NSThread currentThread]);
    });


```
会保证前两个全部执行完毕的时候再执行后边的

 