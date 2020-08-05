# Java 线程： countDownLatch

### 1. 背景

- `countDownLatch`是在java1.5被引入，跟它一起被引入的工具类还有`CyclicBarrier`、`Semaphore`、`concurrentHashMap`和`BlockingQueue`。
- 存在于`java.util.cucurrent`包下。

### 2. 概念

- `CountDownLatch`是一个同步工具类，用来协调多个线程之间的同步，或者说起到线程之间的通信（而不是用作互斥的作用）。

- `CountDownLatch`能够使一个线程在等待另外一些线程完成各自工作之后，再继续执行。使用一个计数器进行实现。计数器初始值为线程的数量。当每一个线程完成自己任务后，计数器的值就会减一。当计数器的值为0时，表示所有的线程都已经完成一些任务，然后在CountDownLatch上等待的线程就可以恢复执行接下来的任务。

### 3. 源码

- countDownLatch类中只提供了一个构造器：

  ```java
  // 参数count为计数值
  public CountDownLatch(int count) {  };  
  ```

- 类中有三个最重要的方法是：

  ```java
  	// 调用await()方法的线程会被挂起，它会等待直到count值为0才继续执行
  	public void await() throws InterruptedException { };   
  
  	/**
       * 等待一定的时间后count值还没变为0的话就会继续执行
       * @param timeout 要等待的最长时间
       * @param unit    参数的时间单位
       * @return   如果计数到达零，则返回 true；如果在计数到达零之前超过了等待时间，则返回 false
       * @throws InterruptedException
       */
      public boolean await(long timeout, TimeUnit unit) throws InterruptedException { };
  
  	// 将count值减1
  	public void countDown() { }; 
  ```

  > 使当前线程在锁存器倒计数至零之前一直等待，除非线程被中断或超出了指定的等待时间。如果当前计数为零，则此方法立刻返回 `true` 值。 如果当前计数大于零，则出于线程调度目的，将禁用当前线程，且在发生以下三种情况之一前，该线程将一直**处于休眠状态**：
  >
  > - ​	由于调用 `countDown()` 方法，计数到达零
  > - ​    其他某个线程中断当前线程；
  > -    已超出指定的等待时间。
  >
  > 如果计数到达零，则该方法返回 `true` 值。
  >
  > 如果当前线程：
  >
  > - ​	在进入此方法时已经设置了该线程的中断状态；
  > - ​    在等待时被中断
  >
  > 则抛出 `InterruptedException`，并且清除当前线程的已中断状态。
  >
  > 如果超出了指定的等待时间，则返回值为 `false`

### 4. 用法

1. 某一线程在开始运行前等待n个线程执行完毕

2. 实现多个线程开始执行任务的最大并行性

   > 注意是并行性，不是并发，强调的是多个线程在某一时刻同时开始执行。

### 5. 不足

​		CountDownLatch是一次性的，计算器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当CountDownLatch使用完毕后，它不能再次被使用。

### 6. 示例

假设某公司需召开一个视频会议，共有10名高管参会，只有所有高管均到场后会试才开始

```java
/**
 * @author hz
 * @blog www.huangzun.top
 * @date 2020/4/12
 */
public class CountDownLatchTest {
    public static void main(String[] args) {
        final CountDownLatch latch = new CountDownLatch(10);

        for (int i = 0; i < 10; i++) {
            ExecutorService es = Executors.newCachedThreadPool();
            String num = i + "";
            es.execute(() -> {
                try {
                    // 模拟公司高管到会不同的时间
                    Thread.sleep((int) (Math.random() * 10 * 1000));
                    Thread.currentThread().setName("boos-" + num);
                    System.out.println("高管(子线程)："+Thread.currentThread().getName()+" 到会！");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                latch.countDown();
            });
            es.shutdown();
        }
        System.out.println("等待所有人到齐...");

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("所有人到齐，会议开始");
    }
}	
```

**运行结果**

![image-20200412183240610](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200412183240610.png)

### **6. CountDownLatch和CyclicBarrier区别**

- `countDownLatch`是一个计数器，线程完成一个记录一个，计数器递减，只能只用一次
- `CyclicBarrier`的计数器更像一个阀门，需要所有线程都到达，然后继续执行，计数器递增，提供reset功能，可以多次使用