---
parent: 
previous: 
next: "[[Enums]]"
---
## Local Blocks

```java hl=4,6,8,10 blue=5
class A {
	public static void main(String[] args) {

		{  // block start
			int i = 10;  // local variable
		}  //block end

		{  //block start
		
		}  // block end
	}
}
```

- If there's a space separated with curly braces without using an identifier or a reserve word(if, try), they are define as *local blocks*
- They cannot be executed as programmer needs.

## Class Blocks

> [!tip]
> - Things we can keep (implement) in a class
> 
> 	 ```java
> 	class A {
> 		int i;  // non static variable
> 		static int j;  // static variable
> 		
> 		void m() {}  // non static method
> 		static void n() {}  // static method
> 		
> 		A() {}  // constructor
> 		
> 		class B() {}  // inner class
> 	
> 		{}  // class block
> 	}
> 	```

- If there's a block inside class block that's been created without static modifier, it's a non static (instance) block.

	 ```java blue=4,5,6
	class A {
		int i;
	
		{
			// instance block
		}
	
		public static void main(String[] args) {
			new A();
		}
		
		A() {
			// super();
			// load non-statics (instance)
			// call instance blocks
		}
	}
	```

- Instance blocks execute from top to bottom
- Instance blocks executed as the 3rd step of the constructor
- Instance variables can be accessed from instance blocks

	 ```java blue=4,5,6,7,8,22,23,24
	class A {
		int i;
	
		{
			System.out.println("Instance block 1");
			i = 100;
			m();
		}
	
		public static void main(String[] args) {
			new A();
		}
		
		A() {
			// super();
			// load non-statics (instance)
			// call instance blocks
		}

        m() {}

        {
			System.out.println("Instance block 2");
		}
	}
	```

> [!note]
> From instance blocks we can instance variables and methods. So them to be accessed, they should be loaded into the memory. For this, instance blocks are executed after loading non statics in the process of class construction
 
- Constructors and all the instance blocks related to a class, are executed once throughout the lifecycle of a object. It's at the object creation.

- If blocks are created inside a class scope with static modifier they are called *static blocks*

	 ```java blue=2,3,4,8,9,10
	class A {
		static {
			System.out.println("Static block 01");
		}
	
		public static void main(String[] args) {}
	
		static {
			System.out.println("Static block 02");
		}
	
	}
	```

- All the static blocks executed before main method executes. It's when loading a *.class* file loads to the JVM
- Static blocks related to a some class executed only once during a JVM lifecycle.
- In parent child relationships, executing static blocks starts from the parent.

```java
class B {
	static {
		System.out.println("Static block 1");
	}
}

class A extends B {
	static {
		System.out.println("Static block 2");
	}
	public static void main(String[] args) {
		System.out.prinln("main");
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