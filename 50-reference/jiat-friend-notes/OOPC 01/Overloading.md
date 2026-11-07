---
parent: "[[Inheritance]]"
previous: "[[Overriding]]"
next: "[[Polymorphism]]"
---
- If a class has multiple methods having same name but different in parameters, it is known as `Method Overloading`

>[!info] Advantage of overloading
>Method overloading increases the readability of the program

##### Different ways to overload the method
- By changing the number of arguments
- by changing the data type

##### Rules of overloading

- First and foremost, rule to overload a method in Java is to change method signature.
- Method signature is med of numbers of arguments, type of arguments and order of arguments if they are of different types.
- Return type of method is not part of method signature, so just changing the return type will not overload method.

```java title=Test.java
class A {
	void m() {
		System.out.prinln("m()");
	}

	void m(int i)  {
		System.out.println("m(int)");
	}
	
	void m(int i, int x) {
		System.out.prinln("m(int, int)");
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