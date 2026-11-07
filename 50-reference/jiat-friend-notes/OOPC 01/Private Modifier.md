---
parent: 
previous: "[[_OOPC 01]]"
next: "[[Inheritance]]"
---

- Private properties cannot be accessed in inherited classes

	 ```java {label='member-1'}
	class Member {
		private String id;
		String name;
		String email;
	
		public static void main(String[] args) {}
		
		// ...
	}
	```

- Private properties cannot be used outside of the scope.

	 ```java title=Test.java hl:5 {import=['member-1']}
	class Student extends Member{
		public static void main(String[] args) {}
		
		void study(){
			System.out.println(id + " is studying.");
			System.out.println(name + " is studying.");
		}
		
		// ...
	}
	
	public class Test {
		public static void main(String[] args) {
			Student student_1 = new Student();
			student_1.study();
			System.out.println("Hello World!");
		}
	}
	```

- Private properties gets loaded into instances, but cannot be accessed.

	 ```java hl=5 {import=['member-1']}
	public class Test {
		public static void main(String[] args) {
			Member member_1 = new Member();
			
			String id = member_1.id;
			// this cannot be done since we are accessing
			// id outside of Member class scope
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