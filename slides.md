---
# try also 'default' to start simple
theme: dracula
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# https://miro.medium.com/v2/resize:fit:1920/1*XOMTPWTpDLypkp079p9XXg.png
background: ''
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS (experimental)
css: unocss
---


# SOLID Design Principle

Build clean code and best practices

<div class="pt-10">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>


<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: image
image: https://miro.medium.com/v2/resize:fit:1920/1*XOMTPWTpDLypkp079p9XXg.png
---

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #FFFFFF;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Single Responsibility Principles (SRP)

Each class should have only a purpose and not be filled with excessive functionality.

<div grid="~ cols-2 gap-2" m="t-2">

 * Shape class, the violation of the (SRP) 
 * The violation occurs because this class has more than one reason to change
 * changes to either the calculation logic or the display logic would impact the same class.

 * which combines the responsibilities of calculating the area + displaying the area.

```java {all|16-20}
public class Shape {
    private String type;
    private double area;

    public Shape(String type) {
        this.type = type;
    }
    private void calculateArea() {
        // Logic for calculating the area based on the type      
    }
    private void displayArea() {
        // Logic for displaying the area
    }
    // Other methods related to the shape might be here...

    // Violating SRP: 
    // Responsibility for both calculating and displayingarea
    public void calculateAndDisplayArea() {
        calculateArea();
        displayArea();
    }
}
```
</div>

---

# Single Responsibility Principles - Solution


<div grid="~ cols-2 gap-2" m="-t-2">

```java {all}
  public class Shape {
    private String type;
    public Shape(String type) {
        this.type = type;
    }
    public String getType() {
        return type;
    }
  }
```
```java {all}
  public class AreaCalculator {

    // Responsible for calculating the area of a shape
    public double calculateArea(Shape shape) {
        
    }
  }
```
```java {all}
  public class Main {
    public static void main(String[] args) {
        Shape circle = new Shape("circle");

        AreaCalculator calculator = new AreaCalculator();
        double area = calculator.calculateArea(circle);

        AreaPrinter printer = new AreaPrinter();
        printer.displayArea(circle, area);
    }
  }
```
```java {all}
  public class AreaPrinter {
    // Responsible for displaying the area of a shape
    public void displayArea(Shape shape, double area) {
        System.out.println("Area of shape");
    }
  }
```
</div>


---

# Single Responsibility Principles

* Class Calculator that currently handles both basic arithmetic operations and logging. 
* This violates the Single Responsibility Principle. We'll refactor it into separate classes to adhere to SRP.

<div class="grid grid-cols-1 gap-2">

```java {all|3|8|13-16}
  public class Calculator {

    public int add(int a, int b) {
        int result = a + b;
        logOperation("Addition", a, b, result);
        return result;
    }
    public int subtract(int a, int b) {
        int result = a - b;
        logOperation("Subtraction", a, b, result);
        return result;
    }
    private void logOperation(String operation, int a, int b, int result) {
        System.out.println(operation + ": " + a + " - " + b + " = " + result);
        // Simulated logging operation
    }
}
```
</div>

---

# Single Responsibility Principles - Solution

* The Calculator class is responsible for basic arithmetic operations.
* The CalculatorLogger class is responsible for logging operations.


<div class="grid grid-cols-2 gap-2">
  <div class="grid grid-cols-1 gap-2">

  ```java {all}
    public class CalculatorLogger {
      
      public void logOperation(
        String operation, int a, int b, int result) {
          System.out.println(
            operation + ": " + a + " - " + b + " = " + result);
      }
    }
  ```
  ```java {all}
    public class Calculator {

      public int add(int a, int b) {
          return a + b;
      }

      public int subtract(int a, int b) {
          return a - b;
      }
    }
  ```
  </div>

  <div>

  ```java {all}
  public class Main {

    public static void main(String[] args) {

        Calculator calculator = new Calculator();
        CalculatorLogger logger = new CalculatorLogger();

        int resultAdd = calculator.add(5, 3);
        logger.logOperation("Addition", 5, 3, resultAdd);

        int resultSubtract = calculator.subtract(8, 2);
        logger.logOperation("Subtraction", 8, 2, resultSubtract);
    }
  }
  ```
  </div>
</div>



---

# Open Close Principles (OCP)

Class should be ***open*** for extension, but ***closed*** for modification.

<div class="grid grid-cols-2 gap-2">
  <div class="grid grid-cols-1 gap-2">

  The Shape interface represents a geometric shape with a draw method.
  The Circle, Rectangle classes implement the Shape interface.

  * The ShapeHandler class has a method drawShape checking for each shape type and adding its handling logic within the existing method.

  ```java {all}    
    public class ShapeHandler {
        public void drawShape(String shapeType) {
            if (shapeType.equals("Circle")) {
                new Circle().draw();
            } else if (shapeType.equals("Rectangle")) {
                new Rectangle().draw();
            }
        }
    }
  ```
  </div>

  <div class="grid grid-cols-1 gap-2">

  ```java {all}
  // Shape interface representing a geometric shape
  public interface Shape {
      void draw();
  }
  ```
  ```java {all}
  public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Circle");
    }
  }
  ```
  ```java {all}
  // Rectangle class implementing the Shape interface
  public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
  }
  ```
  
  </div>
</div>

---

# Open Close Principles - Issue

* Add new required shape: Triangle

<div grid="~ cols-2 gap-2" m="t-2">

  ```java {all}
    // Triangle class implementing the Shape interface
    public class Triangle implements Shape {

        @Override
        public void draw() {
            System.out.println("Drawing Triangle");
        }
    }
  ```

  ```java {all|9-12}
    // ShapeHandler class with a violation of the Open/Closed Principle
    public class ShapeHandler {
      public void drawShape(String shapeType) {
          if (shapeType.equals("Circle")) {
              new Circle().draw();
          } else if (shapeType.equals("Rectangle")) {
              new Rectangle().draw();
          }
          // Violation: Adding a new shape requires modifying existing code
          else if (shapeType.equals("Triangle")) {
              new Triangle().draw();
          }
      }
    }
  ```

</div>

---

# Open Close Principles - Solution

* The Open/Closed Principle, allowing you to extend the system without modifying existing code.

The CircleCreator, RectangleCreator, TriangleCreator classes implement the ShapeCreator interface.

<div class="grid grid-cols-2 gap-2">

```java {all}
  // ShapeCreator interface for creating shapes
  public interface ShapeCreator {
      Shape createShape();
  }
```
```java {all|2}
  // TriangleCreator class implementing ShapeCreator for creating Triangle instances
  public class TriangleCreator implements ShapeCreator {
      @Override
      public Shape createShape() {
          return new Triangle();
      }
  }
```
```java {all|2}
  // RectangleCreator class implementing ShapeCreator for creating Circle instances
  public class RectangleCreator implements ShapeCreator {
      @Override
      public Shape createShape() {
          return new Rectangle();
      }
  }
```
```java {all|2}
  // CircleCreator class implementing ShapeCreator for creating Circle instances
  public class CircleCreator implements ShapeCreator {
      @Override
      public Shape createShape() {
          return new Circle();
      }
  }
```
</div>

---

# Open Close Principles - Solution

<div class="grid grid-cols-2 gap-2">

```java {all}
  // Updated ShapeHandler class using factories
  public class ShapeHandler {
      public void drawShape(ShapeCreator shapeCreator) {
          Shape shape = shapeCreator.createShape();
          shape.draw();
      }
  }
```


```java {all}
  public class Main {
    public static void main(String[] args) {
        ShapeHandler shapeHandler = new ShapeHandler();

        // Drawing a Circle
        shapeHandler.drawShape(new CircleCreator());

        // Drawing a Rectangle
        shapeHandler.drawShape(new RectangleCreator());

        // Drawing a Triangle
        shapeHandler.drawShape(new TriangleCreator());
    }
  }
```

* The CircleCreator, RectangleCreator, TriangleCreator classes are use to create instances of shapes without modifying the ShapeHandler class.

</div>
---

# Liskov Substiution Principle (LSP)

* Ability to replace any instance of a parent class with an instance of child class without negative effects

<div class="grid grid-cols-2 gap-2">
  <div class="grid grid-cols-1 gap-2">

  ```java {all}
  // Base Class
  abstract class Shape {
      public abstract double calculateArea();
  }
  ```

  ```java {all}
  // Child Class
  class Rectangle extends Shape {
      protected double width;
      protected double height;

      public Rectangle(double width, double height) {
          this.width = width;
          this.height = height;
      }

      @Override
      public double calculateArea() {
          return width * height;
      }
  }
  ```
  </div>

  <div>

  ```java {all}
  // Child Class
  class Square extends Shape {
      private double side;

      public Square(double side) {
          this.side = side;
      }

      @Override
      public double calculateArea() {
          return side * side;
      }
  }
  ```
  </div>
</div>

---

# Liskov Substiution Principle - Issue

<div class="grid grid-cols-2 gap-2">

```java {10-14}
  // Shape base class
  class Square extends Shape {
    private double side;

    public Square(double side) { this.side = side; }

    @Override
    public double calculateArea() { return side * side; }
    
    // modifies the side length
    // violate Liskov principle
    public void setSide(double side) {
        this.side = side;
    }
  }
```
```java {2-3|5-6|9-14}
  public static void main(String[] args) {
    Shape rectangle = new Rectangle(5, 10);
    Shape square = new Square(5);

    // Assume we can set the side length of a square
    ((Square) square).setSide(10); // Violates LSP

    // Outputs: "Rectangle area: 50"
    System.out.println("Rectangle area: " + 
    rectangle.calculateArea()); 

    // Outputs: "Square area: 100" (Unexpected)
    System.out.println("Square area: " + 
    square.calculateArea());       
  }
```
</div>

* We're directly violating LSP by modifying the behavior of a Square object in a way that contradicts its expected behavior.
* By allowing the Square object to change its side length after instantiation

---

# Liskov Substiution Principle: Solution

<div class="grid grid-cols-2 gap-2">

```java {9-12}
  class Square extends Shape {
    private double side;

    public Square(double side) { this.side = side; }

    @Override
    public double calculateArea() { return side * side; }
    
    // Removing the setSide method to prevent direct 
    // modification of the side length
    // This ensures that the essential characteristics 
    // of a Square are maintained
  }
```
```java {5-7|9-16}
  public static void main(String[] args) {
    Shape rectangle = new Rectangle(5, 10);
    Shape square = new Square(5);

    // Attempting to modify the side length of a square
    // This line is now commented out to prevent the violation
    // ((Square) square).setSide(10); 

    // Outputs: "Rectangle area: 50"
    System.out.println("Rectangle area: " + 
      rectangle.calculateArea()); 

    // Outputs: "Square area: 25" (Expected)
    System.out.println("Square area: " + 
      square.calculateArea());       
  }
```

</div>

---

# Liskov Substiution Principle: Example #2

<div class="grid grid-cols-2 gap-2">

  <div class="grid grid-cols-1 gap-2">

  ```java {all}
    class Person {
      protected String name;

      public Person(String name) { 
        this.name = name; 
      }
      
      public String getName() { return name; }
    }
  ```
  
  </div>

```java {all}
  class Teacher extends Person {
    protected String subject;

    public Teacher(String name, String subject) {
        super(name);
        this.subject = subject;
    }
    public String getSubject() { return subject; }
}
```

```java {all}
  class Student extends Person {
    protected int studentId;

    public Student(String name, int studentId) {
        super(name);
        this.studentId = studentId;
    }
    public int getStudentId() { return studentId; } 
  }
```

```java {all}
  class Course {
      private String courseName;

      public Course(String courseName) {
          this.courseName = courseName;
      }

      public void enroll(Person person) {
          System.out.println(
            person.getName() + " enrolled in " + courseName);
      }
  }
```

</div>

---

# Liskov Substiution Principle: Issue

<div class="grid grid-cols-1 gap-2">

```java {all|8-9}
  public class Main {
    public static void main(String[] args) {
        Course mathCourse = new Course("Mathematics");

        Student student = new Student("Alice", 123);
        Teacher teacher = new Teacher("Bob", "Math");

        mathCourse.enroll(student); // Outputs: Alice enrolled in Mathematics
        mathCourse.enroll(teacher); // Outputs: Bob enrolled in Mathematics (Violation)
    }
  }
```

</div>

*  when enrolling a teacher in a course, the course treats the teacher just like any other person and prints out the teacher's name. 
* Enrolling a teacher in a course doesn't match enrolling a student, breaking the Liskov Substitution Principle.

---

# Liskov Substiution Principle: Solution

<div class="grid grid-cols-2 gap-2">

```java {all|8-10}
  class Course {
    private String courseName;

    public Course(String courseName) {
        this.courseName = courseName;
    }

    public void enroll(Student student) {
        System.out.println(
          student.getName() + " enrolled in " + courseName);
    }
  }
```

```java {all|7-9}
  public class Main {

    public static void main(String[] args) {
        Course mathCourse = new Course("Mathematics");

        Student student = new Student("Alice", 123);
        Teacher teacher = new Teacher("Bob", "Math");

        // Outputs: Alice enrolled in Mathematics
        mathCourse.enroll(student);
    }
  }
```

</div>

* By modifying the enroll method to accept only **Student** objects.
* This modification ensures only students can be enrolled, preventing any attempts to enroll teachers and maintaining the specified design.

---

# Interface Segregation Principle (ISP)


<div class="grid grid-cols-2 gap-2">

  <div class="grid grid-cols-1 gap-2">

  * Interface should not **force** classes to implement what they can't do.
  * Large interfaces should be devide into small ones.
  
  </div>

```java {all}
  interface Shape {
    double calculateArea();
    double calculateVolume();
  }
```

```java {all|1-5}
  class Circle implements Shape {
    private double radius;
    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }

    @Override
    public double calculateVolume() {
      // "Circle does not have volume.
      throw new UnsupportedOperationException();
    }
  }
```

```java {all}
  class Cube implements Shape {
    private double sideLength;

    public Cube(double sideLength) {
        this.sideLength = sideLength;
    }

    @Override
    public double calculateArea() {
        return 6 * sideLength * sideLength;
    }

    @Override
    public double calculateVolume() {
        return sideLength * sideLength * sideLength;
    }
  }
```

</div>

---

# Interface Segregation Principle: Solution

<div class="grid grid-cols-2 gap-2">

```java {all}
  interface AreaCalculable {
    double calculateArea();
  }
```

```java {all}
  interface VolumeCalculable {
    double calculateVolume();
  }
```

```java {all}
 class Cube implements AreaCalculable, VolumeCalculable {
    private double sideLength;

    public Cube(double sideLength) {
        this.sideLength = sideLength;
    }

    @Override
    public double calculateArea() {
        return 6 * sideLength * sideLength;
    }

    @Override
    public double calculateVolume() {
        return sideLength * sideLength * sideLength;
    }
  }
```

```java {all}
  class Circle implements AreaCalculable {
    private double radius;
    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
  }
```


</div>

---

# Dependency Inversion Principle (DIP)

* Components/Classes should depend on absctraction, not on concretion
* This promotes flexibility, easier maintenance, and reduces coupling in your code.

Suppose you have a User class that needs to send notifications to users. Initially

<div class="grid grid-cols-2 gap-2">

```java {all}
public class User {
    public void sendNotification(String message) {
        EmailService emailService = new EmailService();
        emailService.sendEmail(message);
    }
}
```

```java {all}
class EmailService {
    public void sendEmail(String message) {
        // Logic to send email
        System.out.println("Email sent: " + message);
    }
}
```

</div>

* the User class directly depends on the **EmailService** class, violating the Dependency Inversion Principle. 

* If we want to change the notification method to use ***SMS*** or ***push notifications*** instead of emails, we'll need to modify the User class.

---

# Dependency Inversion Principle: Solution

<div class="grid grid-cols-2 gap-2">

```java {all}
interface NotificationService {
    void sendNotification(String message);
}
```

```java {all}
class EmailService implements NotificationService {
    public void sendNotification(String message) {
        // Logic to send email
        System.out.println("Email sent: " + message);
    }
}
```

```java {all}
public class User {
    private NotificationService notificationService;

    public User(NotificationService notificationService) {
        this.notificationService = notificationService;
    }

    public void sendNotification(String message) {
        notificationService.sendNotification(message);
    }
}

```

```java {all}
public class Main {
  public static void main(String[] args) {
    // Use the notification services as needed
    User user = new User(new EmailService());
    user.sendNotification("Welcome to our platform!");

    User anotherUser = new User(new SMSService());
    anotherUser.sendNotification("Your order has been shipped.");      
  }
}

```

</div>


* Now, the User class depends on the ***NotificationService*** interface instead of a concrete implementation. 
* This allows us to easily switch to different notification methods without modifying the User class.
---

# Dependency Inversion Principle: Solution

<div class="grid grid-cols-2 gap-2">

```java {all}
class SMSService implements NotificationService {
    public void sendNotification(String message) {
        // Logic to send SMS
        System.out.println("SMS sent: " + message);
    }
}
```

```java {all}
class EmailService implements NotificationService {
    public void sendNotification(String message) {
        // Logic to send email
        System.out.println("Email sent: " + message);
    }
}
```

```java {all}
  interface NotificationStrategy {
    NotificationService createNotificationService();
  }
```

```java {all}
  class EmailStrategy implements NotificationStrategy {
    public NotificationService createNotificationService() {
        return new EmailService();
    }
  }
```

```java {all}
  class SMSStrategy implements NotificationStrategy {
    public NotificationService createNotificationService() {
        return new SMSService();
    }
  }
```

```java {all}
  public class NotificationServiceHandler {
      public NotificationService getNotification
        (NotificationStrategy notificationStrategy) {
          
         return notificationStrategy.createNotificationService();
      }
  }
```

</div>

---
layout: center
class: text-center
---

# Learn More

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
