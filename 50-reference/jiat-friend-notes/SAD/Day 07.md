---
parent:
previous: "[[Day 06]]"
next:
---
> [!lecture]
> Day 07
  
> [!resources]
> 

### Layouts

In Java Swing, items like JFrame, JPanel that hold components are called containers. We use layouts to order components In a container like this. In Swing, there are multiple layout managers that can be used to create these layouts. In those layout managers, multiple mechanisms are present to arrange components.

#### Flow Layout

- Arranges components in a row, let to right, when the row is full starts a new row.
- Each component gets its preferred size.
- Default layout for JPanel
- Alignment options
	- LEFT
	- CENTER
	- RIGHT
	- LEADING
	- TRAILING
- Constructors:
	- FlowLayout()
	- FlowLayout(int align)
	- FlowLayout(int align, int hgap, int vgap)

![[flow-layout.png]]

#### Border Layout
- Deivides the container into five regions:
	- NORTH (PAGE_START)
	- SOUTH (PAGE_END)
	- EAST (LINE_END)
	- WEST (LINE_START)
	- CENTER
- Usage: Only one component can be placed in each region.
- Behavior:
	- NORTH & SOUTH: Take full width, get their preferred height.
	- EAST & WEST: Take full height of the middle area, get their preferred width.
	- CENTER: Takes remaining space
- Constructors
	- BorderLayout()
	- BoderLayout(int hgap, int vgap) (with gaps between components)

![[border-layout.png]]

#### Grid Layout
- Arranges components in a rectangular grid.
- All cells are the same size;  each component fills its cell
- Useful for calculator like layouts.
- Constructors
	- GridLayout()
	- GridLayout(int rows, int cols)
	- GridLayout(int rows, into cols, int hgap, int vgap)

![[grid-layout.png]]




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