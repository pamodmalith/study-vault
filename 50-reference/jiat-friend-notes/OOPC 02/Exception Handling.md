---
parent: 
previous: "[[Assertions]]"
next: "[[Multithreading]]"
---
> [!resources]
>- [[Exception Handling.pdf]]
>- [[Exception New.pdf]]

Exceptions are not predictable. The program continues to execute till it catches up with an exception and program crashes there after.

```java
class A {
	public static void main(String args[]) {
		System.out.println("main method");
	}
}
```


> [!note]
> Watch XXE 01 Recording


![[runtime-stack.svg]]

- After executing the main method line by line, it gets removed from the stack

![[runtime-stack-after-executing-main.svg]]

- When the runtime stack is empty it and the tread attached to it destroys. And also the stack frame that finished executing get destroyed.

```java
class A {
	static void m() {
		System.out.println("m");
	}

	static void m1() {
		System.out.println("m1");
	}

	public static void main(String[] args) {
		m();
	}
}
```

![[code-example-stack-frame.svg]]

```java
class A {
	static void m() {
		System.out.println("m");
		m();
	}

	public static void main(String[] args) {
		m();
	}
}
```

![[code-example-stack-overflow.svg]]

-   When a program has multiples threads running, each thread has it's own runtime stack. If any thread has any stack frame, the JVM continues to run until that last stack frame get executed.


- There are 2 way that a stack frame can get terminated
	1. Normally Termination
	2. Abnormally Termination

#### Normally Termination

```java
class A {
	static void m() {
		System.out.println("m");
		m1();
	}

	static void m1() {
		System.out.println("m");
	}

	public static void main(String[] args) {
		m();
	}
}
```

![[normally-termination.svg]]

#### Abnormally Termination

```java error=10
class A {
	static void m() {
		System.out.println("m");
		m1();
	}

	static void m1() {
		System.out.println("m1");

		int i = 10/0;

		System.out.println("m1")
	}

	public static void main(String[] args) {
		m();
	}
}
```

![[abnormally-termination.svg]]

```java hl=3 error=4
class A {
	public static void main(String[] args) {
		int d = 0;
		int a = 10/d;
	}
}
```

- If an unwanted unexpected event(exception) arise from an statement in method, the regarding method is responsible for informing the JVM about that exception
- The method that rose the exception, creates a object of *Exception* class and throws it to the JVM

- When an exception occurs some methods might have normally terminated and some can be abnormally terminated.

```java hl=4 error=5,17
Class A {
	static void m() {
		System.out.println("m");
		m1();
		m2();
	}

	static void m1() {
		System.out.println("m1");
	}

	static void m2() {
		System.out.println(10/0);
	}

	public static void main(String[] args) {
		m();
	}
}
```

![[abnormally-normally-termination-in-single.svg]]

### Default Exception Handling

1. If an exception is  raised in side any method then that method is responsible to create an *Exception* object with the following information.
	- Name of the exception
	- Description of the exception
	- Location of the exception (Stack Trace)
2. After creating the *Exception* object, the method passes it to the JVM
3. JVM check whether the method contain any exception handling code or not. If method doesn't contain any handling code then JVVM terminated that method abnormally and removes corresponding entry from the stack(runtime)
4. JVM identifies the caller method and checks whether the caller method contain any handling code or nor. If the caller method also does not contain handling code then JVM also abnormally terminates that caller method and removes the corresponding entry from the stack
5. This process will be continues until *main()* method and if *main()* method also doesn't contain any exception handling code then JVM terminates *main()* method also and removes corresponding entry from the stack
6. Then JVM handover the responsibility of exception handling to the default exception handler

### Exception Hierarchy
#### Throwable
![[throwable-hierarchy.svg|100]]
- Throwable acts as a root for exception hierarchy
- Throwable class contains the following two child classes
	- Exception
	- Error

![[throwable-extensions.svg]]

#### Exception
Most of the cases exception are caused by our program and these are recoverable.

Ex. If *FileNotFoundException* occurs then we can use local file and we can continue rest of the program execution normally

#### Error
Most of the case error are not caused by our program these are due to lack of system resources and these are non recoverable.

Ex. If *OutOfMemoryError* occurs being a programmer we can't do anything the pgoram will be terminated abnormally. System Admin or Server Admin is responsible to raise/increase heap memory.

### Checked Vs Unchecked Exceptions
- The exceptions which are checked by the compiler whether programmer handling or not, for smooth execution of the program at runtime, are called checked exceptions.
- the exceptions which are not checked by the compiler whether programmer handling or not, are called unchecked exceptions.

> [!note]
> - *RuntimeException* and its child classes, *Error* and its child classes are *unchecked* and all the remaining are considered as checked exceptions.
> - Whether exception is checked or unchecked compulsory it should occurs at runtime only and there is no change of occurring any exception at compile time.



### Customized Exception Handling by Using *try-catch*
- It is highly recommended to handle exceptions
- In our program the code which may raise exception is called *risky code*, we must place *risky code* inside try block and the corresponding handling code inside catch block

![[try-catch-template.svg]]

#### Control Flow in *try catch*
```java
try {
	statement1;
	statement2;
	statement3;
} catch(X e) {
	statement4;	
}
statement5;
```

- Case 1:
	If there is no exception
	1, 2, 3, 5 normal termination
- Case 2:
	if an exception raised at statement 2 and corresponding catch block matched
	1, 4, 5 normal termination
- Case 3:
	If an exception raised at statement 2 but the corresponding catch block not matched
	1 followed by abnormal termination
- Case 4:
	If an exception raised at statement 4 or statement 5 then it's always abnormal termination of the program

> [!abstract]
> - Within the try block if anywhere an exception raised then rest of the try block won't be executed even though we handled that exception. Hence, we must place/take only risky code inside try block and length of the  try block should be as less as possible.
> - If any statement which raised an exception, and it is not part of the try block then it is always abnormal termination of the program.
> - There may be a chance or raising an exception inside catch and finally finally blocks also in addition to try blocks.

> [!note]
> The most super type we can use for a catch block is*Throwable*. We cannot use *object* or any other normal class.


#### Various Method to Print exception Informationj
Throwable class defines the following methods to print exception information to the console.

- *printStackTrace()*
	This method prints exception information in the following format
	- name of the exception : description of exception
	- stack trace

- *toString()*
	This method prints exception information in the following format
	- name o the exception : description of the exception

- *getMessage()*
	This method returns only description of the exception

> [!info]
> Default exception handler internally uses *printStackTrace()* method to print exception information to the console

#### Try with Multiple Catch Blocks
The way of handling  exception varies from exception to exception. Hence for every exception type it is recommended to take a separate catch block. That is try with multiple catch blocks is possible and recommended to use.

![[try-catch-single.png]]
#### Try with Multiple Catch Blocks
![[multiple-catch.svg]]

---
TODO:
Complete the note XXF 04

---
-  Case 01 : If there is no exception
	 ```java
	class Test {
		public static void main(String[] args) {
			try {
				System.out.println("try block executed");
			} catch (ArithmeticException e) {
				System.out.println("catch block executed");
			} finally {
				System.out.println("finally block executed");
			}
		}
	}
	
	// Output:
	
	// try block executed
	// finally block executed
	```

-  Case 02 : If an exception raised and the corresponding catch block matched
	 ```java
	class Test {
		public static void main(String[] args) {
			try {
				System.out.println("try block executed");
				System.out.println(10/0);
			} catch (ArithmeticException e) {
				System.out.println("catch block executed");
			} finally {
				System.out.println("finally block executed");
			}
		}
	}
	
	// Output:
	
	// try block excuted
	// catch block executed
	// finally block executed
	```

- Case 3 : If an exception raised but the corresponding catch block not matched
	 ```java
	class Test {
		public static void main(String[] args) {
			try {
				System.out.println("try block executed");
				System.out.println(10/0);
			} catch (NullPointerException e) {
				System.out.println("catch block executed");
			} finally {
				System.out.println("finally block executed");
			}
		}
	}
	
	// Output:
	
	// try block executed
	// finally block executed
	// Exception in thread "main" java.ArithmeticException: /by zero
	// at Test.main(Test.java:5)
	```

#### *return* Vs *finally*
Even though return statement present in try or catch blocks fist finally will be executed and after that only return statement will be considered.

i.e. finally block dominates return statement

```java
class Test {
	public static void main(String[] args) {
		try {
			System.out.println("try block executed");
			return;
		} catch (ArithmetiException e) {
			System.out.println("catch block executed");
		} finally {
			System.out.println("finally block executed");
		}
	}
}

// Output:

// try block executed
// finally block executed
```

- If return statement present try, catch and finally blocks then finally block return statement will be considered.
	 ```java
	class Test {
		public static int m1() {
			try {
				System.out.println(10/0);
				return 777;
			} catch (ArithmeticException e){
				return 888;
			} finally {
				return 999;
			}
		}
	
		public static void main String[] args) {
			System.out.println(m1());
		}
	}
	
	// Output:
	
	// 999
	```

#### *finally* Vs *System.exit(0)*
There is only one situation where the finally block won't be executed is whenever we are using *System.out(0)* method

Whenever we are using *system.exit(0)* then JVM itself will be shutdown, in this case finally block won't be executed.

```java
class Test {
	public static void main(String[] args) {
		try {
			System.out.println("try");
			System.exit(0);
		} catch (ArithmeticException e) {
			System.out.println("catch block executed");
		} finally {
			System.out.println("finally block execute");
		}
	}
}

// Output:

// try
```

> [!Note]
> *System.exit(0);*
> 1. This argument acts as status code. Instead of zero  we can take any integer value
> 2. *zero* mean normal termination, non-zero means abnormal termination
> 3. This status code in internally used by JBM, whether it is zero or non-zero there in non change in the result

#### Difference between *final*, *finally*, and *finalize*
- *final*
	- *final* is the modifier applicable for classes, method and variables
	- If a class declared as the *final* then child class creatin is not possible
	- If a method is declared as the *final* then overriding of that method is no t possible
	- If a variable declared as the *final* then reassignment is not possible
- *finally*
	- *finally is the block always associated with *try-catch* to maintain clean up code which should be executed always irrespective of whether exception raised or not raised and whether handled or not handled
- *finalize*
	- *finalize* is a method, always invoked by *Garbage Collector* just before destroying an object to perform cleanup activities

> [!note]
> - *finally* block meant  for cleanup activities related to try block where as *finalize()* method meant for cleanup activities related to object
> - To maintain cleanup code *finally* block is recommended over *finalize()* method because we can't expect exact behavior of *Garbage Collector*

#### Control Flow in *try Catch finally*

```java
try {
	statement1;
	statement2;
	statement3;
} catch(Exception e) {
	statement4;	
} finally {
	statement5;
}
statement6;
```

- Case 1
	If there is not exception
	1, 2, 3, 5, 6 normal termination

- Case 2
	if an exception is raised at statement 2 and corresponding catch block matched
	1, 4, 5, 6 normal termination

- Case 3
	If an exception is raised at statement 2 and corresponding catch block is not matched
	1, 5 abnormal termination

- Case 4
	If an exception raised at statement 4 then it's always abnormal termination, but before, the *finally* block will be executed

- Case 5
	If an exception raised at statement 5 or statement 6 its always abnormal termination

### Nested *try catch*

```java
try {
  statement1;
  
  try {
     statement2;
  } catch (X e) {
    statement3;
  }
  
   statement4;
} catch (Y e) {
  statement5;
} finally {
  statement6;
}
```

*Nested try-catch* is a way to handle errors in code by placing gone *try-catch* block inside another

This allows  you manage different types of errors at different levels

- if an error happens in the inner try block, it can be caught and handled in the inner catch block
- If that catch block rethrows the error, it  can be caught in the outer catch block

Both inner and outer *try-catch* blocks can also have *finally* block, which run cleanup code no matter what happens

While nested *try-catch* blocks can make error handling better, using too many can make the code harder to read, so it's important to use them wisely. Then are helpful when different parts of the code might cause different errors that needs specific handling

#### Control Flow in *nested Try Catch finally*
```java
try {
	statement1;
	statement2;
	statement3;
	try {
		statement4;
		statement5;
		statement6;
	} catch (X e) {
		statement7;
	} finally {
		statment8;
	}
	statement9;
} catch (Y e) {
	statement10;
} finally {
	statment11;
}
statement12;
```

- Case 1:
	If there is no exception, 1, 2, 3, 4, 5, 6, 8, 9, 11, 12, normal termination
- Case 2:
	If an exception raised at statement 2 and corresponding catch block matched 1, 10, 11, 12, normal termination.
- Case 3:
	If an exception raised at statement 2 and corresponding catch block is not matched 1, 11, abnormal termination
- Case 4:
	If an exception raised at statement 5, and corresponding inner catch has matched 1, 2, 3, 4, 7, 8, 9, 11, 12, normal termination
- Case 5:
	If an exception raised at statement 5 and inner catch has not matched but outer catch block has matched, 1, 2, 3, 4, 8, 11, 12, normal termination
- Case 6:
	If an exception raised at statement 5 and both inner and outer catch blocks are not matched, 1, 2, 3, 4, 8, 11, abnormal termination
- Case 7:
	If an exception raised at statement 7 and the corresponding catch block matched 1, 2, 3, 4, 5, 8, 10, 11, 12, normal termination
- Case 8:
	If an exception raised at statement 7 and the corresponding catch block not matched 1, 2, 3, 4, 5, 8, 11, abnormal termination
- Case 9:
	If an exception raised at statement 8 and the corresponding catch block has matched 1, 2, 3, 4, 5, 7, 10 , 11, 12, normal termination
- Case 10:
	If an exception raised at statement 8 and the corresponding catch block not matched 1, 2, 3, 4, 5, 7, 11, abnormal termination
- Case 11:
	If an exception raised at statement 9 and corresponding catch block matched 1, 2, 3, 4, 5, 6, 8, 10, 11, 12, normal termination
- Case 12:
	If an exception raised at statement 9 and corresponding catch block not matched 1, 2, 3, 4, 5, 6, 8, 11, abnormal termination
- Case 13:
	If an exception raised at statement 10 is always abnormal termination but before that finally block 11 will be executed
- Case 14:
	If an exception raised at statement 11 or 12 is always abnormal termination


```java
class Test {
	public static void main(String[] args) {
		try {
			System.out.println(10/0);
		} catch (ArithmeticException e) {
			System.out.println(10/0);;
		} finally {
			String s = null;
			System.out.println(s.length());
		}
	}
}
```

> [!note]
> Default exception handler can only only one exception at a time and that is the most recently raised exception

#### Various Possible Combinations of *try-catch-finally*
> [!abstract]
> 1. Whenever we are writing try block compulsory, we should write either catch or finally
> 	**i.e.** try without catch o finally is invalid
> 2. Whenever we are writing catch block compulsory we should write try
> 	**i.e.** catch without try is invalid
> 3. Whenever we are writing finally block compulsory we should write try.
> 	**i.e.** finally without try is invalid
> 4. In try-catch-finally order is important.
> 	- exceptions should be in the order of parent child parent 
> 		**i.e.** ArithmeticException then Exception
> 1. Within the try-catch-finally blocks we can take try-catch-finally.
> 	**i.e.** nesting of try-catch-finally is possible
> 6. for try-catch-finally blocks curly braces are mandatory.


![[try-catch-fianlly-combinations.svg]]

### *throw* Statement

Sometimes we can create Exception object explicitly and we can hand over to the JVM manually by using **throw** keyword.

![[throw-syntax.svg]]

```java implicit-exception.java
class Test {
	public static void main(String[] args) {
		System.out.println(10/0);
	}
}

// In thi scase creation of AirthmeticException object
// and handover to the JVM will be performed automatically
// the main() method
```

```java explicit-exception.javva
class Test {
	public static void main(String[] args) {
		throw new ArithmeticException("/by zero");
	}
}

// In this case we are creating exception object explictly
// and handover to the JVM manually
```

> [!note]
> In general we can use **throw** keyword for customized exception but not for predefined exceptions.
> 
> ```java
> class Test {
> 	static ArithmeticException e = new ArithmeticException();
> 	public static void main(String[] args) {
> 		throw e;
> 	}
> }
> ```
> ```java error=4
> class Test {
> 	static ArithmeticException e;
> 	public static void main(String[] args) {
> 		throw e;
> 	}
> }
> 
> // Here we get a NullPointerException
> // Because we cannot throw a null pointer
> // (default value of e is null, because it's an object)
> ```

- After **throw** statement we can't take any statement directly otherwise we will get compile time error saying unreachable statement.

	```java hl=4
	class Test {
		public static void main(String[] args) {
			System.out.println(10/0);
			System.out.println("hello");
		}
	}
	```

	 ```java error=4
	class Test {
		public static void main(String[] args) {
			throw new ArithmticException("/by zero");
			System.out.println("hello");  // unreachable statement
		}
	}
	```

- We can use throw keyword only for *Throwable* types otherwise we will get compile time error saying incompatible types
	 ```java error=3
	class Test {
		public static void main(String[] args) {
			throw new Test();
		}
	}
	
	// Here Test class is not a Throwable. Only Throwables can be thrown.
	```

	 ```java hl=3
	class Test extends RuntimeException {
		public static void main(String[] args) {
			throw new Test();
		}
	}
	
	// This code works because RuntimeException is a extention of Throwable
	```

### *throws* Statement

- Hence the main objective of *throws* keyword is to delegate the responsibility of exception handling to the caller method.
- *throws* keyword required only for [[#Checked Vs Unchecked Exceptions|checked]] exceptions. There is no use for using *throws* for unchecked exceptions.
- *throws* keyword required only to convince compiler. Usage of throws keywords doesn't prevent abnormal termination of the program.

> [!note]
> We can keep forwarding exception until it get's to the JVM. But it won't get away from abnormally terminating the program. By only using try-catch, we can stop the program being a terminated.

- Compiler checks for *throws* and *throw* at the compile time as 2 keywords relating to exceptions.
- We can only throw same same and subtype exceptions through *throws*
	 ```java error=2,3
	public class Test {
		static void m() throws Exception {
			throw new Throwable("New exception");
			// hear Exception is a subtype of Throwable
		}
	
		public static void main(String[] args) {
			try {
				m()
			} catch (Exception e) {
				System.out.println("Catched");
			}
		}
	}
	```

	But This won't apply to runtime exceptions as they are not checked by the compiler. Compiler only checks for *checked exceptions* and *runtime exceptions* are unchecked.

	 ```java hl=2,3
	public class Test {
		static void m() throws ArithmeticException {
			throw new RuntimeException("Throwable");
		}
	
		public static void main(String[] args) {
			try {
				m();
			} catch (Exception e) {
				System.out.println("Catched");
			}
		}
	}
	```

## 1.7 Version Enhancements
### *try* With Resources

![[try-with-resources-syntax.svg]]

```java
class Test {
	publi static void main(String[] args) throws IOExcepton {
		BufferedReader br = null;
		try {
			br = new BufferedReader(new FileReader("abc.txt"));
			String line = br.readLine();

			while ((line != null) && (!line.isEmpty())) {
				System.out.println(line);
				line = br.readLine();
			}
		} catch (IOException e) {
			e.getStackTrace();
		} finally {
			if (br != null) {
				br.close();
			}
		}
	}
}
```

problems in this approach:
- Programmer is required to close all opened resources with increases the complexity of the programming.
- It is compulsory that we should write finally block explicitly which increases the length of the code and readability readability.

To overcome these problems Oracle People introduced *try with resources* in **1.7** version

The main advantage of *try with resources* is the resources which are opened as part of try block will be closed automatically.
Once the control reaches end of the try block either normally or abnormally and hense we are not required to close explicitly so that complexity of programming will be reduces. It is not required to write finally block explicitly and hence length of the code will be reduced and readability will be improved
Any object that implements *java.lang.AutoCloseable*, which includes all objects which implement *java.io.Closeable*, can be used as a resource.


### Multi Catch Block
Until 1.6 version, even though Multiple Exceptions having same handling code we had to write  a separate catch block for every exceptions. it increased num of code lines and reviewers readability.

```java
try { ... }
catch (ArithmeticException e) {
	e.printStackTrace();
} catch (NullPointerException e) {
	e.printStackTrace();
} catch (lassCastException e) {
	System.out.println(e.getMessage());
} catch (IOException e) {
	System.out.println(e.getMessage());
}
```

To overcome this problem Oracle introduces *multi catch block* concept in 1.7 version.

The main advantage of multiple catch block is we can write a single catch bloc, which can handle multiple different exceptions.

```java
try { ... }
catch (ArithmeticException | NullPointerException e) {
	e.printStackTrace();
} catch (ClassCastException | IOException e) {
	e.printStackTrace();
}
```

In multi catch block, there should  not be any relation between *Exception* types (either child to parent / parent to child / or same type, otherwise we will get compile time error )

![[multiple-catch-error.svg]]


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