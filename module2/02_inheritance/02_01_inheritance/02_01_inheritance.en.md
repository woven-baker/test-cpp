---
Concept: C++ Inheritance
Required: [ C++ Classes, C++ Functions, C++ Object Oriented Programming ]
Notes:
---

# C++ Inheritance

**Inheritance** is a fundamental concept in object-oriented programming (OOP) that allows you to create a new class, called a **derived class**, from an existing class, called a **base class**. The derived class inherits properties and methods from the base class, allowing you to reuse existing code and extend functionality as needed. Inheritance is different from **composition**, which is another way of reusing code by including an instance of a class within another class.

## Problem Scenario

Imagine you are developing a game that includes several types of characters, such as warriors, mages, and archers. Each character type shares some common properties, like health, level, and experience, but they also have unique abilities and attributes.

To model these characters, you could create separate classes for each character type and duplicate the common properties and methods in each class. However, this approach would lead to a lot of redundant code and make it difficult to maintain and update the codebase.

## Inheritance as a Solution

Inheritance offers a more efficient solution to this problem. Instead of duplicating the common properties and methods in each character class, you can create a base class called `Character` that includes the shared properties and methods. Then, you can define derived classes for each character type (e.g., `Warrior`, `Mage`, and `Archer`) that inherit from the `Character` base class.

```
Character (Base Class)
|
├── Warrior (Derived Class)
|
├── Mage (Derived Class)
|
└── Archer (Derived Class)
```

This approach enables you to reuse the common code in the `Character` class and extend or override the functionality in the derived classes as needed.

## Inheritance vs. Composition

Inheritance and composition are both techniques for reusing code and modeling relationships between classes. However, they have different use cases and implications.

**Inheritance** is used when there is an "is-a" relationship between classes. In the game character example, a `Warrior` _is a_ `Character`, so inheritance is appropriate.

**Composition** is used when there is a "has-a" relationship between classes. For example, a `Car` _has a_ `Engine`, so composition would be appropriate for modeling this relationship.

In general, you should prefer composition over inheritance when possible, as it promotes more modular and flexible code. However, inheritance can be a powerful tool for modeling and reusing code in certain scenarios, such as the game character example.

## When to Use Inheritance

Inheritance should be used when:

1. There is a clear "is-a" relationship between classes.
2. You need to reuse code or properties from a base class.
3. You want to take advantage of polymorphism, which allows you to use a derived class object as if it were an object of the base class.

## Pseudocode Example

Here is a simple pseudocode example that demonstrates how to use inheritance in the game character scenario:

```cpp
class Character {
// Common properties and methods
}

class Warrior inherits Character {
// Warrior-specific properties and methods
}

class Mage inherits Character {
// Mage-specific properties and methods
}

class Archer inherits Character {
// Archer-specific properties and methods
}
```

By using inheritance, you can efficiently model the relationships between different character types and reuse the common properties and methods defined in the `Character` base class.

# Exercises

## Exercise 1 (Level 1)
Choose the correct definition of inheritance in object-oriented programming:

```
A: The process of including an instance of one class within another class.
B: The process of creating a new class by reusing properties and methods from an existing class.
C: The process of splitting a large class into smaller, more specialized classes.
D: The process of combining multiple classes into a single, more general class.
```

---

```
B
```

## Exercise 2 (Level 2)
Consider the following class hierarchy:

```
Animal (Base Class)
    |
    ├── Mammal (Derived Class)
    |
    ├── Bird (Derived Class)
    |
    └── Reptile (Derived Class)
```

Which statement best describes the relationship between the Animal class and its derived classes?

```
A: An Animal has a Mammal, Bird, and Reptile.
B: An Animal is a Mammal, Bird, and Reptile.
C: A Mammal, Bird, and Reptile are Animals.
D: A Mammal, Bird, and Reptile have an Animal.
```

---

```
C
```

## Exercise 3 (Level 3)
Imagine you are designing a simple vehicle hierarchy. The base class is called Vehicle, which contains common properties like speed and fuel. You also have two derived classes: Car and Motorcycle, both of which inherit from Vehicle. Create a pseudocode representation of this class hierarchy.

---

```cpp
class Vehicle {
    // Common properties: speed, fuel
}

class Car inherits Vehicle {
    // Car-specific properties and methods
}

class Motorcycle inherits Vehicle {
    // Motorcycle-specific properties and methods
}
```

## Exercise 4 (Level 4)
Consider the following class hierarchy:

```
Shape (Base Class)
    |
    ├── Circle (Derived Class)
    |
    ├── Rectangle (Derived Class)
    |
    └── Triangle (Derived Class)
```

You have been asked to add a new feature to calculate the perimeter of each shape. Which of the following approaches would be the most appropriate way to implement this functionality?

```
A: Add a perimeter() method to each derived class (Circle, Rectangle, and Triangle), implementing the specific perimeter calculation for each shape.

B: Add a perimeter() method to the Shape base class, implementing a general perimeter calculation that works for all shapes.

C: Modify the Shape base class to include new properties required for perimeter calculation, then add a perimeter() method to each derived class.

D: Create a new PerimeterCalculator class that takes a Shape object as input and calculates the perimeter based on the shape's properties.
```

---

```
A
```