---
parent: 
previous: "[[Has-A Relationship]]"
next: "[[Garbage Collection]]"
---
*Unified Modeling Language (UML)* is a standard way to visualize the design of a software system.
among the many diagrams offered by UML, *class diagrams* are the perhaps the most widely used.
UML class diagram provides a static view of an object-oriented system, showcasing its *classes, attributes, methods, and the relationships among objects*.

### Class
A class is a blueprint or template that defines the properties and behavior of an object.

Represented as  rectangles,  classes are divided into three compartments.
- Name (top) : The unique identifier of the class (e.g. A)
- Attributes (middle) : The properties or data associated with the class (e.g. x, y, z)
- Operations (bottom) : The actions or methods that  can be performed by objects or the class (e.g. m(), m1())

![[uml-structure-overview.svg]]
### Visibility Markers
Visibility markers indicate the accessibility of attributes and methods within a class.
- `+` (Public) : The attribute or method is accessible from any class.
- `-` (Private) : the attribute or method is only accessible within the same class.
- `#` (Protected) : The attribute or method is accessible within the same class and its sub classes.
- `~` (Package) : The attribute or method is accessible within the same package

### Attributes
Attributes in a UML class diagram represent the properties or data fields of a class.
Attributes are typically written in the format:

![[uml-visibility.svg]]
- Name : The name of the attribute
- type : The data type of the attribute
- Default value : (Optional) The initial value of the attribute

![[uml-structure-visibility.svg]]

### Methods
Methods (or operations) in a UML class diagram represent the functions or behaviors that a class can perform.
Methods are typically written in the format:
![[uml-methods.svg]]
- Name : the name of the method
- Parameter List : A comma separated list of parameters, each specified as `name : type`
- Return Type : The data type returned by the  method.

![[uml-structure-methods.svg]]

### Interfaces
An interface defined a contract for classes that implement it. It specifies a set of methods that implementing classes must provide.

Interfaces are depicted as a class rectangle with the keyword `<<interface>>` above the interface name. Methods in interfaces are abstract by nature, so they are usually shown without any implantation details.

![[uml-interface.svg]]

### Abstract Classes
An abstract class is a class that cannot be instantiated (you can't create objects directly from it).

It serves as a blueprint for other classes (subclasses) that inherit from it.

An abstract class in UML is represented with the class name in italics and the keyword `<<abstract>>` above the class name. Abstract methods within the class are also typically show in italics.

![[uml-abstract.svg]]

### Relationships in UML Class Diagrams
In class diagrams, relationships between classes describe how classes are connected or interact with each other within a system. There are several types of relationships in object-oriented modeling, each serving a specific purpose.

There are six main types of relationships between classes, *association*, *aggregation*, *composition*, *inheritance*, *implementation* and *dependency*.

![[uml-relationships.svg]]

#### Association
Association represents a *uses-a* relationship between two classes where one class uses or interacts with the other.

An association represents a bidirectional relationship between two classes. It indicated that instances of one class are connected to instances of another. Associations are typically depicted as solid lines connecting the classes, with optional arrows indicating the relationship's direction.

![[uml-association.svg]]

#### Directed Association
A directed association in a UML class diagram represents a relationship between two classes where to association as a direction, indicating that one class is associated with another in specific way.

In a directed association, an arrowhead is added to the association line to indicate the direction of the relationship. The arrow points from the class that initiates the association to the class that is being targeted or affected by the association.

Directed association are used when the association has a specific flow or directionality, such as indicting which class is responsible for initiating the association or which class has a dependency on another.

![[uml-directred-association.svg]]

#### Aggregation
Aggregation represents a *has-a* relationship where one class (the whole) contains another class (the part), but the contained class can exist independently.

Aggregation is represented by a diamond shape on the side of the whole class. In this kind of relationship, the child class can exist independently of its parent class.

![[uml-aggregation.svg]]

#### Composition
Composition represents **strong** *has-a* relationship where the part cannot exist without the whole. f the whole is destroyed, the parts are also destroyed.

In composition, the part class cannot exist independently of the whole class. Composition is represented by a filled diamond shape on the side of the whole class.

![[uml-composition.svg]]

#### Dependency
Dependency represents a `uses` relationship where a change in one class (the supplier) may affect the other class (the client).

 A dependency exists between two classes when one class relies on another, but relationship is not as strong as an association or inheritance. It represents a more loosely coupled connection between classes. Dependencies are often depicted as a dashed arrow.

![[uml-dependency.svg]]

#### Inheritance (Generalization)
Inheritance (or generalization) represents an **is-a** relationship where one class (subclass) inherits the attributes and methods of another class (superclass).

Inheritance is depicted by a slid line with a closed, hollow arrowhead pointing from the subclass to the superclass.

![[uml-inheritance.svg]]

#### Realization (Implementation)
Realization or implementation represents a relationship between a class and an interface, where the class implements the methods declared in the interface.

Realization is depicted by a dashed line with an open arrowhead pointing from the implementing class to the interface.

![[uml-realization.svg]]



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