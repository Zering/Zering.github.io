---
layout:     post
title:      "Java集合类"
subtitle:   " \"Java\""
date:       2016-9-28 15:08:56   
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - java
---

## Java集合类合集

### 集合框架图

![](http://pic002.cnblogs.com/images/2012/80896/2012053020261738.gif)

### 前言

Java集合类分为两大类：Collection和Map；

Collection下主要为List和Set

### 1. List接口

`List`是一个有序的Collection接口，提供了Iterator和ListIterator。

#### 1.1、ArrayList类
`ArrayList`内部实现为一个长度可变的数组，如果不指定，初始容量为10，随着向 ArrayList 中不断添加元素，其容量也自动增长。

`ArrayList`内部的元素为插入顺序，可以存放重复元素，允许null值，不同步（同步实现的类Vector,性能不佳，不建议使用），可以用包装的方法实现同步。

**基本使用方法如下**

	package collection.list;
	
	import java.util.ArrayList;
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.List;
	import java.util.ListIterator;
	import java.util.Vector;
	
	/**
	 * ArrayList 有序（添加顺序），可重复可null，不同步，可变长度的数组
	 * @author Zhanghj
	 *
	 */
	public class ArrayListDemo {
		
		public static void main(String[] args) {
			
			//构造一个初始容量为 10 的空列表，自动的扩容和减容
			List<String> list = new ArrayList<String>();
			
			list.add("first");
			list.add("Second");
			list.add("Second Repeat");
			list.add("Second Repeat");
			list.add("Second Repeat");
			list.add(null);
			list.add("after null");
			
			//toString 输出
			System.out.println("list.toString() 输出："+list.toString());
			
			//循环遍历
			System.out.print("for循环遍历输出：");
			for(int i = 0;i < list.size();i++){
				System.out.print(list.get(i) + ",");
			}
			System.out.println();
			
			//Iterator遍历
			System.out.print("Iterator遍历输出：");
			Iterator<String> iter = list.iterator();
			while (iter.hasNext()) {
				String string = iter.next();
				System.out.print(string+",");
			}
			System.out.println();
			
			
			//ListIterator遍历，在Iterator基础上添加了add(),previous()和hasPrevious()方法
			System.out.print("ListIterator遍历输出：");
			ListIterator<String> listIterator = list.listIterator();
			while (listIterator.hasNext()) {
				String string = (String) listIterator.next();
				System.out.print(string);
				listIterator.add("|");
			}
			System.out.println();
			
			
			//for each遍历
			System.out.print("for each遍历输出：");
			for(String str:list){
				System.out.print(str+",");
			}
			System.out.println();
			
			//其他常用操作
			System.out.println("是否包含first:"+list.contains("first"));
			System.out.println("移除first:"+list.remove("first"));
			System.out.println("是否包含first:"+list.contains("first"));
			System.out.println("下标方式移除下标为0的元素："+list.remove(0));
			System.out.println("返回某个元素的下标，查找的第一个元素："+list.indexOf("Second Repeat"));
			System.out.println("返回最后一个指定元素的下标："+list.lastIndexOf("Second Repeat"));
			System.out.println("判断是否为空：" + list.isEmpty());
			
			//创建一个同步的ArrayList的方法：使用Collection.synchronizedList包装ArrayList
			@SuppressWarnings("unused")
			List<String> synchronizedList = Collections.synchronizedList(new ArrayList<String>());
			
			//另一个能实现同步的方式是使用Vector，但其性能太差，一般不用
			@SuppressWarnings("unused")
			Vector<String> vector = new Vector<String>();
		}
	}

#### 1.2、LinkedList类

`LinkedList`是有链表实现的，允许null值，不同步，既可以对头部操作，也可以在尾部操作，所以LinkedList可以用做堆栈、队列和双端队列。

**基本使用方法**
	
	package collection.list;
	
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.LinkedList;
	import java.util.List;
	import java.util.ListIterator;
	
	/**
	 * LinkedList 链表结构，有序，可实现堆栈、队列。双端队列，不同步
	 * @author Zhanghj
	 *
	 */
	public class LinkedListDemo {
		
		public static void main(String[] args) {
			
			LinkedList<String> linkedList = new LinkedList<String>();
			
			linkedList.addFirst("First");
			linkedList.offerFirst("Second"); //等价
			linkedList.add("third");
			linkedList.add("third");
			linkedList.add("third");	//可重复
			linkedList.add(null);		//可null
			linkedList.add("forth");
			linkedList.addLast("five");
			linkedList.offerLast("six");	//等价
			
			linkedList.add(3, "insert");
			
			//toString输出
			System.out.println("linkedList.toString() 输出：" + linkedList.toString());
			
			//for循环遍历
			System.out.print("for循环遍历输出：");
			for(int i = 0;i < linkedList.size();i++){
				System.out.print(linkedList.get(i));
			}
			System.out.println();
			
			//Iterator遍历
			System.out.print("Iterator循环遍历输出：");
			Iterator<String> iterator = linkedList.iterator();
			while (iterator.hasNext()) {
				String string = iterator.next();
				System.out.print(string+",");
			}
			System.out.println();
			
			//ListIterator遍历
			System.out.print("ListIterator遍历输出：");
			ListIterator<String> listIterator = linkedList.listIterator();
			while (listIterator.hasNext()) {
				String string = listIterator.next();
				System.out.print(string+",");
				listIterator.add("||");
			}
			System.out.println();
			
			//for each遍历
			for(String string:linkedList){
				System.out.print(string);
			}
			System.out.println();
			
			//其他常用操作
			System.out.println("取头元素："+linkedList.getFirst());
			System.out.println("取尾元素："+linkedList.getLast());
			System.out.println("移除头元素："+linkedList.removeFirst());
			System.out.println("移除尾元素："+linkedList.removeLast());
			System.out.println("默认remove,移除头元素："+linkedList.remove());
			
			//此方法不同步，多线程使用时用：Collections.synchronizedList(new linkedList())
			@SuppressWarnings("unused")
			List<String> synchronizedLinkedList 
					= Collections.synchronizedList(new LinkedList<String>());
		}
		
	}

### 2.Set接口

`Set`不包含重复的元素，最多含有一个null元素

#### 2.1、HashSet类

`HashSet`内部由HashMap实现，不保证 set 的迭代顺序；特别是它不保证该顺序恒久不变，允许使用 null 元素，不同步。

**基本使用方法**
	
	package collection.set;
	
	import java.util.Collections;
	import java.util.HashSet;
	import java.util.Iterator;
	import java.util.Set;
	
	/**
	 * HashSet 内部由HashMap实现，可null，无序，无重复，不同步
	 * @author Zhanghj
	 *
	 */
	public class HashSetDemo {
		
		public static void main(String[] args) {
			//初始容量为16，加载因子0.75
			Set<String> hSet = new HashSet<String>();
			hSet.add("first");
			hSet.add("Second");
			hSet.add("Second Repeat");
			hSet.add("Second Repeat");
			hSet.add("Second Repeat");
			hSet.add(null);
			
			System.out.println(hSet.toString());
			
			Iterator<String> iterator = hSet.iterator();
			while (iterator.hasNext()) {
				String string = (String) iterator.next();
				System.out.print(string);
			}
			System.out.println();
			
			for (String string : hSet) {
				System.out.print(string);
			}
			System.out.println();
			
			System.out.println("转数组："+hSet.toArray());
			System.out.println("是否包含某元素："+hSet.contains("Second"));
			System.out.println("移除某个元素："+hSet.remove("Second"));
			
			//线程同步的方式，Collections.synchronizedSet(new HashSet)
			@SuppressWarnings("unused")
			Set<String> synchronizedHashSet = Collections.synchronizedSet(new HashSet<String>());
			
		}
	}


#### 2.2、LinkedHashSet类

`LinkedHashSet`是在HashSet的基础上，维护一个双重链表，以此定义迭代顺序（插入顺序），Set不能有重复元素，插入重复元素也不会改变原有元素的位置。不同步。

**基本用法**

	package collection.set;
	
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.LinkedHashSet;
	import java.util.Set;
	
	/**
	 * LinkedHashSet hashcode决定存储位置，另外维护一个双重链表来决定次序（插入顺序）
	 * 本质是在HashSet的基础上再维护一个双重链表,用法上与HashSet一致
	 * @author Zhanghj
	 *
	 */
	public class LinkedHashSetDemo {
	
		public static void main(String[] args) {
			/*
			 * 初始化一个空的LinkedHashSet
			 * 一种常用的用法是 new LinkedHashSet(Set s)  因为获得的副本可以保持s的顺序
			 */
			Set<String> linkedHashSet = new LinkedHashSet<String>();
			
			linkedHashSet.add("first");
			linkedHashSet.add(null);
			linkedHashSet.add("Second");
			linkedHashSet.add("third");
			linkedHashSet.add("first");  //不会插入重复的元素，也不会改变原元素在set中的顺序
			linkedHashSet.add("forth");
			
			System.out.println(linkedHashSet);
			
			Iterator<String> iterator = linkedHashSet.iterator();
			while (iterator.hasNext()) {
				String string = (String) iterator.next();
				System.out.print(string);
			}
			System.out.println();
			
			for (String string : linkedHashSet) {
				System.out.print(string);
			}
			System.out.println();
			
			//实现同步的包装方法
			@SuppressWarnings("unused")
			Set<String> s = Collections.synchronizedSet(new LinkedHashSet<String>());
			
			//可参考HashSetDemo
		}
	}

#### 2.3、TreeSet类

`TreeSet`内部由TreeMap实现，是一个有序（自然顺序、大写在前）的Set实现，不允许null值，不同步。

**基本使用方法**

	package collection.set;
	
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.SortedSet;
	import java.util.TreeSet;
	
	/**
	 * TreeSet 内部由TreeMap实现（TreeMap的key），有序（自然顺序），tree结构，不同步
	 * @author Zhanghj
	 *
	 */
	public class TreeSetDemo {
		
		public static void main(String[] args) {
			TreeSet<String> treeSet = new TreeSet<String>();
			
			treeSet.add("first");
	//		treeSet.add(null);  	运行时NullPointerException
			treeSet.add("First");
			treeSet.add("first");	//Set值唯一，不会报错但也只有一个first元素
			treeSet.add("second");
			treeSet.add("Second");
			
			System.out.println(treeSet);
			
			System.out.println(treeSet.descendingSet()); //降序
			
			Iterator<String> iterator = treeSet.iterator();
			while (iterator.hasNext()) {
				String string = (String) iterator.next();
				System.out.print(string);
			}
			System.out.println();
			
			Iterator<String> descendingIterator = treeSet.descendingIterator();
			while (descendingIterator.hasNext()) {
				String string = (String) descendingIterator.next();
				System.out.print(string);
			}
			System.out.println();
			
			for (String string : treeSet) {
				System.out.print(string);
			}
			System.out.println();
			
			//其他操作
			System.out.println(treeSet.contains("first"));
			System.out.println(treeSet.first());
			System.out.println(treeSet.last());
			System.out.println("treeSet.ceiling(\"f\"):返回大于等于f的最小元素，没有返回null："+treeSet.ceiling("f"));
			System.out.println("treeSet.floor(\"f\"):返回小于等于f的最大元素，没有返回null："+treeSet.floor("f"));
			System.out.println(treeSet.higher("f"));
			System.out.println(treeSet.lower("f"));
			System.out.println(treeSet.headSet("f"));
			System.out.println(treeSet.subSet("A", "f"));
			System.out.println(treeSet.remove("first"));
			System.out.println(treeSet.pollFirst());
			System.out.println(treeSet.pollLast());
			
			//创建同步的TreeSet
			@SuppressWarnings("unused")
			SortedSet<String> synchronizedTreeSet = Collections.synchronizedSortedSet(new TreeSet<String>());
		}
	}


### 3.Map

`Map`用于存储键值对，键值唯一且一个键最多映射到一个值。

#### 3.1、HashMap类

`HashMap`以HashCode的方式确定键值对的存储位置（bucket），当初始化一个HashMap时，默认容量为16，加载因子为0.75，当两个元素的Hashcode相同时，他们以链表的方式存放在bucket中，当一个bucket中的元素过多时，会发生hash冲突，在1.8以前是通过rehash的方式将bucket扩大为两倍，1.8的做法是将链表改为红黑树。

`HashMap`运行null键和null值，不同步。

**基本使用方法**

	package collection.map;
	
	import java.util.Collections;
	import java.util.HashMap;
	import java.util.Iterator;
	import java.util.Map;
	import java.util.Map.Entry;
	
	/**
	 * HashMap:以Hashing的方式分配键值对的存储位置（bucket），
	 * 当hashcode相同时，相同的bucket上以链表的方式存储，get时先根据hashcode再用key.equals来取值
	 * 当bucket容量达到加载因子(默认0.75，即75%)的规定时，会将bucket的容量扩大两倍（rehash）
	 * 允许null键和null值，无序
	 * @author Zhanghj
	 *
	 */
	public class HashMapDemo {
	
		public static void main(String[] args) {
			//初始化一个容量16，加载因子0.75的空map
			HashMap<String, String> hashMap = new HashMap<String, String>();
			
			hashMap.put("one", "first");
			hashMap.put("two", "second");
			hashMap.put("three", "third");
			hashMap.put(null, null);
			hashMap.put("four", "forth");
			hashMap.put("four", "modify");	//key值唯一，对同一key的put操作相当于更新
			
			//由于hashmap运行null值，所以判断键值null时，用containsKey方法来判断
			System.out.println(hashMap.containsKey(null));
			
			System.out.println(hashMap);
			
			Iterator<Entry<String, String>> iterator = hashMap.entrySet().iterator();
			while (iterator.hasNext()) {
				Entry<String, String> entry = iterator.next();
				System.out.println("key:"+entry.getKey()+" ,value:"+entry.getValue());
			}
			
			System.out.println(hashMap.remove("tree"));
			System.out.println(hashMap.replace("two", "another second"));
			System.out.println(hashMap.remove("one", "first"));
			System.out.println(hashMap.replace("four", "modify", "another four"));
			
			System.out.println(hashMap);
			
			//hashmap线程不安全，多线程中使用需要进行包装，或者使用hashtable/concurrentHashMap
			@SuppressWarnings("unused")
			Map<String, String> synchronizedHashMap = Collections.synchronizedMap(new HashMap<String, String>());
		}
		
	}

#### 3.2、LinkedHashMap类

`LinkedHashMap`在HashMap的基础上，维护一个双重链表，决定元素的顺序（插入顺序），该类能保证元素顺序为插入顺序，常用来复制其他的Map类型以确保和原Map的存储顺序一致。允许null元素，不同步。

**基本使用方法**

	package collection.map;
	
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.LinkedHashMap;
	import java.util.Map;
	import java.util.Map.Entry;
	
	/**
	 * LinkedHashMap:在hashmap上，维护一个双重链表，决定迭代顺序（插入顺序），对相同的键多次插入不会影响其位置
	 * @author Zhanghj
	 *
	 */
	public class LinkedHashMapDemo {
		
		public static void main(String[] args) {
			LinkedHashMap<String, Integer> linkedHashMap = new LinkedHashMap<String, Integer>();
			
			linkedHashMap.put("first", 1);
			linkedHashMap.put("second", 2);
			linkedHashMap.put("three", 3);
			linkedHashMap.put("four", 4);
			linkedHashMap.put(null, 0);
			linkedHashMap.put("five", 5);
			
			System.out.println(linkedHashMap);
			
			Iterator<Entry<String, Integer>> iterator = linkedHashMap.entrySet().iterator();
			while (iterator.hasNext()) {
				Entry<String, Integer> entry = iterator.next();
				System.out.println("key:"+entry.getKey()+"value:"+entry.getValue());
			}
			
			
			//复制一个已有的任意Map,可以确保copy与原Map完全一致
			Map<String, Integer> copy = new LinkedHashMap<String, Integer>(linkedHashMap);
			System.out.println(copy);
			
			//同步包装
			@SuppressWarnings("unused")
			Map<String,Integer> m = Collections.synchronizedMap(new LinkedHashMap<String,Integer>());
			
			//其他操作参考LinkedHashSet
			
		}
	}

#### 3.3、TreeMap类

`TreeMap`是一个以红黑树来实现有序的Map，顺序为自然顺序，key值不能为null,不同步。

**基本使用方法**

	package collection.map;
	
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.Map.Entry;
	import java.util.SortedMap;
	import java.util.TreeMap;
	
	/**
	 * TreeMap 基于红黑树实现，有序（键的自然顺序，或者自定义Comparator）
	 * @author Zhanghj
	 *
	 */
	public class TreeMapDemo {
		public static void main(String[] args) {
			TreeMap<String, Integer> treeMap = new TreeMap<String, Integer>();
			
			treeMap.put("first", 1);
			treeMap.put("two", 2);
			treeMap.put("three", 3);
	//		treeMap.put(null, 0);	不可为null，空指针异常
			treeMap.put("foue", 4);
			treeMap.put("five", 5);
			
			System.out.println(treeMap);
			
			Iterator<Entry<String, Integer>> iterator = treeMap.entrySet().iterator();
			while (iterator.hasNext()) {
				Entry<String, Integer> entry = iterator.next();
				System.out.println("key:"+entry.getKey()+" ,value:"+entry.getValue());
			}
			
			//同步包装
			@SuppressWarnings("unused")
			SortedMap<String,Integer> m = Collections.synchronizedSortedMap(new TreeMap<String,Integer>());
			
			//常用操作参考TreeSet
		}
	}

#### 2.4、WeakHashMap类

`WeakHashMap`是一个以弱键实现的HashMap，当某个键不再正常使用时，将自动移除其条目。更精确地说，对于一个给定的键，其映射的存在并不阻止垃圾回收器对该键的丢弃，这就使该键成为可终止的，被终止，然后被回收。丢弃某个键时，其条目从映射中有效地移除，因此，该类的行为与其他的 Map 实现有所不同。允许null元素，不同步。

**基本使用方法/弱键演示**

	package collection.map;
	
	import java.util.WeakHashMap;
	
	/**
	 * WeakHashMap:弱键实现的HashMap,当一个键不在正常使用时，自动移除（该类不阻止gc的回收）
	 * @author Zhanghj
	 *
	 */
	public class WeakHashMapDemo {
		
		public static void main(String[] args) {
	        int size = 100;
	
	        if (args.length > 0) {
	            size = Integer.parseInt(args[0]);
	        }
	
	        Key[] keys = new Key[size];
	        WeakHashMap<Key, Value> whm = new WeakHashMap<Key, Value>();
	
	        for (int i = 0; i < size; i++) {
	            Key k = new Key(Integer.toString(i));
	            Value v = new Value(Integer.toString(i));
	            if (i % 3 == 0) {
	                keys[i] = k;//强引用
	            }
	            whm.put(k, v);//所有键值放入WeakHashMap中
	        }
	
	        System.out.println(whm);
	        System.out.println(whm.size());
	        System.gc();
	        
	        try {
	            // 把处理器的时间让给垃圾回收器进行垃圾回收
	            Thread.sleep(4000);
	        } catch (InterruptedException e) {
	            e.printStackTrace();
	        } 
	        
	        System.out.println(whm);
	        System.out.println(whm.size());
	    }
	
	}
	
	class Key {
	    String id;
	
	    public Key(String id) {
	        this.id = id;
	    }
	
	    public String toString() {
	        return id;
	    }
	
	    public int hashCode() {
	        return id.hashCode();
	    }
	
	    public boolean equals(Object r) {
	        return (r instanceof Key) && id.equals(((Key) r).id);
	    }
	
	    public void finalize() {
	        System.out.println("Finalizing Key " + id);
	    }
	}
	
	class Value {
	    String id;
	
	    public Value(String id) {
	        this.id = id;
	    }
	
	    public String toString() {
	        return id;
	    }
	
	    public void finalize() {
	        System.out.println("Finalizing Value " + id);
	    }
	
	}


### 后记

本文用于复习Java集合类的最基本用法，联系源码地址：[https://github.com/Zering/Java-SE-Core](https://github.com/Zering/Java-SE-Core)
