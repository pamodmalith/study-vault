---
parent: "[[Multithreading]]"
previous: "[[Multithreading]]"
next: "[[Inter Thread Communication]]"
---
XXJ 03 note should be completed and recording should be completed(no sound in it)

---
### Synchronized Block in Java
- Synchronized block can be used to perform synchronization on any specific resource of the method.
- Suppose you have 50 lines of code in your method, but you want to synchronize only 5 lines, you can use synchronized block.
- If you put all the codes of the method in the synchronized block, it will work as same as the synchronized method.

#### Points to Remember for Synchronized Block
- Synchronized bock is  used to lock an object for any shared resource.
- Scope of any synchronized block is smaller than the method.

![[synchronized-block-syntax.svg]]

---
1. What is *synchronized* keyword and where we can apply it?
	The synchronized keyword in Java is used to prevent thread interference and ensure thread safety by allowing only one thread to access a block or method at a time.
	
	Where can it can be applied :
	- *synchronized method* :  Only one thread can execute the method at a time.
	- *synchronized block* - Used for finer control; synchronized only part of the method.
	- *static synchronized method* - Locks the class level, not the object level.
<br>
2. Explain advantages of *synchronized* keyword
	- *Thread safety* : Ensures that shared data is accessed by only one thread at a time.
	- *Data integrity* : Prevents race conditions and maintains consistent object state.
	- *Easy to use* : Simplifies the implementation of critical sections without external libraries.
<br>
3. Explain disadvantage of *synchronized* keyword
	- *Performance overhead* : Synchronization adds cost due to context switching and locking.
	- *Deadlock risk* : Improper use can lead to deadlocks if multiple threads wait for locks indefinitely.
	- *Reduced concurrency* : Only one thread can access synchronized code, reducing parallelism.
	- *Blocking* : Threads that try to access synchronized method or blocks are blocked if the lock is held by another thread.
<br>
4. What is *class level* lock and when it's required?
	A *class level lock* is acquired when a *static synchronized method* is called. It locks the .class object of the class, not any instance.
	  ```java
	public class MyClass {
		public static synchronized void show() {
			// locked on MyClass.class
		}
	}
	```
	
	**When is  it required?** : When you want to synchronize access to *static data* or shared resources across all instances of a class.
	
5. What is difference between *class level* lock and *object level* lock?
	![[class-lvl-lock-vs-object-lvl-lock.svg]]
6. Is thread can acquire multiple locks simultaneously? Explain. 
	Yes, a thread can acquire multiple locks simultaneously, but with caution.
	```java
	synchronized (obj1) {
		synchronized (obj2) {
			// thread holds lock on both obj1 and obj2
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