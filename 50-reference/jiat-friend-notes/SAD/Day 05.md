---
parent: 
previous: "[[Day 04]]"
next: "[[Day 06]]"
---
> [!lecture]
> Day 05
  
> [!resources]
> 

*Event handling* in Java Swing means, responding the actions done by the application user. For example,
- clicking a button
- moving the move
- typing with the keyboard
- closing a window

Basically, event handling means, building relationship between user actions and program logic.

In event handling, we need to be consider 4 facts

| 1   | Event Source       | Component that generate the event (JButton, JTextfield)                                      | JButton, JTextField, Window          |
| --- | ------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------ |
| 2   | Event Object       | Contains information about the event                                                         | ActionEvent, MouseEvent, KeyEvent... |
| 3   | Event Listener     | Most of time, this is a interface. we write the actions that should be done by the component | Action Listener, Mouse Listener      |
| 4   | Event Registration | Used to connect source and the listener with *addListener()* method                          | button.AddMouseListener()            |

#### Event Handling - *JButton*

![[jbutton - action listener.png]]

![[jbutton - mouse listener.png]]

#### Event Handling - *JTextField*

![[j textfield - event handling.png]]

#### Event Handling - *Window*
![[window - event handling.png]]


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