---
parent: 
previous: "[[SAD/Day 03]]"
next: "[[Day 05]]"
---
> [!lecture]
> Day 04
  
> [!resources]
> 


| Symbol | Descrption    | Unicode | Escape |
| ------ | ------------- | ------- | ------ |
| √      | square root   | U+221A  | \u222A |
| ²      | superscript 2 | U+00B2  | \u00B2 |
| ³      | superscript 3 | U+00B3  | \u00B3 |
| ⁿ      | superscript n | U+207F  | \u207F |
| ∛      | cube root     | U+221B  | \u221B |
| ±      | plus-minus    | U+00B1  | \u00B1 |

#### MouseListener
![[mouse-click-event.svg]]

#### ActionListener
![[action-listner-event.svg]]

```java
b1.addActionListener(new ActionListener() {
	@Override
	public void actionPerformed(ActionEvent e) {
		Sstem.out.println("Hello");
	}
})

// or

b1.adActionListner(() -> {
	System.out.println("Hello");
})
```

#### Text Field
![[text-field.svg]]
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