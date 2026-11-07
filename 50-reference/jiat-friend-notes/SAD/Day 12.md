---
parent: 
previous: "[[Day 11]]"
next: "[[Day 13]]"
---
> [!lecture]
> dAY 12
 
> [!resources]
> 

![[j dialog getter-setter-data-passing.png]]

- Below method is less secure
![[j dialog public-varaible-data-passing.png]]

##### Retrieving Data from JDialog
To get data from a JDialog in Java Swing, several approaches can be used depending on the design and complexity of the application.
- One common method is to define *getter methods* inside the dialog class, which allows the main program to retrieve user input after the dialog is closed.
- Another option is using *public variables* to directly access data, though this is generally discouraged in favor for encapsulation.
- For more interactive communication, you can use *callback methods or listener interfaces* to notify the parent class when data is ready.

![[j dialog retrieving-data-variables-issue.png]]

- Home is a JFrame (It's because JFrame class is extended by Home class)
- This doesn't mean all JFrame classes are Home classes
-  For this reason, when we create the dialog java raises an error.
	Java cannot decide Home is exactly a JFrame
- To resolve this issue, either we should exactly say Home class is needed in the constructor or we need to cast the JFrame receive to Home.

![[j dialog retrieving-data-variables-fixed.png]]



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