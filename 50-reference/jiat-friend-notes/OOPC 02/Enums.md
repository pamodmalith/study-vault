---
parent: 
previous: "[[Blocks]]"
next: "[[Assertions]]"
---
> [!resources]
> - [[Enum.pdf]]

- Custom data type created by using *enum* 
	 ```java
	public enum Day {
		SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
	}
	```

- Objects cannot be created from a enum
	 ```java error=5 hl=6
	enum Status { ON, OFF }
	
	public class A {
		public static void main(String[] args) {
			Status s = new Status()
			Status s = Status.ON
		}
	}
	```

- Enums are case sensitive
	 ```java error=5 hl=6
	enum State { ON, OFF }
	
	public class A {
		public static void main(String[] args) {
			State s = State.on;
			State s = State.ON;
		}
	}
	```

- We can only assign values of a enum to variable typed with that enum
	 ```java error=5 hl=6
	enum State { ON, OFF }
	
	public class A {
		public static void main(Sring[] args) {
			State s = "ON";	
			State s = State.ON;
		}
	}
	```

- Enums without values can be created. We can only declare variables with that, but cannot be initialized.
	 ```java
	enum State {}
	
	public class A {
		public static void main(String[] args) {
			State s;
		}
	}
	```

- When an enum is created, compiler converts it's to a classed in the compile time. During that all the values are converted in to *public static final variables*
	 ```java
	public enum Day {
		SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
	}

	// after compilation
	final class Day extends java.lang.Enum<Day> {
		public static final Day SUNDAY;
		public static final Day MONDAY;
		...
	}
	```

- After an enum is compiled, the resulting class will have 2 methods
	 ```java
	public enum Day {
		SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
	}

	// after compilation
	final class Day extends java.lang.Enum<Day> {
		...
		public static Day[] values();
		public static Day valueOf(java.lang.String);
	}
	```

- The `java.lang.Enum` abstract class cannot be explicitly extended to a class.
- An *enum* cannot be extends with a concrete or abstract class. Because resulting class has *final* keyword

### History
- From Java 1.0 to 4.0, it didn't have Enums because it could be implemented with constants. (Enum in old languages)
```java
class Day {
	public static final Day SUNDAY = new Day();
	public static final Day MONDAY = new Day();
	public static final Day TUESDAY = new Day();
	public static final Day WEDNESDAY = new Day();
	public static final Day THURSDAY = new Day();
	public static final Day FRIDAY = new Day();
	public static final Day SATURDAY = new Day();
}

public class A {
	public static void main(String[] args) {
		Day day = Day.SUNDAY;
	}
}
```

![[enum_self_stack_heap.png]]

- In Enums in old languages we could only add required values separated by commas.
- Java 1.5 introduced a powerful Enum than that is found in old languages.
	This gave us ability to implement things found in class concept in addition to *enum values*
	 ```java
	enum Day {
		SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
	
		int i;
	
		void m();
		{} // blocks
		
		Day() {} //constructor
	
		class A {}
	
		enum X {}
	}
	```

#### Rules Associated with Enum
- A semicolon should be added at the end of the values if things related to class concept being implemented in a Enum
	 ```java blue=3
	enum Day {
		SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
		THURSDAY, FRIDAY, SATURDAY;
	
		int i;
		void m() {}
	}
	```

- Values must at the the top most of the enum
	 ```java error=2,3
	enum Day {
		int i;
		void m() {}

		SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
		THURSDAY, FRIDAY, SATURDAY;
	}
	```

- Can be used as a main class
	 ```java
	enum Day {
		SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
		THURSDAY, FRIDAY, SATURDAY;
	
		public static void main(String[] args) {
			System.out.println("Enum class");
		}
	}
	```

- *final* non access modifier cannot be used with an enum. Because in the compile time compiler adds it internally.
- *abstract* modifier [[#Abstract Methods and Overriding]]
- *strictfp* (strict floating point) can be used as modifier with enums
- Enums cannot be extends. But can implement interfaces
- Enums cannot be instantiated. (Outside the enum, inside the enum, inside the constructor etc...)
	 ```java error=6,12
	enum Status {
		ACTIVE, INACTIVE, BLOCKED;
	
		Status() {
			System.out.println("Status()...");
			new Status();
		}
	}
	
	class A {
		public static void main(String[] args) {
			new Status();
		}
	}
	```

##### When Defined Externally
- Only *public* and *default* access modifiers can be used with external enums. (Just like classes)
##### When Defined Internally
- All 4 access modifiers can be used
- static modifier can be used
<br>
- Enum type must not be local
	 ```java error=3
	public class A {
		public static void main(String[] args) {
			enum X { A, B, C }
		}
	}
	```


### Enums vs. Switch
##### Evolution of Switch
1.4 - byte, short, char, int
1.5 - Enum types, All wrapper classes(Byte, Short. Integer...)
1.7 - String

- We can use a enum values as a switch case value. Because value of an enum is a final variable.

### Built in Method
- There's a static method associated with a enum.
	*values* static method of any enum return a array of values in that enum.
	 *values* static method is being generated due to *enum* keyword by the compiler. Not a inherited one.
	 ```java
	enum Status { ACTIVE, INACTIVE, BLOCKED }
	
	public class A {
		public static void main(String[] args) {
			Status[] values = Status.values();	
	
			for (Status status : values) {
				System.out.println(status);
			}
		}
	}
	```
	
- *name* and *ordinal* methods associated with a enum value are inherited from *java.lang.enum* abstract class.
	- *name* method give the value of the enum value
	- *ordinal* method gives position of the enum value in the enum.
		- Position are decided by the order by which enum values are defined in the num.
		- Positions start with 0 index.

 ```java
enum Status {
	ACTIVE(10), INACTIVE(20), BLOCKED(false);

	Status(int i) {
		System.out.println("int");
	}

	Status(boolean b) {
		System.out.println("boolean");
	}
}

 public class Test {
	public static void main(String[] args) {
		Status status = Status.ACTIVE;

		switch(status) {
			case ACTIVE:
				System.out.println("User status is ACTIVE");
				break;
		}

		Status[] values = Status.values();

		for (Status s : values) {
			System.out.println(s.name() + " " + s.ordinal());
		}
	}
}
```


### Constructors
- Constructors can be overloaded. But matching values should pass at the end of every enum
	 ```java
	enum Status {
		ACTIVE(10), INACTIVE, BLOCKED(false);
	
		Status(int i) {
			System.out.println("Status(int i)");
		}
	
		Status(boolean b) {
			System.out.println(Status(boolean b));
		}

		Status() {}
	}
	```

- Use case of a constructor overloading in Enums
	 ```java
	 enum Status {
		 PENDING("Status is pending"),
		 IN_PROGRESS("Status is in progress"),
		 COMPLETED("Status is completed"),
		 FAILED("Status is failed");
	
		private String message;
	
		Status(String message) {
			this.message = message;
		}
	
		public String getMessage() {
			return message;
		}
	 }
	
	public class A {
		public static void main(String[] args) {
			Status status = Status.PENDING;
			System.out.println(status.getMessage());
		}
	}
	```

### Abstract Methods and Overriding
- Abstract methods can be defined in enums. But they need to be defined anonymously at each value
	 ```java
	enum Status {
		PENDING {
			public String getMessage() {
				return "Pending"
			}
		},
		IN_PROGRESS {
			public String getMessage() {
				return "In Progress"
			}
		},
		COMPLETED {
			public String getMessage() {
				return "Completed"
			}
		},
		FAILED {
			public String getMessage() {
				return "Failed"
			}
		};
	
		public abstract String getMessage();
	}
	```

This is only possible in enums. Concrete classed cannot do this.

- Methods can be overridden
	 ```java
	enum Status {
		PENDING {
			public String getMessage() {
				return "Pending"
			}
		},
		IN_PROGRESS,COMPLETED,FAILED;
	
		public String getMessage() {
			return "";
		};
	}
	```

### Operators

```java error=13
enum X { A, B, C, D }

public class A {
	public static void main(String[] args) {
		X letter = X.A;
		System.out.println(letter.equals(X.A));

		// hashCodes are not constants. Not a good method for comparison
		System.out.println(X.A.hashCode() > X.B.hashCode());
		
		System.out.println(X.A.ordinal() > X.B.ordinal());

		System.out.println(X.A > X.B);
	}
}
```

- *equals* used for value comparison
- *equal operator* used for memory location comparison
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