# Challenge 1 (Level 3): Implementing Inheritance

Create a base class named Animal with a public member function makeSound() that outputs "Unknown sound." Create a derived class named Dog that inherits from Animal. Override the makeSound() function in the Dog class to output "Woof!" Test the Dog class by creating an object and calling its makeSound() function.

# Challenge 2 (Level 4): Identifying the Correct Inheritance Relationship

Given the following code snippet, identify the correct relationship between the base and derived classes.

```cpp
class A {
public:
    void function1() { /*...*/ }
};

class B : public A {
public:
    void function2() { /*...*/ }
};
```

```
A. Class B is a base class, and class A is a derived class.
B. Class A is a base class, and class B is a derived class.
C. There is no inheritance relationship between classes A and B.
```

# Challenge 3 (Level 5): Fixing the Abstract Class and Virtual Function

Examine the following code snippet and identify any issues with the abstract class and virtual function. Provide a corrected version of the code.

```cpp
class Shape {
public:
    void getArea() = 0;
};

class Circle : public Shape {
public:
    Circle(double radius) : m_radius(radius) {}
    double getArea() { return 3.14159 * m_radius * m_radius; }

private:
    double m_radius;
};
```

# Challenge 4 (Level 6): Designing a Class Hierarchy with Abstract Classes and Virtual Functions

Design a class hierarchy for geometric shapes. Your hierarchy should include an abstract base class named Shape with a pure virtual function getArea(). Create two derived classes, Rectangle and Triangle, that inherit from the Shape class. Implement the getArea() function in both derived classes to compute the area of the respective shapes. Test your class hierarchy by creating objects of both Rectangle and Triangle and using their getArea() functions to verify correct results.

# Challenge 5 (Level 6): Implementing a Class Hierarchy with Multiple Inheritance

Research and implement a class hierarchy using multiple inheritance. Design three classes: MusicalInstrument, ElectronicDevice, and Synthesizer. The MusicalInstrument class should have a virtual function playNote() that takes an integer representing the note's frequency and outputs the corresponding note. The ElectronicDevice class should have a virtual function turnOn() that outputs "Device turned on" and a virtual function turnOff() that outputs "Device turned off." The Synthesizer class should inherit from both MusicalInstrument and ElectronicDevice, and implement all the required virtual functions. Test the Synthesizer class by creating an object, turning it on, playing a note, and turning it off.

# Challenge 6 (Level 6): Customizing Standard Library Containers with Inheritance

Research and implement a custom container class that inherits from the C++ Standard Library std::vector class. Add a new member function called getMean() that calculates and returns the mean (average) value of the elements in the container. Assume the container will store integers. Test your custom container by adding a series of integer values, retrieving the mean, and verifying the result.

# Challenge 7 (Level 6): Designing a Polymorphic Container

Design a polymorphic container that can store pointers to objects of different classes derived from a common base class. Start by creating an abstract base class called Drawable with a pure virtual function draw(). Create two derived classes, Circle and Square, that inherit from Drawable. Implement the draw() function in each derived class to output a simple text representation of the respective shapes. Create a container class called Scene that can store pointers to Drawable objects. The Scene class should have a member function render() that calls the draw() function for each object stored in the container. Test your polymorphic container by adding Circle and Square objects to a Scene and rendering the scene.