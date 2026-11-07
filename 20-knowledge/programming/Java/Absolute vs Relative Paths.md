### 1. Absolute Path (starts with `/`)
```java
getClass().getResource("/lk/jiat/smarttrade/img/store.png")
```

- **Behavior**: Starts from the root of the classpath    
- **Looks in**: `[classpath_root]/lk/jiat/smarttrade/img/store.png`    
- **Equivalent to**: `ClassLoader.getResource("lk/jiat/smarttrade/img/store.png")`    

### 2. Relative Path (no leading `/`)
```java
getClass().getResource("../img/store.png")
```

- **Behavior**: Relative to the current class's package    
- **How it works**:    
    1. First determines the package path of the current class        
        - If your class is `lk.jiat.smarttrade.gui.MyClass
        - Base path becomes `lk/jiat/smarttrade/gui/`            
    2. Applies the relative path:        
        - `../` moves up one package level (to `lk/jiat/smarttrade/`)            
        - Then looks in `img/store.png`            
    3. Final path: `lk/jiat/smarttrade/img/store.png`        

## Detailed Example

Given this project structure:
text
```
src/
├── lk/
│   └── jiat/
│       └── smarttrade/
│           ├── gui/
│           │   └── MyClass.java
│           └── img/
│               └── store.png
```
And this class:
```java
package lk.jiat.smarttrade.gui;

public class MyClass {
    public void loadImage() {
        // Absolute path
        URL absPath = 
		        getClass().getResource("/lk/jiat/smarttrade/img/store.png");
        
        // Relative path
        URL relPath = getClass().getResource("../img/store.png");
    }
}
```

Both approaches will find the same file, but resolve it differently:

1. **Absolute path resolution**:    
    - Starts from root → `lk/jiat/smarttrade/img/store.png`        
2. **Relative path resolution**:    
    - Class package: `lk/jiat/smarttrade/gui/`        
    - `../` moves to `lk/jiat/smarttrade/`        
    - Appends `img/store.png`        
    - Final: `lk/jiat/smarttrade/img/store.png`        

## Key Differences

|Characteristic|Absolute Path|Relative Path|
|---|---|---|
|**Starts with**|`/`|No leading `/`|
|**Base location**|Classpath root|Current class's package|
|**Path navigation**|No `..` allowed|Can use `..` to go up directories|
|**Portability**|More portable|Tied to class location|
|**Refactoring**|Safe if package moves|Breaks if class moves packages|

## When to Use Each

**Use absolute paths when:**
- The resource has a fixed location    
- You might move the calling class to different packages    
- You want to be explicit about the resource location    

**Use relative paths when:**
- The resource is package-relative    
- You want to maintain resource/class co-location    
- You're working with tightly-coupled resources    

## Special Notes About `../`

1. You can chain them:    
    - `../../img` goes up two package levels        
2. They must be at the start of the path    
    - `img/../store.png` won't work as expected        
3. Don't go above the classpath root:    
    - `../../../../resource` may fail if it escapes the root        

## Debugging Tip

To see exactly where Java is looking:
```java
System.out.println("Absolute: " + getClass().getResource("/lk/jiat/smarttrade/img/store.png"));
System.out.println("Relative: " + getClass().getResource("../img/store.png"));
```

This will show you the actual resolved file URLs, helping verify your path logic.