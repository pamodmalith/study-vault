---
parent: 
previous: "[[Garbage Collection]]"
next:
---
```java
Class A {
	A() {
		System.out.println("A()");
	}
	
	pubic static void main(String args[]) {
		new A();
	}
}
```

- A constructor is defined as same as the class name
- Constructors don't have a return type. Only has a parameter list.
- Constructor automatically applied if the class already don't have a constructor defined. And it's a no argument constructor.
- Constructors can be overloaded
	 ```java
	class A {
		A() {
			System.out.println("A()");
		}
	
		A(int i) {
			System.out.println("A(int)");
		}
		public static void main(String[] args) {
			new A(10);
		}
	}
	```

```java
class A {
	int x;

	A() {}
	A(int i) {
		System.out.println("A(int)");
		x = i;
	}

	public static void main(String[] args) {
		new A(20);
	}
}
```

![[constructor-mechanism-simple.svg]]

- First statement of any constructor is super()
- It's because constructor of the Object class should be called before any other constructor being called. After calling the Object class's constructor, other constructors being called in order of the hierarchy.

![[constructor-execution-order.svg]]
> [!important]
> - We use `super` keyword to access parent class methods and variables.
> - We use `super` method to call the parent class's constructor

```java
class B {
	int x;
	
	B() {
		System.out.println("B()");
	}
}

class A extends B {
	A(){
		System.out.println("A()");
	}

	A(int i) {
		super();  // super method
		super.x = i;  // super keyword
		System.out.println("A(int)");
	}
	
	public static void main(String[] args) {
		new A(10);
	}
}
```

- `super()` should be the first method in any constructor. Only if the programmer uses it.
- We can call parent constructors that are with arguments through super method
	``` java hl=13
	class B{
		B() {
			System.out.println("B()");
		}
	
		B(int i) {
			System.out.prinln("B(int)");
		}
	}
	
	class A extends B {
		A() {
			super(100);
			System.out.println("A()");
		}
	
		public static void main(String[] args) {
			new A();
		}
	}
	```

- We can use `this()` as a alternative for `super()`
	- It's used to call a constructor of the same class.
	- Cannot use both at the same time 

	``` java hl=9
	class B{
		B() {
			System.out.println("B()");
		}
	}
	
	class A extends B {
		A() {
			this(false);
			System.out.println("A()");
		}

		A(boolean b) {
			// super();
			System.out.pritnln("A(boolean)");
		}
	
		public static void main(String[] args) {
			new A();
		}
	}
	```

	``` java %% error=9,14
	class B{
		B() {
			System.out.println("B()");
		}
	}
	
	class A extends B {
		A() {
			// this will result in a recursion (compiler error)
			this(false);
			System.out.println("A()");
		}

		A(boolean b) {
			this();
			System.out.pritnln("A(boolean)");
		}
	
		public static void main(String[] args) {
			new A();
		}
	}
	```

#### Statements in a Constructor
1. super()
2. load all instance/non static (variables, methods)
3. call instance blocks
4. user defined code

```java
class A {
	A() {
		// 1st - super();
		// 2nd - load all instance/non static (variabls, methods)
		// 3rd - call instance blocks
		// 4th - System.out.println("B()");
	}
}
```

> [!NOTE] Tip
> We can use `javap` command to inspect a *.class* file

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