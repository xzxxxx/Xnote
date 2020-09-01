# IO模型

## 同步和异步

同步：调用方必须等调用方法完成返回后，才能继续其他的操作，用户空间的线程是发起io的一方

异步：调用方调用方法后，方法会立刻返回，调用方可以进行其他操作，在方法完成后会通知调用方，内核空间是发起io的一方



## 阻塞和非阻塞

阻塞：调用方一直在等待，而不能做其他事情

非阻塞：调用方拿到内核的状态值就可以做其他的事情

## BIO(Blocking IO)

![image-20200827154502016](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200827154502016.png)



同步阻塞IO，调用期间用户线程阻塞，等到内核空间io完成后，用户线程才会解除阻塞



## NIO(Non-Blocking IO)

![image-20200827154856060](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200827154856060.png)

当内核缓冲区中没有数据时，可以立即返回一个信息，内核缓冲区已经有数据时，io操作时阻塞的

需要不断轮询内核中数据是否准备就绪，占用大量cpu资源



## IO多路复用

![image-20200827161026894](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200827161026894.png)

与Non-Blocking IO类似，轮询的操作时由系统内核完成的。

通过select/epoll，可以轮询多个socket的连接。

一个selector监听多个socket连接，在selector中的注册的事件到来时，线程阻塞处理io事件。





## AIO(Asynchronous IO)

![image-20200827161117866](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200827161117866.png)

用户线程发起io后就可以做其他事，内核空间中数据准备好后会通知用户线程io操作完成



# Java中的IO

## OIO（old IO)

- ==面向字节流或字符流：只能顺序读取==
- io时是阻塞操作	
- io操作时单向的，只能读（输入流），或者写（输出流）

## NIO（New IO、IO多路复用）

JAVA中NIO由selector、channel、buffer三大组件构成

==非阻塞，面向缓冲区==

使用的是堆外内存（直接内存）

#### buffer

java中的nio是面向缓冲区的，可以从缓冲区中的任意位置读取数据，而不像oio那样需要顺序读取



Java NIO Buffer类的基本步骤如下：

（1）使用创建子类实例对象的allocate()方法，创建一个Buffer类的实例对象。

（2）调用put方法，将数据写入到缓冲区中。

（3）写入完成后，在开始读取数据前，==调用Buffer.flip()方法，将缓冲区转换为读模式。==

（4）调用get方法，从缓冲区中读取数据。

（5）读取完成后，调用==Buffer.clear() 或Buffer.compact()方法，将缓冲区转换为写入模式。==



#### channel

通过一个channel可以完成双向的io操作，即可以读也可以写

#### selector

在selector中注册事件，selector就会监听该事件。通过选择器，一个线程可以查询多个通道的IO事件的就绪状态。