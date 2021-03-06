# 并发编程

Go 语言提倡不要通过共享内存来通信，而应通过通信来共享内存；将共享的值通过信道传递，实际上，多个独立执行的线程从不会主动共享。在任意给定的时间点，只有一个 协程能够访问该值。数据竞争从设计上就被杜绝了。

Go 的 slice 的 append 和 map 的写操作都是并发不安全的，如果出现多 Go 协程，写入场景时，需要用 sync 包中的 Mutex 或 RWMutex。map 场景的话可以直接用 sync.Map。

sync.Map 在以下两种场景下优于 map+RWMutex

1. 对于给定的 key，只写一次，多次读取
2. 对于多个 Go 协程，每个协程读写的 key 为互斥的子集
