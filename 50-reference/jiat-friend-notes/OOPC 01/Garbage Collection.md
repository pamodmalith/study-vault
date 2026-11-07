---
parent: 
previous: "[[UML Class Diagrams]]"
next: "[[Constructors]]"
---
### Garbage Collection in Computer Science
- In *computer science, garbage collection (GC)* is a for form of automatic memory management. The *garbage collector*, or just *collector*, attempts to reclaim *garbage*, or memory occupied y objects that re no longer in use by program. Garbage collection was invented by *John McCarthy* around 1959 to simplify manual memory management in *Lisp*

## Java Garbage Collection
- In Java, garbage means unreferenced values.
- *Garbage Collection* is process of reclaiming the runtime unused memory automatically. In other words, it is a way to destroy the unused objects.
- To do so, we were using *free()* function in C language and *delete()* in C++. but, in java it is performed automatically. So, Java provides better memory management.
- *Garbage Collection* is a software located in *Java Virtual Machine*.
- It is carried by a *Daemon Thread* called *Garbage Collector*.

### Advantages of Garbage Collection
- It makes java *memory efficient* because garbage collector removes the unreferenced objects from heap memory
- It is *automatically done* by the garbage collector (a part of JVM) so we don't need to make extra efforts.

## Garbage Collection Types
- Nulling Reference
- Reassign
- Local
- Isolated Island

### Nulling Reference
```java
Class A {
	pubic static void main(String args[]) {
		A a new A();
		a = null;
	}
}
```

![[garbage-collection-null-reference.svg]]

### Reassigning
```java
Class A {
	public static void main(String args[]) {
		A a = new A();
		a = new A();
	}
}
```

![[garbage-collection-reassign.svg]]

### Local
```java
Class A {
	public static void method() {
		A a = new A();
	}
	public static void main(String args[]) {
		method();
	}
}
```

![[garbage-collection-local.svg]]

### Isolated Island
```java
Class A {
	A a;

	public static void main(String args[]) {
		A a1 = new A();
		A a2 = new A();
		A a3 = new A();
		a1.a = a2;
		a2.a = a3;
		a3.a = a1;
		a1 = null;
		a2 = null;
		a3 = null;
	}
}
```

![[garbage-collection-isolated-island.svg]]

#### finalize() Method [protected Void finalize() {}]
```java
class A {
	public void finalize() {
		System.out.println("Remove");
	}

	public static void main(String args[]) {
		A a1 = new A();
		a1 = null;

		try {
			Tread.sleep(5000);
		} catch (Exception e) {}
	}
}
```

- finalize() object class - deprecated
#### gc() Method
```java
class A {
	public void finalize() {
		System.out.println("Remove");
	}

	public static void main(String args[]) {
		A a1 = new A();
		a1 = null;

		System.gc(); // alternatively we can use Runtime.getRuntime().gc()

		try {
			Tread.sleep(5000);
		} catch (Exception e) {}
	}
}
```

- we can use `System.gc()` or `Runtime.getRuntime().gc()` to call the finalize method


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