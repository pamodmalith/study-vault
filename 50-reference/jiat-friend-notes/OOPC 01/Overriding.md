---
parent: "[[Inheritance]]"
previous: 
next: "[[Overloading]]"
---
* Class properties in parent class can be overridden by properties in the child class. Override is for `non static methods`.

  ```java title=Test.java hl:2,10
	class Parent {
		void car() {
			System.out.println("Minor");
		}
	}
	
	class Child extends Parent {
		// a indicator for the compiler and not necessary to put
		// not effect on output
		@Override
		void car() {
			super.car();
			System.out.println("BMW i8");
		}
	}
	
	class Test {
		public static void main (String[] args) {
			Child child = new Child();
			child.car();
		}
	}
	```

* Variables cannot be overridden.
* To method to be overridden there should be inheritance.
* A method can only be written in sub class, not in same class (Cannot override in the same class scope).

  ```java title=Test.java
	class Parent extends Object {
		void m() {
			System.out.println("Parent m()");
		}
	}
	
	class Child extends Parent {
		void m() {
			System.out.println("Child m()");
		}
	}
	
	public Test {
		public static void main(String[] args) {
			Child child = new Child();
			child.m();
		}
	}
	```

	![[child-stack-heap.png]]

	Always properties of a object finds from sub class to super class. In this case JVM first looks for m() in `child` class. Since it has it JVM stops there. Otherwise it'll look for `m` in super classes

-  To override a method method signatures must be identical

	![[method-signature.png]]

	Return types should be the same or a subtype of the return type declare in the original overridden method in the super class

	 ```java title=Test.java hl=7,15
	class A{}
	
	class B extends A {}
	
	class Parent extends Object {
	    // A is super type
	    A m() {
	        System.out.println("Parent.m()");
	        return null;
	    }
	} 
	
	class Child extends Parent {
	    // B is sub type
	    B m() {
	        System.out.println("Child.m()");
	        return null;
	    }
	}
	  
	public class Test {
	    public static void main(String[] args) {
	        Child c = new Child();
	        c.m();
	    }
	}
	```

	This is wrong since B is the sub class of A

	 ```java title=Test.java error:2,9
	class Parent extends Object {
	    B m() {
	        System.out.println("Parent.m()");
	        return null;
	    }
	}
	
	class Child extends Parent {
	    A m() {
	        System.out.println("Child.m()");
	        return null;
	    }
	}
	```

- Static methods cannot be overridden. But can be redeclared.
- Only methods with access modifiers can be used for overriding. The access lever cannot be more restrictive than the overridden method's access level.

	>[!info] Example
	>If he super class method is declared public then the overridden method in the sub class cannot be either private or protectd.

	- When overriding same or wider access level can be used.
	
	  ```java title=Test.java hl:2,8
		 class Parent extends Object {
		 	void m() {
		 		System.out.println("Parent.m()");
		 	}
		 }
		 
		 class Child extends Parent {
		 	public void m() {
		 		System.out.println("Child.m()");
		 	}
		 }
		 ```

	- But it cannot make narrower.

	  ```java title=Test.java error:2,8
		class Parent extends Object {
			public void m() {
				System.out.println("Parent.m()");
			}
		}
		
		class Child extends Parent {
			void m() {
				System.out.println("Child.m()");
				return null;
			}
		}
		```

	-  Class method with final modifier cannot be overridden

		```java title=Test.java
		class Parent extends Object {
			public static void main(String[] args) {}
			
			final void m() {
				System.out.println("Parent m()");
			}
		}

		class Child extends Parent {
			public void m1() {
				m();
				System.out.println("Child m()");)
			}
		}
		 ```

- Constructors cannot be overridden

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