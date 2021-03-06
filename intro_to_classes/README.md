# Classes in C++

This repository contains a basic introduction to classes in C++. We use the trapezoidal rule for integration as the solution to a basic numerical integration problem.
We develop a class called Trapezoidal that serve as the integration solver. It's aimed at people new to C++ that studies computational science or similar subjects.

### Direct links to the actual codes:
  - [trapezoidal.hpp](https://github.com/reneaas/classes_in_cpp/blob/master/trapezoidal.hpp)
  - [trapezoidal.cpp](https://github.com/reneaas/classes_in_cpp/blob/master/trapezoidal.cpp)
  - [makefile](https://github.com/reneaas/classes_in_cpp/blob/master/makefile)
  - [main.cpp](https://github.com/reneaas/classes_in_cpp/blob/master/main.cpp)

### The integral we solve and the numerical algorithm:
  - [Can be found here](https://github.com/reneaas/classes_in_cpp/blob/master/classes_cpp.pdf)

Before we process to look at classes, we just wanna summarize some basic details specific to C++ codes that we need in this tutorial:

### Some basic details specific to C++ coding

1. Code lines in a C++ code ends with ";". This tells the compiler that the current line has ended.
2. Contrary to Python codes, we must *declare* variables in C++. We'll show this by an example: Say we have a variable *a = 2.0* which is a floating point number and a variable *N = 100* which is an integer.

    * In python this could simply be written as:
      ```python
      a = 2.0
      N = 100
      ```
    * In C++ we must declare the variables and tell the compiler what *types* they are:

    ```C++
    double a = 2.0;
    int N = 100;
    ```

    * In C++ we can also declare it without specifying which value it has, and then assign a value later:

    ```C++
    double a;
    int N;
    a = 2.0;
    N = 100;
    ```

      * Also, note that each line ends with ";".
3. When we define functions, we must also specify the function's type. Say we have some function that returns a floating point number.
    * In python, this would simply be something like this:

    ```python
    def f(x):
      return x**2
    ```

    * While in C++, it would look like this:

    ```C++
    double f(double x){
      return x*x;
    }
    ```

4. *For*-loops are pretty straight forward to implement.
    * In python, it would look something like this

    ```python
    for i in range(N):
        #code block
    ```

    * In C++, the same for loop looks like this:

    ```C++
    for (int i = 0; i < N; i++){
        /* Code block */
    }
    ```

      * The "i++" part is equivalent with i = i + 1 or i+=1 in Python.
5. From a few of the examples above, you may be wondering why there are *curly-brackets* (i.e {...}) in some the C++ codes. This is simply to tell the compiler what belongs to a certain code block.

### Basic ingredients of classes in C++

There are 4 basic ingredients we need when we use classes in C++:

1. A main code, usually called main.cpp.
    * The main program is where you use the actual class.
    * In our case, we use it to solve a given integral.

2. A class code, usually named class_name.cpp
    * In our case, it's called trapezoidal.cpp.
    * This is where all functions are specified.

3. A header file, usually names class_name.hpp
    * This is where we declare the class, its variables and functions/methods.
    * In our case, it's called trapezoidal.hpp

4. A makefile
    * A makefile is either called makefile or Makefile (no file extension, the file should be created with either of these names).
    * This is where you instruct the compiler to compile and execute the code.


At this point, you may wonder: where do we start? The answer is, the header file.

### The header file
We begin our project by thinking about what type of functions we need. In our problem we need the following:

1. A function that initializes the solver
    * This usually means that we specify certain parameters of the class.
    * For the trapezoidal rule this means to specify the integration limits and how many integration points we want.
    * In some cases, this could mean a function that reads data from a file and initializes parameters in the class.

2. A function that solves the problem
    * This is not necessarily always a single function though.
    * In our case, it suffices to have a single function that implements the trapezoidal rule for integration, since this algorithm is pretty simple.

3. A function that writes to file or provides output.
    * In our case, we will just print the computed value to the terminal.
    * In many cases though, it's necessary to write computed values to a file, but we'll restrict ourselves to simply printing the result.


#### Structure of the header file

The basic structure of the header file should be something like this:

```c++
#ifndef HEADERFILENAME_HPP
#define HEADERFILENAME_HPP

class ClassName {
private:
  /* class variables */

public:
  /* function declarations */
};
#endif
```
Note that there's a couple lines here that is necessary to avoid compiling the functions more than once. The block
```c++
#ifndef HEADERFILENAME_HPP
#define HEADERFILENAME_HPP
/* class declaration */
#endif
```
tells the compiler that this code block should only be compiled once (so always do this).

In our case, this basic formula above translates to the following header file (recall that we called the header file trapezoidal.hpp):

```c++
#ifndef TRAPEZOIDAL_HPP
#define TRAPEZOIDAL_HPP

class Trapezoidal {
private:
  double m_a, m_b;      //Integration interval [a,b]
  int m_N;      //Number of integration points.
  double m_stepsize;      //Stepsize in the integration algorithm.
  double m_I = 0.;     //Will store the value of the integral.


public:
  void Initialize(double a, double b, int N);     //Initialize the parameters m_a, m_b  N
  void Integrate(double f(double x));         //Integrate the function
  void Print();                               //Print result to command line
};

#endif
```

Let's discuss a few details here:
- We have two sections: *private* and *public*.
  1. In the *private* section we declare variables that we need inside the class but do not need to access directly from outside the class. (I typically declare all my variables private and use class functions to print, write to file and so forth so I never need to manually change anything - neither should you).
      * *All* the class functions/methods can access these private variables.
      * This means that we rarely need to *return* anything from class functions and simply use type *void*.
      * A function of type *void*, is simply a function that returns nothing at all.
      * You *may* declare functions as private as well. But only member functions of the class will be able to use private functions. This is useful in cases where you want to break a large algorithm into smaller parts that can easily be debugged. 
  2. The *public* section is where we declare the class functions.
      * You may declare variables here too if you want to access or change them from the outside of the class, but we'll restrict ourselves to the simpler case where no variable is public.
- I've used "m_" in all the class variables. This is to distinguish them from the variables we send in. As a result, we get a better overview of which variables belong to the class (called member variables), and which variables do not.
  - You can think of "m_" as the analogue to "self." in python, but it's not something you *must* write in C++.
  - It's just a convention so that when other people read your code, they can more easily understand it.
- All the functions are defined as type *void*. This is because we don't really need access to the variables in any way since we're going to create a class function that prints the result to the screen. After all, all functions inside the class has access to the all member variables that are "private".


### The class code

Now that we've declared the class as well as all its variables and functions, we may write the specific class code. The result may be as below:

```c++
#include "trapezoidal.hpp"
#include <iostream>     //Provides us with function to print to terminal
#include <cmath>        //Provides us with mathematical functions such as sin, cos, exp and so on.

using namespace std;

void Trapezoidal::Initialize(double a, double b, int N)
{
  m_a = a;      //Start point of integration
  m_b = b;      //End point of integration
  m_N = N;      //Number of integration points
  m_stepsize = (b-a)*(1./N);
}

void Trapezoidal::Integrate(double f(double x)){
  double x = 0; //This variable is only needed in this function, so not a member variable.
  //From here we just implement the trapezoidal rule for integration.
  m_I += 0.5*(f(m_a) + f(m_b));
  for (int i = 0; i < m_N; i++){
    x = m_a + i*m_stepsize;
    m_I += f(x);
  }
  m_I *= m_stepsize;
}

void Trapezoidal::Print(){
  //This is an example on how to print to terminal in C++.
  cout << "Integral = " << m_I << endl;
}
```

Let's look at some details here:

1. We must include the header file at the top. We use #include" " instead of #include< > for header files.
2. When we specify the function, we must write:
    * Type ClassName::FunctionName(arguments)

### The main code

Now that we have our header file and class code, we can proceed to use it. This is done via the so-called main code or main program. The general structure of such a code is:

```c++

#include <library1>
#include <library2>
//and so on...

using namespace std; //You can generally always write this namespace (gives tidier code)

int main(int argc, char const *argv[]) {
  /* Write your code here */
  return 0;
}
```

For our purposes, you don't need to worry too much about what the arguments to the main function does. In our case, the realization is

```c++
#include "trapezoidal.hpp"
#include <iostream>     //Need it to print stuff
#include <cmath>        //Need math functions

using namespace std;

double f(double x); //We declare the function here, but specify it at the bottom.

int main(int argc, char const *argv[]) {
  //Parameters
  double a = 0;   //Start point of integration
  double b = 1;   //End point of integration
  int N = 1000;   //Number of integration points

  //Solve integral
  Trapezoidal my_solver;    //Create the solver of type Trapezoidal
  my_solver.Initialize(a, b, N);  //Initialize the parameters in the class/solver
  my_solver.Integrate(f); //Solve the integral
  my_solver.Print();    //Print it to terminal

  return 0;   //Don't worry about this one, but you should always let it stay here at the bottom of the main function.
}

//The function we want to integrate
double f(double x){
  return exp(-x);
}
```


### Makefile
At this point we're done with the codes, but how do we run them? Let's tackle this.

First of all, we must make sure we have a C++ compiler on our computer. To check this, write the following in your terminal and hit enter:

```terminal
which c++
```
If your output is something like this:
```terminal
/usr/bin/c++
```
you already have a C++ compiler and can proceed. If not, you must install it on your computer.
  * On Ubuntu this is done by the following command:
    ```terminal
    sudo apt install gcc g++ -y
    ```
  * On macOS you simply install Xcode from the app store (if you've installed homebrew, you've implicitly installed Xcode and don't need to worry about this).
  * On Windows, the C++ compiler is bundled with Visual Studio Code.

In this case, the makefile need only look as follows

```makefile
all: compile execute

compile:
  c++ -o main.out main.cpp trapezoidal.cpp

execute:
  ./main.out
```

Let's discuss this in some detail:
1. The top line tells the computer that if we write the following in the terminal,
  ```terminal
  make all
  ```
  it should do both the "compile" and "execute" part.

2. The "compile" part is essentially how to compile the code without any fancy optimizations and stuff. It compiles all the .cpp codes and places the *executable* program in "main.out" (the computer translates the code into "machine language" so that it can be run).
    * The first name that follows "-o" will be the name of the executable. So here it will be "main.out"
3. The "execute" part tells the computer to run the executeable file "main.out".
    * the dot "." in "./main.out" tells the computer that the file is in the current directory.
    * In other words, "./main.out" is the so-called *relative path* to the executable file (that is, the full path from the current directory to the file's location).

For the codes written in this small tutorial, we get the following output when we write "make all" in the terminal:

```terminal
make all
c++ -o main.out main.cpp trapezoidal.cpp
./main.out
Integral = 0.633121
```
- The first line is what we write to compile and run the code.
- The second and third line is just the computer telling us precisely which instructions it performs.
- The fourth line is the output from the executable file, which in our case is an approximation to the integral.

#### Doing specific parts of the makefile

Instead of writing
```terminal
make all
```
you could write,
```terminal
make compile
make execute
```
in the terminal. This will do exactly the same.


#### A more clever way to write the makefile

If you have many .cpp files, there's a way to avoid writing out all their names explicitly.
This is done by using the so-called *wildcard* function provided by *make*

```makefile
all: compile execute

compile:
  c++ -o main.out $(wildcard *.cpp)

execute:
  ./main.out
```

Notice how we replaced writing out all .cpp files explicitly by $(wildcard *.cpp). This tells the compiler to search for all files that ends with .cpp and compile them. To compile and execute the code, you simply write "make all" as usual.


#### That's it! A small hands-on tutorial on how to write a class that implements the trapezoidal rule in C++.
