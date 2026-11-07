---
parent: 
previous: 
next: "[[SAD/Day 02]]"
---
> [!lecture]
> Day 01
  
> [!resources]
> 

### Standalone Application
A standalone application is a application that can be used in a one computer. This type applications can be used without a server or an internet connection. But as of now, due to technological development, this standalone application thing has changes.

### Java SE (Standard Edition) Frameworks
- Java AWT (Abstract Window Toolkit)
- Java Swing
- Java FX

#### Java Swing
In this subject  *Java Swing* framework is used to make desktop application. The main key features of *Java Swing* are
- Platform independent
- Lightweight components
- Highly customizable

```java
import javax.swing.JFrame;
import javax.swing.JButton;

public class A extends JFrame {
	public static void main(String[] args) {
		A a = new A();
		a.setSize(400, 400);
		a.setLocationRelativeTo(null);

		JButton btn1 = new JButton("Submit");
		a.add(btn1);
		a.setVisible(true);
	}
}
```
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