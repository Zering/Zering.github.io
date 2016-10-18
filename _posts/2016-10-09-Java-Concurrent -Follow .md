---
layout:     post
title:      "Java concurrent（二）"
subtitle:   " \"Java\""
date:       2016-10-11 17:12:04   
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
---
## java.util.concurrent（二）

### 前言

java.util.concurrent包含在并发编程中很常用的实用工具类。概略图如下：

![](http://s4.sinaimg.cn/large/00300w6Bzy6E916bVGb43&690)

### 5.BlockingQueue

JDK解释：
支持两个附加操作的 Queue，这两个操作是：获取元素时等待队列变为非空，以及存储元素时等待空间变得可用。

BlockingQueue 方法以四种形式出现，对于不能立即满足但可能在将来某一时刻可以满足的操作，这四种形式的处理方式不同：第一种是抛出一个异常，第二种是返回一个特殊值（null 或 false，具体取决于操作），第三种是在操作可以成功前，无限期地阻塞当前线程，第四种是在放弃前只在给定的最大时间限制内阻塞。下表中总结了这些方法：

		抛出异常		特殊值		阻塞			超时
	插入	add(e)		offer(e)	put(e)		offer(e, time, unit)
	移除	remove()	poll()		take()		poll(time, unit)
	检查	element()	peek()		不可用	      不可用
BlockingQueue 不接受 null 元素。试图 add、put 或 offer 一个 null 元素时，某些实现会抛出 NullPointerException。null 被用作指示 poll 操作失败的警戒值。

BlockingQueue 可以是限定容量的。它在任意给定时间都可以有一个 remainingCapacity，超出此容量，便无法无阻塞地 put 附加元素。没有任何内部容量约束的 BlockingQueue 总是报告 Integer.MAX_VALUE 的剩余容量。

BlockingQueue 实现主要用于生产者-使用者队列，但它另外还支持 Collection 接口。因此，举例来说，使用 remove(x) 从队列中移除任意一个元素是有可能的。然而，这种操作通常不 会有效执行，只能有计划地偶尔使用，比如在取消排队信息时。

BlockingQueue 实现是线程安全的。所有排队方法都可以使用内部锁或其他形式的并发控制来自动达到它们的目的。然而，大量的 Collection 操作（addAll、containsAll、retainAll 和 removeAll）没有 必要自动执行，除非在实现中特别说明。因此，举例来说，在只添加了 c 中的一些元素后，addAll(c) 有可能失败（抛出一个异常）。

BlockingQueue 实质上不 支持使用任何一种“close”或“shutdown”操作来指示不再添加任何项。这种功能的需求和使用有依赖于实现的倾向。例如，一种常用的策略是：对于生产者，插入特殊的 end-of-stream 或 poison 对象，并根据使用者获取这些对象的时间来对它们进行解释。

注意，BlockingQueue 可以安全地与多个生产者和多个使用者一起使用。

个人理解：
线程安全的一个队列，生产者从一端put对象，消费者从另一端take对象。

代码用例

	package concurrent;
	
	import java.util.Random;
	import java.util.concurrent.BlockingQueue;
	import java.util.concurrent.LinkedBlockingQueue;
	
	/**
	 * 
			抛出异常		特殊值		阻塞		超时
		插入	add(e)		offer(e)	put(e)	offer(e, time, unit)
		移除	remove()	poll()		take()	poll(time, unit)
		检查	element()	peek()		不可用	不可用
	
	 * 
	 * 典型用例 生产者-消费者
	 * @author Zhanghj
	 *
	 */
	public class BlockingQueueDemo {
		public static void main(String[] args) {
			final BlockingQueue<Integer> queue = new LinkedBlockingQueue<Integer>();
			final Random random = new Random();
			
			class Producer implements Runnable{
	
				@Override
				public void run() {
					while (true) {
						int i = random.nextInt(10);
						try {
							queue.put(i);
							if (queue.size() == 3) {
								System.out.println("队列已满。");
							}
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
					
				}
			}
			
			class Customer implements Runnable{
	
				@Override
				public void run() {
					while (true) {
						try {
							int used = queue.take();
							System.out.println(Thread.currentThread().getName()+" Customer used:"+used);
							Thread.sleep(10);
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				}
				
			}
			
			//创建一个生产者
			new Thread(new Producer()).start();
			
			//创建一百个消费者
			for (int i = 0; i < 100; i++){
				new Thread(new Customer()).start();
			}
		}
	}


### 6.SynchronousQueue

JDK解释：
一种阻塞队列，其中每个插入操作必须等待另一个线程的对应移除操作 ，反之亦然。同步队列没有任何内部容量，甚至连一个队列的容量都没有。不能在同步队列上进行 peek，因为仅在试图要移除元素时，该元素才存在；除非另一个线程试图移除某个元素，否则也不能（使用任何方法）插入元素；也不能迭代队列，因为其中没有元素可用于迭代。队列的头 是尝试添加到队列中的首个已排队插入线程的元素；如果没有这样的已排队线程，则没有可用于移除的元素并且 poll() 将会返回 null。对于其他 Collection 方法（例如 contains），SynchronousQueue 作为一个空 collection。此队列不允许 null 元素。

同步队列类似于 CSP 和 Ada 中使用的 rendezvous 信道。它非常适合于传递性设计，在这种设计中，在一个线程中运行的对象要将某些信息、事件或任务传递给在另一个线程中运行的对象，它就必须与该对象同步。

对于正在等待的生产者和使用者线程而言，此类支持可选的公平排序策略。默认情况下不保证这种排序。但是，使用公平设置为 true 所构造的队列可保证线程以 FIFO 的顺序进行访问。

个人理解：
在同步队列中，插入和移除必须同时进行（因为同步队列的容量为0，不能保存元素，相当于只是一个通道）。

代码用例：

	package concurrent;
	
	import java.util.Arrays;
	import java.util.List;
	import java.util.concurrent.BlockingQueue;
	import java.util.concurrent.SynchronousQueue;
	
	/**
	 * 一种阻塞队列，其中每个插入操作必须等待另一个线程的对应移除操作 ，反之亦然。
	 * 同步队列没有任何内部容量，甚至连一个队列的容量都没有。
	 * 不能在同步队列上进行 peek，因为仅在试图要移除元素时，该元素才存在；
	 * 除非另一个线程试图移除某个元素，否则也不能（使用任何方法）插入元素；
	 * 也不能迭代队列，因为其中没有元素可用于迭代。
	 * 队列的头 是尝试添加到队列中的首个已排队插入线程的元素；
	 * 如果没有这样的已排队线程，则没有可用于移除的元素并且 poll() 将会返回 null。
	 * 对于其他 Collection 方法（例如 contains），SynchronousQueue 作为一个空 collection。
	 * 此队列不允许 null 元素。
	 * 
	 * @author Zhanghj
	 *
	 */
	public class SynchronousQueueDemo {
		public static void main(String[] args) {
			SynchronousQueue<String> queue = new SynchronousQueue<String>();
			new Thread(new Producer(queue)).start();
			new Thread(new Customer(queue)).start();
		}
	}
	
	class Producer implements Runnable{
		
		private BlockingQueue<String> q;
		
		public Producer(BlockingQueue<String> q) {
			this.q = q;
		}
		
		@Override
		public void run() {
			List<String> list = Arrays.asList("one","two","three");
			try {
				for(int i = 0;i < 3;i++){
					q.put(list.get(i));
				}
				q.put("Done");
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		
	}
	
	class Customer implements Runnable{
		private BlockingQueue<String> q;
		
		public Customer(BlockingQueue<String> q) {
			super();
			this.q = q;
		}
	
		@Override
		public void run() {
			try {
				String obj = null;
				while (!("Done".equals(obj))) {
					obj = q.take();
					System.out.println(obj);
					Thread.sleep(3000);		//在不take时，producer不能put
				}
				
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			
		}
		
	}

### 7.ScheduledThreadPoolExecutor

JDK解释：
ThreadPoolExecutor，它可另行安排在给定的延迟后运行命令，或者定期执行命令。需要多个辅助线程时，或者要求 ThreadPoolExecutor 具有额外的灵活性或功能时，此类要优于 Timer。

一旦启用已延迟的任务就执行它，但是有关何时启用，启用后何时执行则没有任何实时保证。按照提交的先进先出 (FIFO) 顺序来启用那些被安排在同一执行时间的任务。

虽然此类继承自 ThreadPoolExecutor，但是几个继承的调整方法对此类并无作用。特别是，因为它作为一个使用 corePoolSize 线程和一个无界队列的固定大小的池，所以调整 maximumPoolSize 没有什么效果。

扩展注意事项：此类重写 AbstractExecutorService 的 submit 方法，以生成内部对象控制每个任务的延迟和调度。若要保留功能性，子类中任何进一步重写的这些方法都必须调用超类版本，超类版本有效地禁用附加任务的定制。但是，此类提供替代受保护的扩展方法 decorateTask（为 Runnable 和 Callable 各提供一种版本），可定制用于通过 execute、submit、schedule、scheduleAtFixedRate 和 scheduleWithFixedDelay 进入的执行命令的具体任务类型。默认情况下，ScheduledThreadPoolExecutor 使用一个扩展 FutureTask 的任务类型。

个人理解：
一个能调度线程的线程池执行器，用于取代Timer

代码用例

	package concurrent;
	
	import java.util.concurrent.ScheduledThreadPoolExecutor;
	import java.util.concurrent.TimeUnit;
	
	/**
	 * ScheduledThreadPoolExecutor，它可另行安排在给定的延迟后运行命令，或者定期执行命令。
	 * 需要多个辅助线程时，或者要求 ThreadPoolExecutor 具有额外的灵活性或功能时，此类要优于 Timer。
	 * 
	 * 一旦启用已延迟的任务就执行它，但是有关何时启用，启用后何时执行则没有任何实时保证。
	 * 按照提交的先进先出 (FIFO) 顺序来启用那些被安排在同一执行时间的任务。
	 * @author Zhanghj
	 *
	 */
	public class ScheduledThreadPoolExecutorDemo {
		public static void main(String[] args) {
			ScheduledThreadPoolExecutor scheduledThreadPoolExecutor = 
											new ScheduledThreadPoolExecutor(1);
			
			scheduledThreadPoolExecutor.scheduleAtFixedRate(new Runnable() {
				@Override
				public void run() {
					System.out.println("1s later start execute,and execute once per 5s");
				}
			}, 1, 5, TimeUnit.SECONDS);
			
			scheduledThreadPoolExecutor.scheduleAtFixedRate(new Runnable() {
				@Override
				public void run() {
					System.out.println("2s later start execute,and execute once per 2s");
				}
			}, 2, 2, TimeUnit.SECONDS);
			//两个定时任务互不干扰
		}
	}

### 8.FutureTask

JDK解释：
可取消的异步计算。利用开始和取消计算的方法、查询计算是否完成的方法和获取计算结果的方法，此类提供了对 Future 的基本实现。仅在计算完成时才能获取结果；如果计算尚未完成，则阻塞 get 方法。一旦计算完成，就不能再重新开始或取消计算。

可使用 FutureTask 包装 Callable 或 Runnable 对象。因为 FutureTask 实现了 Runnable，所以可将 FutureTask 提交给 Executor 执行。

除了作为一个独立的类外，此类还提供了 protected 功能，这在创建自定义任务类时可能很有用。

个人理解：
当有一个耗时很长的计算时，可以使用FutureTask进行异步计算。期间可以进行其他任务，在合适位置调用get()来获取结果。

代码用例

	package concurrent;
	
	import java.util.concurrent.Callable;
	import java.util.concurrent.ExecutionException;
	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	import java.util.concurrent.FutureTask;
	
	/**
	 * 可取消的异步计算。利用开始和取消计算的方法、查询计算是否完成的方法和获取计算结果的方法，
	 * 此类提供了对 Future 的基本实现。仅在计算完成时才能获取结果；
	 * 如果计算尚未完成，则阻塞 get 方法。一旦计算完成，就不能再重新开始或取消计算。
	 * @author Zhanghj
	 *
	 */
	public class FutureTaskDemo {
	
		public static void main(String[] args) {
			FutureTask<String> task = new FutureTask<String>(new Callable<String>() {
				@Override
				public String call() throws Exception {
					//do something which needs long time 
					return Thread.currentThread().getName();
				}
			});
			
			ExecutorService executorService = Executors.newCachedThreadPool();
			executorService.execute(task);
			try {
				String result = task.get();
				System.out.println(result);
			} catch (InterruptedException e) {
				e.printStackTrace();
			} catch (ExecutionException e) {
				e.printStackTrace();
			}
			
		}
	}



### 后记

练习源码地址：[https://github.com/Zering/Java-SE-Core/tree/master/src/concurrent](https://github.com/Zering/Java-SE-Core/tree/master/src/concurrent)
