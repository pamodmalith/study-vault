---
parent: 
previous: "[[Day 08]]"
next: "[[Day 10]]"
---
> [!lecture]
> Day 09
 
> [!resources]
> 

#### GridBag Layout
- Components can span multiple rows or columns.
- Cells do not need to be the same size.
- Each component's placement and sizing is controlled by a GridBagConstraints object

![[grid-bag-layout.png]]

![[grid-bag-layout-params.png]]

#### Null Layouts
- Setting a container's layout to null disables layout management, so you must manually set the position and size of every component using setBounds(x, y, width, height)

##### Pros
- Complete control over component placement

##### Cons
- Does not adjust to window resizing or different screen resolutions.
- Not responsive to fond or local changes.
- Maintenance can be difficult for complex UIs.

![[null-layout.png]]

#### Absolute Layout (Provided by NetBeans)
- A layout manager provided by NetBeans (org.netbeans.lib.awstextra.AbsoluteLayout) that mics null layout but lets you specify component positions and sizes as constrains when adding them.
- Difference from Null Layout
	- AbosluteLayout encapsulates position and size in a constraint object, making the code slightly cleaner and reducing the need for separate setBounds calls.

![[absolute-layout.png]]

#### Free Design (NetBeans GUI Builder)
- NetBeans `FreeDesign` mode allows you to visually drag and drop components, with the IDE automatically generating a flexible layout (typically using GroupLayout)
##### Pros
- Easier to design visually.
- The resulting layout is more aduptable than null or absolute layouts.
- Supports automatic alignment and resizing

##### Cons
- sometimes, switching from Free Design to Null Layout can cause components to disappear or behave unexpectedly.

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