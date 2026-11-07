---
parent: "[[Multithreading]]"
previous: "[[Inter Thread Communication]]"
next: "[[Starvation]]"
---
> [!lecture]
> XXK 01

```java
public class Test {
	public static void main(String[] args) {
		System.out.println("Start....");
		
		try{
			Thread.currrentThread().join();
		} catch (InterruptedException e) {}

		System.out.println("End...");
	}
}
```

```java
class MyThread extends Thread {
	Thread parent;

	public MyThread(Thread parent) {
		this.parent = parent;
	}

	@Override
	public void run() {
		try {
			parent.join();
		} catch (InterruptedException e) {}
	}
}

public class Test {
	public static void main(String[] args) throws InterruptedException {
		MyThrad mt = new MyThread(Thread.currentThread());
		mt.start();
		mt.join();
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