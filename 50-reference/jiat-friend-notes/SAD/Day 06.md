---
parent: 
previous: "[[Day 05]]"
next: "[[Day 07]]"
---
> [!lecture]
> Day 06
  
> [!resources]
> 

#### Event Handling - *JComboBox (ActionListener)*

![[j combobox - event handling.png]]

This event called when an item in the combobox is selected

#### *JComboBox (ItemListener)*

![[j combobox - item listener.png]]

Called when item in an combobox being selected and deselected

#### *JComboBox (PopupMenuListener)*

![[j combobox - popup menu listener.png]]

Called when items in combobox is being shown or hide

#### *JCheckBox (ItemListener)*

![[j checkbox - itemlistener.png]]

This gets called when check is clicked whatever the current state is

#### *JCheckBox (ActionListener)*

![[j checkbox - action listener.png]]

This event gets triggered when the check is checked. *isSelected()* method in the checkbox is called to get the state

#### *JRadioButton (ActionListener)*

![[j radiobutton - action listener.png]]

To confirm only one radio button is selected when multiple radios buttons are created, we can use button group.

##### Grouping RadioButton
![[j radibutton - grouping.png]]

#### Event Handling - *Adapters*

There are multiple abstract methods of most interfaces we use in event handling. In such scenario, we need to override all the methods even though don't use them. To resolve this issue, we can use adapter classes.

Below are few examples for adapter classes

| Adapter Class | Listener Interface                                           | Used For                          |
| ------------- | :----------------------------------------------------------- | --------------------------------- |
| MouseAdapter  | MouseListener<br>MouseMotionListener<br>MouseWheelListener   | Mouse  event (click, move scroll) |
| KeyAdapter    | KeyListener                                                  | Keyboard events                   |
| FocusAdapter  | FocusListener                                                | Focus gained/lost                 |
| WindowAdapter | WindowListener<br>WindowFocusListener<br>WindowStateListener | Window events                     |

Main usage of using an adapter class is, we can override the only method we need.

![[event handling - adapters.png]]

### Differences between *Listeners* and *Adapters*


| Feature              | Listener Interface                              | Adapter Class                 |
| :------------------- | :---------------------------------------------- | :---------------------------- |
| Type                 | Interface                                       | Abstract Class                |
| Override all methods | Yes                                             | No (override needed method)   |
| Recommended          | Handling multiple events / handling all events  | Handle specific method        |
| Flexibility          | Less                                            | High                          |
| Examples             | mouseListener, KeyListener, WindowListener, ... | MouseAdapter, KeyAdapter, ... |


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