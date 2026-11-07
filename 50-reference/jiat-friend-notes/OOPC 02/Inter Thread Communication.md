---
parent: "[[Multithreading]]"
previous: "[[Synchronization]]"
next: "[[Dead Lock]]"
---
> [!lecture]
> XXK 01

![[thread-lifecycle-full.svg]]

```java
class X {
	synchronized void m() {
		System.out.println("m() start");
		
		try {
			System.out.println("i'm waiting...");
			this.wait();
		} catch (Exception e) {}
		
		for (int i=10; i>0; i--) {
			System.out.println("i'm printing : " + i);
			try {
				Thread.sleep(1000);
			} catch (Exception e) {}
		}

		System.out.println("m() done");
	}
	
	synchronized void m1() {
		System.out.println("m1() start");

		for (int i=0; i<10; i++) {
			System.out.println("i'm printing :" + i);
			try {
				Thread.sleep(1000);
			} catch (Exception e) {}
		}
		
		try {
			this.notify();
		} catch (Exception e) {}
		
		System.out.println("m1() done");
	}
}

class MyThread1 extends Thread {
	X x;
	
	public MyThread1(X x) {
		this.x = x;
	}
	
	public void run() {
		x.m();
	}
}

class Mythread2 extends Thread {
	X x;
	
	public MyThread2 (X x) {
		this.x = x;
	}
	
	public void run() {
		x.m1();
	}
}

public class Test {
	public static void main(String[] args) throws Exception {
		X x = new X();
		
		MyThread1 t1 = new MyThread1(x);
		MyThread2 t2 = new MyThread2(x);
		
		t1.start();
		t2.start();
		
		t1.join();
		t2.join();
		
		System.out.prinln("main end");
	}
}
```

```java blue=7,8,9
class X {
	void m() {
		System.out.println("m() start");
		
		try {
			System.out.println("i'm waiting...");
			synchronized (this) {
				this.wait();
			}
		} catch (Exception e) {}
		
		// ...
	}
	
	synchronized void m1() {
		// ...
		
		try {
			this.notify();
		} catch (Exception e) {}
		
		// ...
	}
}
```

```java
class X {
	synchronized void m() {
		System.out.println("m() start");
		
		for (int i=10; i>0; i--) {
			System.out.println("i'm printing : " + i);
			try {
				Thread.sleep(1000);
			} catch (Exception e) {}
		}

		System.out.println("m() done");
	}
	
	synchronized void m1() {
		System.out.println("m1() start");

		for (int i=0; i<10; i++) {
			System.out.println("i'm printing :" + i);
			try {
				Thread.sleep(1000);
			} catch (Exception e) {}
		}
		
		System.out.println("m1() done");
	}
}

class MyThread1 extends Thread {
	X x;
	
	public MyThread1(X x) {
		this.x = x;
	}
	
	public void run() {
		try{
			synchronized(x) {
				x.wait();
			}
		} catch (Exception e) {}
		x.m();
	}
}

class Mythread2 extends Thread {
	X x;
	
	public MyThread2 (X x) {
		this.x = x;
	}
	
	public void run() {
		x.m1();
		try{
			synchronized(x) {
				x.notify();
			}
		} catch (Exception e) {}
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