---
layout:     post
title:      "Java concurrent"
subtitle:   " \"Java\""
date:       2016-10-9 17:00:51  
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - java
---
## java.util.concurrent（一）

### 前言

java.util.concurrent包含在并发编程中很常用的实用工具类。概略图如下：

![](http://s4.sinaimg.cn/large/00300w6Bzy6E916bVGb43&690)

### 1. CountDownLatch

JDK解释：
一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。

用给定的计数 初始化 CountDownLatch。由于调用了 countDown() 方法，所以在当前计数到达零之前，await 方法会一直受阻塞。之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。这种现象只出现一次——计数无法被重置。如果需要重置计数，请考虑使用 CyclicBarrier。

CountDownLatch 是一个通用同步工具，它有很多用途。将计数 1 初始化的 CountDownLatch 用作一个简单的开/关锁存器，或入口：在通过调用 countDown() 的线程打开入口前，所有调用 await 的线程都一直在入口处等待。用 N 初始化的 CountDownLatch 可以使一个线程在 N 个线程完成某项操作之前一直等待，或者使其在某项操作完成 N 次之前一直等待。

CountDownLatch 的一个有用特性是，它不要求调用 countDown 方法的线程等到计数到达零时才继续，而在所有线程都能通过之前，它只是阻止任何线程继续通过一个 await。

个人理解：
声明一个CountDownLacth之后，每调用一次countdown()方法，计数减一，直到计数为零之前，所有的线程必须在await()方法前等待。

代码用例：

	package concurrent;
	
	import java.util.concurrent.CountDownLatch;
	
	/**
	 * CountDownLatch 是一个通用同步工具，它有很多用途。将计数 1 初始化的 CountDownLatch 用作一个简单的开/关锁存器，或入口：
	 * 在通过调用 countDown() 的线程打开入口前，所有调用 await 的线程都一直在入口处等待。
	 * 用 N 初始化的 CountDownLatch 可以使一个线程在 N 个线程完成某项操作之前一直等待，或者使其在某项操作完成 N 次之前一直等待。
	 * @author Zhanghj
	 *
	 */
	public class CountDownLatchDemo {
		private static final int N = 10;
		public static void main(String[] args) {
			CountDownLatch doneSignal = new CountDownLatch(N);
			CountDownLatch startSignal = new CountDownLatch(1);
			
			for (int i = 1; i <= N; i++) {  
	            new Thread(new Worker(startSignal,doneSignal,i)).start();//线程启动了  
	        }  
	        System.out.println("begin------------");  
	        startSignal.countDown();//开始执行啦  
	        try {
				doneSignal.await();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}//等待所有的线程执行完毕  
	        System.out.println("Ok");  
			
		}
	}
	
	class Worker implements Runnable{
		
		private final CountDownLatch startSignal;
		private final CountDownLatch doneSignal;
		private int beginIndex;
		
		public Worker(CountDownLatch startSignal, CountDownLatch doneSignal, int beginIndex) {
			super();
			this.startSignal = startSignal;
			this.doneSignal = doneSignal;
			this.beginIndex = beginIndex;
		}
	
		@Override
		public void run() {
			System.out.println("Thread:"+beginIndex+" start;");
			try {
				startSignal.await();
				int count = (beginIndex - 1) * 10 + 1;  
	            for (int i = count; i <= count + 10; i++) {  
	                System.out.println(i);  
	            }  
			} catch (InterruptedException e) {
				e.printStackTrace();
			} finally {
				doneSignal.countDown();
			}
			
			System.out.println("Thread:"+beginIndex+" end;");
		}
		
	}

### 2.CyclicBarrier

JDK解释：
一个同步辅助类，它允许一组线程互相等待，直到到达某个公共屏障点 (common barrier point)。在涉及一组固定大小的线程的程序中，这些线程必须不时地互相等待，此时 CyclicBarrier 很有用。因为该 barrier 在释放等待线程后可以重用，所以称它为循环 的 barrier。

CyclicBarrier 支持一个可选的 Runnable 命令，在一组线程中的最后一个线程到达之后（但在释放所有线程之前），该命令只在每个屏障点运行一次。若在继续所有参与线程之前更新共享状态，此屏障操作 很有用。

个人理解：
一般我们把CyclicBarrier称为栅栏，设定一组线程，CyclicBarrier的await()方法处是它们的休息点，必须等待所有的线程都到达之后，才能做其他的事情。

代码用例
	
	package concurrent;
	
	import java.util.Random;
	import java.util.concurrent.BrokenBarrierException;
	import java.util.concurrent.CyclicBarrier;
	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	
	/**
	 * 一个同步辅助类，它允许一组线程互相等待，直到到达某个公共屏障点 (common barrier point)。
	 * 在涉及一组固定大小的线程的程序中，这些线程必须不时地互相等待，此时 CyclicBarrier 很有用。
	 * 因为该 barrier 在释放等待线程后可以重用，所以称它为循环 的 barrier。
	 * @author Zhanghj
	 *
	 */
	public class CyclicBarrierDemo {
		
		public static void main(String[] args) {
			ExecutorService executorService = Executors.newCachedThreadPool();
			final Random random = new Random();
			
			final CyclicBarrier cyclicBarrier = new CyclicBarrier(4, new Runnable() {
				
				@Override
				public void run() {
					System.out.println("All Treads Complete, now do something else");
				}
			});
			
			for(int i = 0; i < 4 ;i++){
				executorService.execute(new Runnable() {
					
					@Override
					public void run() {
						try {
							Thread.sleep(random.nextInt(1000));
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
						
						System.out.println(Thread.currentThread().getName()+" 任务完成，等待其他线程");
						try {
							cyclicBarrier.await();
						} catch (InterruptedException e) {
							e.printStackTrace();
						} catch (BrokenBarrierException e) {
							e.printStackTrace();
						}
					}
				});
			}
			
			executorService.shutdown();
			
		}
	}


### 3.Semaphore

JDK解释：
一个计数信号量。从概念上讲，信号量维护了一个许可集。如有必要，在许可可用前会阻塞每一个 acquire()，然后再获取该许可。每个 release() 添加一个许可，从而可能释放一个正在阻塞的获取者。但是，不使用实际的许可对象，Semaphore 只对可用许可的号码进行计数，并采取相应的行动。

Semaphore 通常用于限制可以访问某些资源（物理或逻辑的）的线程数目。

个人理解：
用于一个限定数量的许可集，Semaphore能acquire()的数量受限制，在达到上限之后，只有调用release()之后才能再acquire新的元素。

	package concurrent;
	
	import java.util.Collections;
	import java.util.HashSet;
	import java.util.Set;
	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	import java.util.concurrent.Semaphore;
	/**
	 * Semaphore 一个计数信号量。从概念上讲，信号量维护了一个许可集。
	 * 如有必要，在许可可用前会阻塞每一个 acquire()，然后再获取该许可。
	 * 每个 release() 添加一个许可，从而可能释放一个正在阻塞的获取者。
	 * 但是，不使用实际的许可对象，Semaphore 只对可用许可的号码进行计数，并采取相应的行动。
	 * Semaphore 通常用于限制可以访问某些资源（物理或逻辑的）的线程数目。
	 * @author Zhanghj
	 *
	 */
	public class SemaphoreDemo {
	
		public static void main(String[] args) {
			SemaphoreDemo demo = new SemaphoreDemo();
			//信号量控制为两个
			final BoundedHashSet<String> set = demo.getSet(2);
			
			ExecutorService executorService = Executors.newCachedThreadPool();
			
			//第三个线程被限制,在release一个元素之后才能加入第三个元素
			for(int i = 0; i < 3;i++){
				executorService.execute(new Runnable() {
					@Override
					public void run() {
						try {
							set.add(Thread.currentThread().getName());
							Thread.sleep(1000);
							set.remove(Thread.currentThread().getName());
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				});
			}
			executorService.shutdown();
			
		}
		
		public BoundedHashSet<String> getSet(int bound) {
			return new BoundedHashSet<String>(bound);
		}
	}
	
	class BoundedHashSet<T>{
		private final Set<T> set;
		private final Semaphore semaphore;
		public BoundedHashSet(int bound) {
			this.set = Collections.synchronizedSet(new HashSet<T>());
			this.semaphore = new Semaphore(bound, true);
		}
		
		public void add(T o) throws InterruptedException {
			semaphore.acquire();
			set.add(o);
			System.out.printf("add: %s%n",o);
		}
		
		public void remove(T o) {
			if(set.remove(o)){
				semaphore.release();
				System.out.printf("remove: %s%n",o);
			}else {
				System.out.printf("not found: %s%n",o);
			}
		}
	}
	
### 4.Exchanger

JDK解释：
可以在对中对元素进行配对和交换的线程的同步点。每个线程将条目上的某个方法呈现给 exchange 方法，与伙伴线程进行匹配，并且在返回时接收其伙伴的对象。Exchanger 可能被视为 SynchronousQueue 的双向形式。Exchanger 可能在应用程序（比如遗传算法和管道设计）中很有用。

个人理解：
用一个伙伴线程，交换线程中的元素

代码用例
	
	package concurrent;
	
	import java.util.ArrayList;
	import java.util.concurrent.Exchanger;
	
	/**
	 * 可以在对中对元素进行配对和交换的线程的同步点。
	 * 每个线程将条目上的某个方法呈现给 exchange 方法，与伙伴线程进行匹配，并且在返回时接收其伙伴的对象。
	 * Exchanger 可能被视为 SynchronousQueue 的双向形式。Exchanger 可能在应用程序（比如遗传算法和管道设计）中很有用。
	 * 
	 * 示例：使用 Exchanger 在线程间交换缓冲区，因此，在需要时，填充缓冲区的线程获取一个新腾空的缓冲区，并将填满的缓冲区传递给腾空缓冲区的线程。
	 * @author Zhanghj
	 *
	 */
	public class ExchangerDemo {
		
		public static void main(String[] args) {
			final Exchanger<ArrayList<Integer>> exchanger = new Exchanger<ArrayList<Integer>>();
			final ArrayList<Integer> buff1 = new ArrayList<Integer>(10);
			final ArrayList<Integer> buff2 = new ArrayList<Integer>(10);
			
			new Thread(new Runnable() {
				@Override
				public void run() {
					ArrayList<Integer> buff = buff1;
					while(true){
						
	
						if (buff.size() >= 10) {
							try {
								System.out.println("before exchange buff1.size():"+buff.size());
								buff = exchanger.exchange(buff);
								System.out.println("Exchange buff1");
								System.out.println("after exchange buff1.size():"+buff.size());
	//							for (Integer integer : buff) {
	//								System.out.println(Thread.currentThread().getName()+": "+integer);
	//							}
								buff.clear();
							} catch (InterruptedException e) {
								e.printStackTrace();
							}
						}
						buff.add((int)(Math.random()*1000));
						try {
							Thread.sleep((long)(Math.random()*1000));
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				}
			}).start();
			
			new Thread(new Runnable() {
				
				@Override
				public void run() {
					ArrayList<Integer> buff = buff2;
					while (true) {
						System.out.println("buff2.size():"+buff.size());
	//					for (Integer integer : buff) {
	//						System.out.println(Thread.currentThread().getName()+": "+integer);
	//					}
						
						try {
							Thread.sleep(1000);
							buff = exchanger.exchange(buff);
							System.out.println("Exchange buff2");
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				}
			}).start();
		}
	}


### 后记

练习源码地址：[https://github.com/Zering/Java-SE-Core/tree/master/src/concurrent](https://github.com/Zering/Java-SE-Core/tree/master/src/concurrent)

下节练习：`BlockingQueue`、`SynchronousQueue`、`ScheduledThreadPoolExecutor`、`FutureTask`