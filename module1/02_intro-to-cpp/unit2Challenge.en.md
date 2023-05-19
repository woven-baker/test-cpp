# Unit 2 Challenge
In this Challenge, you will practice using fundamental C++ skills and keywords.

## Requirements
- Copy and Paste the below code into a new `.cpp` file.
- The code is full of errors that need to be fixed.
- Read the comments in the code, and complete the tasks.
- Compile the code and execute it.

If the code compiles and executes without errors, the challenge is complete.

```cpp
#include <iostream>
#include <string>


int main() {
    //Complete everything in this program.
    //Compile and execute the program without errors.

    //Complete the following variables by using the correct types for the data.
    pizzaSlices = 12;
    pi = 3.14159f;
    starsInTheUniverse = 1234567890LL;
    aLetter = 'k';
    aWord = "kaleidescope";
    veryPrecisePi = 3.14159265358979;
    thisIsTrue = true;
    thisIsAlsoTrue = thisIsTrue;


    //On the next line, make a pointer that holds an int value.

    //On the next line, dereference your pointer, and store the value in a new variable.

    //On the next three lines, declare and initialize a new variable on each line. Each variable must be a different data type.
    //variable #1 here
    //variable #2 here
    //variable #3 here


    //Complete these arrays by giving them the correct data type.
    myArray[] = {1,2,3,4}
    anotherArray[] = {3.5, 1.2, 6.7}

    //Make your own array on the next line. It should contain at least 5 numbers. The numbers can be any number data type, but they must all be the same data type.
    
    //This array should not use `int` because it stores big numbers. What data type is better?
    bigNumbers[] = {500000, 700000, 800000}

    //Use static_cast to change 'sizeOfCesiumAtom' to an int. Use 'static_cast' on the line after sizeOfCesiumAtom.
    double sizeOfCesiumAtom = 0.267

    //Make three constant variables. You can use any data type, but each variable must be a different data type.

    //Optional challenge: use std::cout to print the new variable you made with static_cast, on the next line.


    //You don't need to change anything here. These lines are for checking program output.
    std::cout << pizzaSlices << std::endl;
    std::cout << pi << std::endl;
    std::cout << starsInTheUniverse << std::endl;
    std::cout << aLetter << std::endl;
    std::cout << aWord << std::endl;
    std::cout << veryPrecisePi << std::endl;
    std::cout << thisIsTrue << std::endl;
    std::cout << thisIsAlsoTrue << std::endl;
    std::cout << "The size of a Cesium atom in nanometers is: " << sizeOfCesiumAtom << std::endl;

    

    return 0;
}
```