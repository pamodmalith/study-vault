---
parent: 
previous: "[[Day 09]]"
next: "[[Day 11]]"
---
> [!lecture]
> Day 10
  
> [!resources]
> 

### Swing Interfaces
Swing is a part of Java used to create windows and screens in desktop applications. It provides ready-made tools like buttons, menus, and dialogs to make program look and work better. Swing is easy to use and helps us build user-friendly interfaces. It is more powerful than the older AWS (Abstract Windows Toolkit) system and gives a better look to our applications.

#### JFrame
JFrame is a basic window in Java Swing that is used to create the main screen of a desktop application.

![[JFrame.png]]

#### JDialog

JDialog is Swing component used to create small windows that pop up over a main window, usually to show messages, get input, or confirm actions. Unlike JFrame, which is used for the main application window, JDialog is typically used for short interactions and can e set as a modal, which means it block the main window until the user closes the dialog. It is useful for things like settings windows, warnings, or custom input forms.

![[SAD/resources/JDialog.png]]

- `java.swt.Frame parent` - Passes the who through the dialog should open
- `boolean model` - defines if we allow to do actions in the parent.
	- true : allows to do actions
	- false: doesn't' allow to do actions

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