---
parent: 
previous: "[[Day 07]]"
next: "[[Day 09]]"
---
> [!lecture]
> Day 08
  
> [!resources]
> 

#### Box Layout
- Arranges components in a single row or column.
- Direction specified by
	- BoxLayout.X_AXIS (row, let to right)
	- BoxLayout.Y_AXIS (column, top to bottom)
- Each component gets its preferred width (for columns) or height (for rows); others dimension is adjusted as needed.
- Also supports LINE_AXIS and PAGE_AXIS for more advanced alignment
- Constructor
	- BoxLayout(Container target, int axis)

![[box-layout.png]]

![[box-layout gaps.png]]
#### Card Layout
- Only one component (card) is visible at a time.
- You can switch between cards programmatically.
- Each card is identified by a unique name (string).
- Constructors
	- CardLayout()
	- CardLayout(int hgap, int vgap)
- Switch between cards using CardLayout methods
	- show(Container parent, String name)
	- next(Container parent)
	- previous(Container parent)
	- first(Container parent)
	- last(Container parent)

![[card-layout.png]]
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