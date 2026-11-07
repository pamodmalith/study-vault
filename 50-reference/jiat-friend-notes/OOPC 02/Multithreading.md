---
parent: 
previous: "[[Exception Handling]]"
next: "[[Synchronization]]"
---
> [!resources]
>- [[Multithreading.pdf]]
>- [[Multithreaded Programming Guide.pdf]]

Thread of execution is the smallest sequence of programmed instructions.

1. Introduction
2. The ways to define a Thread
	- By extending Thread class
	- By implements Runnable Interface
3. Getting & Setting name of Thread
4. Thread Priorities
5. The method to prevent Thread execution
	- yield()
	- join()
	- sleep()
6. Synchronization
7. Inter Thread Communication
8. Deadlock
9. Daemon Threads
10. Multithreading enhancement

## What is *Multitasking*
- Multitasking is a process of executing multiple tasks simultaneously.
- There are 2 types of *multitasking*
	1. Process based multitasking
	2. Thread based multitasking

### Process Based *Multitasking* (Multiprocessing)
- Executing several tasks simultaneously, where ach task is a separate independent process, is called process based multitasking.

	Ex: While typing a java program in editor we can listen to songs by mp3 player in the same system. At the same time we can download a file the net. All these task are executing simultaneously and independent of each other. Hence, it is process based multitasking.

> [!hint]
> Process based multitasking is best suitable at OS level.
> 
> 1. Each process has an address in memory. In other words, each process allocates a separate memory area.
> 2. A process is heavyweight.
> 3. Cost of communication between the process is high.
> 4. Switching from one process to another requires some time for saving and loading registers, memory maps, updating lists, etc.

### Process Based *Multitasking* (Multiprocessing)
- Executing several tasks simultaneously, where each task is a separate independent part of the same program, is called thread based multitasking.
- Each independent parts is called thread.

> [!hint]
> Thread based multitasking is best suitable at Programmatic level
> 
> 1. Thread share the same address space.
> 2. A tread is lightweight.
> 3. Cost of communication between the thread is low.

- Whether it is *process based* or *thread based* the main objective a multitasking is to improve performance of the system by reducing response times.
- The main application areas of multithreading are
	- developing video games,
	- multimedia graphics
	- implementing animations.
	- to develop web server and application server
- Java provides inbuilt support for multithreading with a rich api (Thread, Runnable, ThreadGroup, ThreadLocal)
- Being a programmers we have to know hot to use this api and we are not responsible to define that api. Hence, developing multithreading programs is very easy in java when compared with c++.

## What is a *Thread*
1. Thread is nothing but separate path of sequential execution.
2. the independent execution technical name is called thread.
3. Whenever different parts of the program  executed simultaneously that each every part is called thread.
4. The thread is light weight process because whenever we are creating thread, it isn't occupying a separate memory, it uses the same memory. Whenever the memory is shared means it is not consuming more memory.
5. Executing more than one thread a time is called multithreading.

- We can define a thread in the following 2 ways.
	- By extending *Thread* class
	Create a class extending standard *java.lang.Thread* class
	- By implementing *Runnable* interface
	Create a class implementing *java.lang.Runnable* interface

### First Approach to Create Thread - Extending *Thread* Class
- [1] Step 1 : Create a class that is extended by *Thread* classes and override the *run()* method
```java
class MyThread extends Thread {
	public void run() {
		System.out.println("child - thread");
		System.out.println("body of the thread");
	}
}
```

- [1] Step 2 : Create a *Thread* object
```java
MyThread t = new Mythread();
```

- [1] Step 3 : Start the execution of the thread
```java
t.start();
```

1. Whenever we are calling *start()* method the JVM search for the *start()* in the *MyThread* class but the *start()* method is not present in the *MyThread* class so JVM goes to parent class called *Thread* class and search for the *start()* method.
2. In the *Thread* class, *start()* method is available hence JVM in executing *start()* method.
3. Whenever the *Thread* class start that *start()* is responsible person to call *run()* method.
4. Finally the *run()* automatically executed whenever we are calling *start()*  method
5. Whenever we are giving a chance to the *Thread* class *start()* method then only a new thread will be created.
	We cannot guarantee that a new thread will be created every time  we call the *start()* method

### Second Approach to Create Thread - Implementing *Runnable* Interface

- [1] Step 1 : Create a class that implements *Runnable* interface.
```java
class MyRunnable implements Runnable {
	public void run () {
		System.out.println("run...");
		System.out.println("body of the thread");
	}
}
```

- [1] Step 2 :  Create a object
```java
MyRunnable obj = new MyRunnable();
```

- [1] Step 3 : Create a *Thread* class object
```java
Thread t = new Thread(obj);
```

- [1] Step 4 : Starts the execution of the thread
```java
t.start();
```

---
TODO:
XXH 04 Recording Note - Couldn't record it

---
#### Thread Scheduler
1. Thread scheduler is a part of the JVM. It decides which thread is executed first and which thread is executed next.
2. Only one thread is executed at a time.
3. We can't expect exact behavior of the thread scheduler it is JVM vendor dependent. So we can't expect output of the multithreaded examples we can say the possible outputs.

##### Difference between *start()* and *run()* Methods
1. In the case of **start()**, *Tread* class **start** is executed a new thread will be created, that is responsible for the execution of **run()** method
2. But in the case of **run()** method, no new thread will be created, and the **run()** is executed like a normal method call by the main thread.

- Overloading of the *run()* method is possible, but *Thread* class *start()* method always calls only the no argument *run()* method. But after the *run()* method we have to call explicitly just like a normal method call.
	 ```java
	class MyThread extends Thread {
		public void run() {
			System.out.println("no args run()");
		}
	
		public void run(int i) {
			System.out.println("int args run()");
		}
	}
	
	class ThreadDemo {
		public static void main(String[] args) {
			MyThread t = new MyThread();
			t.start();
		}
	}
	
	// here run() is called when we call for start.
	```

- It is highly recommended to override *run()* method
	 ```java
	class MyThread extends Thread {}
	
	class ThreadDemo {
		public static void main(String[] args) {
			MyThread t = new MyThread();
			t.start();
		}
	}
	```
	If we are not overriding the *run()* method, the *Thread* class run method is executed which is having empty implementation, so we are not getting any output.

- If we are overriding *start()* method, then JVM is executes overridden *start()* method. At this situation we are not giving chance to *Thread* class *start()* method, hence no new thread will be created, only one thread is available the name of that thread is main thread.
	 ```java
	class MyThread extends Thread {
		public void start() {
			System.out.println("override start method");
		}
	}
	
	public class Test {
		public static void main(String[] args) {
			MyThread = t new MyThread();
			t.start();
			for (int i=0; i<5, i++) {
				System.out.println("main thread");
			}
		}
	}
	```

	- It is not recommended to override *start()* method
	- If we want to add some functionality  to start method like logging, we can do that by adding `super.start()` 

	 ```java
	class MyThread extends Tread {
		public void start() {
			super.start();
			// rest of code here (ex: logging...)
		}
	}
	```

### Thread Lifecycle
![[thread-lifecycle.svg]]

- The outputs of the program depends on the way *Thread Scheduler* allocates and deallocates threads that are in *ready/runnable* state, to the processor. For that reason we can't say exact output, only possible outputs can be said.

- Every thread in Java has some name if may be default name provided by the JVM or customized name provided by the programmer.
	The following methods are useful to set and get the name of a *Thread*.
	1. *public final String getName()*
	2. *public final void setName(String name)*

### *Thread* Class Constructors
![[thread-constructors.svg]]
- A special *system* stack size used for JVM internal threads; for example, the garbage collection and code generation thread, the default system stack size is 256 KB on al platforms.

### Thread Priorities
```java
class MyThread extends Thread {
	public void run() {
		System.out.println("A");
	}
}

public class Test {
	public static void main(String[] args) {
		MyThread mt = new MyThread();
		mt.setPriority(Thread.MAX_PRIORITY);
		mt.start();
		System.out.println("main mehtod");
		System.out.println("main");
	}
}
```

- We can get the priority of a thread by `.getPriority()` method
	 ```java
	public class Test {
		public static void main(String[] args) {
			System.out.println("main " + Thread.currentThread().getPriority());
			
			MyThread mt = new MyThread();
			mt.setPriority(Thread.MAX_PRIORITY);
			System.out.println(mt.getName() + " " + mt.getPriority());
			//...
		}
	}
	```

- Every *Thread* in java has some priority. It may be default priority provided by the JVM or customized priority provided by the programmer.
- The valid range of thread priorities are 1 - 10. Where q is the lowest priority and 10 is the highest priority.
- The default priority of main thread is 5. The priority of child thread is inherited from the parent.
- *Thread Scheduler* will use priorities while allocating processor the thread which is having highest priority will get change first and the thread which is having low priority.
- If two thread having the priority then we can't expect exact execution order it depends upon *Thread Scheduler*.
- The thread which is having low priority has to wait until completion of high priority threads.
- Three constant values for the thread priority
	1. MIN_PRIORITY = 1
	2. NORM_PRIORITY = 5
	3. MAX_PRIORITY = 10
- Here *priority* indicates a number which is in the allowed range of 1 - 10. Other we will get *Runtime* exception saying `IllegalArgumentException`.

### *Thread* Class Methods
#### Sleep()
- The *sleep()* method causes the current thread to sleep for a specified amount of time in milliseconds.
<br>
- `public static void native void sleep(long millis) throws InteruruptedException`
- `public static void sleep(long millis, int nanosec) throws InterruptedException`

```java
class Test {
	public static void main(String[] args) {
		for (int i = 0; i < 10; i++) {
			System.out.println("Hi");
			try {
				Thread.sleep(5000);
			} catch (InterruptedException ex) {
				System.out.println("Error");
			}
		} 
	}
}
```

![[thread-lifecycle-time-waiting-state.svg]]

- The Java *Thread.sleep()* method can be used to pause the execution of the current thread for a specified time in milliseconds. The argument value for milliseconds cannot be negative. Otherwise, it throws *IllegalArgumentException*.
- *sleep(long millis, int nanos)* is another method that can be used to pause the execution of the current thread for a specified number of milliseconds and nanoseconds. The allowed nanoseconds values are between 0 and 999,999
- *Thread.sleep()* interacts with the thread scheduler to put the current thread in a time waiting state for a specified period of time. Once the wait time is over, the thread state is changed to runnable state and waits for the CPU for further execution. The actual time that the current thread sleeps depends on the thread scheduler that is part  of the operating system.
	- It always pauses the current thread execution.
	- The actual time the thread sleeps before waking up and starting execution depends on system timers and schedulers. For a quiet system., the actual time for sleep is near to the specified sleep time, but for a busy system, it will a little bit longer.
	- Any other thread  can interrupt the current thread in sleep, and in such cases *InterruptedException* is thrown

#### Java.lang.Thread.yield()
The *yield()* method is used in Java to hint the thread scheduler to move the currently executing task to the ready state and move another task or thread to the running state. The tread scheduler is free to ignore the hint. This method essentially signals to the scheduler that the thread is willing to pause its execution temporarily, giving other threads a chance to execute. it's a way for threads to cooperate and share processor time more efficiently.

- *yield()* method cause to pause current executing Thread for fiving the chance for waiting threads of same priority.
- If there are no waiting threads or all threads are having low priority then the same thread will continue its execution once again.

![[thread-lifecycle-yield.svg]]
```java
public class MyThread extends Thread {
	public static void main(String[] args) {
		Thread t1 = new MyThread();
		Thread t2 = new MyThread();
		t1.setName("Thread 01");
		t2.setName("Thread 02");
		t1.start();
		t2.start();
	}

	@Override
	public void run() {
		for (int i=0; i<5; i++) {
			System.out.println(Thread.currentThread().getName());
			Thread.yeild();
		}
	}
}
```

#### Java.lang.Thread.join()
If a Thread wants to wait until completing some other thread then we should go for *join()* method.

- *public final void join() throws InterruptedException*
- *public final void join(long ms) throws InterruptedException*
- *public final void join(long ms, int ns) throws InterruptedException*

![[thread-lifecycle-join.svg]]


```java
public class MyThread extends Thread {
	public static void main(String[] args) throws InterruptedException {
		Thread t1 = new MyThread();
		Thread t2 = new MyThread();
		t1.setName("Thread 01");
		t2.setName("Thread 02");
		t1.start();
		t1.join();
		t2.start();
	}

	@Override
	public void run() {
		for (int i=0; i<5; i++) {
			System.out.println(Thread.currentThread().getName());
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```

![[thread-methods-summery.svg]]

#### public void interrupt()
- *Interrupt()* has no effect when the thread is not in a state of *waiting* or *time waiting* state.
	 ```java
	class MyThread extends Thread {
		public void run() {
			for (int i=0; i<10; i++) {
				System.out.println("I not sleeping");
			}
		}
	}
	
	class A {
		public static void main(String[] args) {
			MyThread t = new MyThread();
			t.start();
			t.interrupt();
		}
	}
	```

```java
class MyThread extends Thread {
	public void run() {
		try {
			for (int i=0; i<10; i++) {
				System.out.println("I am sleeping");
				Thread.sleep(5000);
			}
		} catch (InterruptedException i) {
			System.out.println("I got interrupted by interrupt() method call");
		}
	}
}

class A {
	public static void main(String[] args) {
		MyThread t = new MyThread();
		t.start();
		t.interrupt();
	}
}
```


--- start-multi-column: ExampleRegion1  
```column-settings  
number of columns: 2
border: disabled
shadow: off
```

> [!previous]
> `=this.previous`

--- end-column ---

> [!next]
> `=this.next`

--- end-multi-column