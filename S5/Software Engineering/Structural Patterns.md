
>[!note] Definition
>They focus on how classes and objects are composed to form larger, flexible, and efficient structures.
>- **Goal**: Make identifying relationships between entities simpler.

## Examples
### Pattern #1: Decorator
Imagine you run a coffee shop, with the base `Coffee`. Users have the choice between a number of toppings `Milk`, `Whip`, `Mocha`... but what about multiple at once ?
- **Without Decorator**: The only way for it to work is to create a class for each combination `SimpleCoffee`, `MilkCoffee`, `WhipCoffee`, `MilkAndWhipCoffee`, `MochaAndMilkCoffee`... 
  Leading to *class explosion* (Many classes that have minimal difference in logic).
- **With Decorator**: We can start with a `SimpleCoffee` object, then dynamically create a decorator class for each topping `MilkDecorator`, `WhipDecorator`... which wrap around it.
#### Java Implementation Example
##### Component Interface and Concrete Component
```java
// 1. The Component Interface
public interface Coffee {
    String getDescription();
    double getCost();
}

// 2. The Concrete Component (Base Object)
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 2.00;
    }
}
```

##### Base Decorator
```java
// 3. The Base Decorator (implements Coffee and holds a Coffee reference)
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

>[!note]- Why a Base Decorator is necessary ?
>Imagine the `Coffee` interface now has 5 methods:
>```java
>public interface Coffee {
>    String getDescription();
>    double getCost();
>    int getCalories();
>    String getOriginCountry();
>    boolean isFairTrade();
}
>```
>And imagine you eventually create 10 different toppings (Milk, Whip, Caramel, Vanilla, Cinnamon, Honey, etc.).
>Without the *Base Decorator* each and every method of `Coffee` would need to be implemented in each of the *Concrete Decorators*, even if they only use the `getDescription()` and `getCost()` methods.
>Futhermore, copying the *Base Decorator*'s contructor content into each *Concrete Decorator* is considered boilerplate.
>
> All in all, it is just meant to reduce redundancy, even though it isn't super obvious in this example, so it is closer to a best practice than a necessary bit of code.
##### Concrete Decorators
```java
// 4. Concrete Decorator A: Adding Milk
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 0.50;
    }
}

// 5. Concrete Decorator B: Adding Whip
public class WhipDecorator extends CoffeeDecorator {
    public WhipDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Whip";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 0.75;
    }
}
```

#### Usage in Client Code
You can nest decorators dynamically at runtime to compose any combination of behavior:

```java
public class CoffeeShopApp {
    public static void main(String[] args) {
        // Start with a basic coffee
        Coffee myCoffee = new SimpleCoffee();
        System.out.println(
	        myCoffee.getDescription() + " -> $" + myCoffee.getCost()
	    );
        // Output: Simple Coffee -> $2.0

        // Add milk
        myCoffee = new MilkDecorator(myCoffee);
        
        // Add whip cream on top of the milk coffee
        myCoffee = new WhipDecorator(myCoffee);

        System.out.println(
	        myCoffee.getDescription() + " -> $" + myCoffee.getCost()
	    );
        // Output: Simple Coffee, Milk, Whip -> $3.25
    }
}
```

### Pattern #2: Composite
The **Composite Pattern** is a structural design pattern that lets you compose objects into tree structures to represent *part-whole hierarchies*.
	**Part-whole hierarchy**: A structure where complex objects (wholes) are composed of simpler objects (parts) that can themselves contain further parts. Just like a FileFolderSystem, where a Folder can both be whole to a File and part of another Folder.
#### The Classic Analogy
Think of your computer's file system:
- **Leaf Objects**: Individual files, with a name and a size.
- **Composite Objects**: Folders that can contain files or other folders.
- **The Uniformity**: Both files and folders implement a common interface (e.g. `FileSystemItem` ). When a folder is asked of its size, it automatically delegates the request to all its children and sums them up.
##### Java Implementation Example
###### Imports
```java
import java.util.ArrayList;
import java.util.List;
```
###### Component Interface
```java
// 1. Component Interface
public interface FileSystemComponent {
    void showDetails();
}
```
###### Leaf Class
```java
// 2. Leaf Class (Represents a single file)
public class File implements FileSystemComponent {
    private String name;

    public File(String name) {
        this.name = name;
    }

    @Override
    public void showDetails() {
        System.out.println("   File: " + name);
    }
}
```
###### Composite Class
```java
// 3. Composite Class (Represents a folder/directory)
public class Directory implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> children = new ArrayList<>();

    public Directory(String name) {
        this.name = name;
    }

    public void add(FileSystemComponent component) {
        children.add(component);
    }

    public void remove(FileSystemComponent component) {
        children.remove(component);
    }

    @Override
    public void showDetails() {
        System.out.println("Directory: " + name);
        for (FileSystemComponent component : children) {
            component.showDetails(); // Recursively calls children
        }
    }
}
```

##### Usage in Client Code
Notice how the client code treats `Directory` and `File` through the same `FileSystemComponent` interface:

```java
public class CompositeDemo {
    public static void main(String[] args) {
        // Create individual files
        FileSystemComponent resume = new File("Resume.pdf");
        FileSystemComponent projectReport = new File("Report.docx");
        FileSystemComponent song = new File("Song.mp3");

        // Create directories and add files
        Directory workDocs = new Directory("Work Documents");
        workDocs.add(resume);
        workDocs.add(projectReport);

        Directory root = new Directory("Root Directory");
        root.add(workDocs);
        root.add(song);

        // Display the entire tree structure uniformly
        root.showDetails();
    }
}
```

**Output:**
```
Directory: Root Directory
Directory: Work Documents
   File: Resume.pdf
   File: Report.docx
   File: Song.mp3
```

#### Why use the Composite Pattern ?
1. **Easy to Add New Component Types**: Thanks to polymorphism and the shared component interface, you can easily introduce new kinds of leaves or composites (e.g., a `ZipArchive`) without breaking existing code.