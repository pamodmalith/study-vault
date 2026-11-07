---
parent: 
previous: "[[Day 10]]"
next: "[[Day 12]]"
---
> [!lecture]
> Day 11
  
> [!resources]
> 

#### JDialog
##### Setting data in JDialog components
When working with JDialog in Java Swing, there are multiple ways to set data in its components.
The most common approaches include using *constructors*, *setter methods*, and public *access variables*.
- Constructors allow you to pass data to the dialog at the time of its creation, making it ideal for initializing fields immediately.
- Setter methos offer more flexibility by letting you update component values after the dialog has been created.
- public variables can be used to access or modify data directly, though this approach is less secure and not recommended for larger or more complex applications.
Choosing the right method depends on the application's design needs, data flow, and maintainability requirements.

![[j dialog constructor-data-passing.png]]


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