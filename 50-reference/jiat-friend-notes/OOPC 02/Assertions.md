---
parent: 
previous: "[[Enums]]"
next: "[[Exception Handling]]"
---
> [!resources]
> - [[Assertions.pdf]]

- Ensures the correctness of any assumptions which have been done in the program.

> [! Software bug?]
> A software bug is an error, failure or fault in a computer program or system that causes it to produce an incorrect or unexpected result, or to behave in unintended ways.
#### The Debugging Process
In *software development*, debugging involves locating and correcting *code* errors in in a computer program. Debugging is part of the *software testing process* and is an integral part of the entire software development lifecycle. the debugging process starts as soon as code is written and continues in successive stages as code is combined with other units of programming to form a software product. In a large program that has thousands and thousands of lines of code, the debugging process can be made easier by using strategies such as *unit test, code reviews* and *pair programming.*

> [!Assertion]
> Assertion is a statement in java. It can be used to test your assumptions about the program. It provides an effective way to detect and correct programming errors.

![[assert-syntax.svg|300]]
![[assert-mechanism.svg|400]]

```java
public class A {
    public static void main(String[] args) {
        int i = 10;
        assert i == 11;
        System.out.println("Done ");
    }
}
```

- By default assertions are disabled. To enable assertions *-ea* or *enableassertions* switch of java must be used

- Types of assert statements
	- Simple version
	- Augmented version

##### Simple Version
```java
public class A {
    public static void main(String[] args) {
        int i = 10;
        assert i == 11;
        System.out.println("Done ");
    }
}
```

![[simple-asserrtion-error.svg|600]]
##### Augmented Version
![[augmented-assert-syntax.svg|500]]
```java
public class A {
    public static void main(String[] args) {
        int i = 10;
        assert i > 11 : "Here i value should be > 11, but it isn't";
        System.out.println("Done ");
    }
}
```
- Here we get the message next to the colon along with the *AssertionError*

#### Various Possible Runtime Flags
1. *-ea* | *-enableassertions*
		Enable assertions for non-system classes
2. *-da* | *-disableassertions*
		Disable assertions for non-system classes
3. *-esa* | *-enablesystemassertions*
		Enable assertions for system classes
4. *-dsa* | *-disablesystemassertions*
		Disable assertions for system classes


#### Simultaneous Assertions
![[simultaneous-assertions.svg]]

1. To enable assertions in B  class
		java -ea:com.B
2. To enable assertions in both B and D classes
		java -ea:com.B -ea:net.D
3. To enable assertions in every classes of com
		java -ea:com...
4. To enable assertions in every classes of com except B class
		java -ea:com... -da:com.B
5. To enable assertions in every classes of com except net classes
		java -ea:com... -da:net...
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