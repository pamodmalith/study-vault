---
parent: "[[Multithreading]]"
previous: "[[Starvation]]"
next: "[[Thread Group]]"
---
 > [!lecture]
> XXK 02

 *Daemon thread* is a low priority thread that runs in background to perform tasks such as garbage collection.
 
 1. They can not prevent the JVM from exiting when all the user threads finish their execution.
 2. JVM terminates itself when all the user threads finish their execution.
 3. If JFM finds running daemon thread, it terminates the thread and after that shutdown itself. JVM does not care whether Daemon thread is running or not.
 4. It is an utmost low priority thread.

```java
class Mythread extends Thread {
	@Override
	public void run() {
		for (int i=0; i<100; i++) {
			System.out.println("Thread is running : " + i);
		}
	}
}

public class Test {
	public static void main(String[] args) throws InterruptedException {
		MyThread mt = new MyThread();
		mt.setDaemon();
		mt.start();
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