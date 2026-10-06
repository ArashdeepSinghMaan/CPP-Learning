# C++ From First Principles — Level 0
# What Is a C++ Program Actually Doing?

> **Learning principle:** Do not learn C++ as a collection of syntax rules. Learn it as a set of abstractions built on top of computers, memory, machine instructions, and operating-system resources.

## 1. Purpose of Level 0

Level 0 establishes the mental model that supports everything else in C++.

Before pointers, references, classes, OOP, constructors, destructors, RAII, smart pointers, templates, concurrency, or ROS2 C++, we need to understand what a C++ program actually is.

The goal is not to become a compiler or operating-system expert. The goal is to understand the chain:

```text
C++ Source Code
      ↓
Preprocessing
      ↓
Compilation
      ↓
Object Code
      ↓
Linking
      ↓
Executable
      ↓
Operating System
      ↓
Process
      ↓
Memory
      ↓
Objects
      ↓
CPU Instructions
```

We will learn by repeatedly asking **what problem exists, what the machine needs, what abstraction C++ provides, and what actually happens at runtime**.

## 2. The Six Questions

For every concept, ask:

1. What problem existed before this feature?
2. What is the simplest solution without it?
3. Why does that solution become insufficient?
4. What does C++ introduce to solve the problem?
5. What happens in memory, the compiler, and runtime?
6. When should I use it, and when should I not?

This reasoning loop is more important than memorizing definitions.

## 3. What Is C++?

C++ is a programming language. It lets humans describe data, operations, control flow, abstractions, and resource-management rules.

A CPU does not directly understand C++ syntax. It executes machine instructions. Therefore:

```text
Human-readable C++
        ↓
      compiler
        ↓
machine-oriented code
        ↓
       CPU
```

C++ is particularly interesting because it provides high-level abstractions while still allowing detailed control over memory, object lifetime, resource ownership, and performance.

## 4. Source Code Is Not the Running Program

Consider:

```cpp
int x = 10;
```

That is source code. It is not itself a running object in memory.

A useful model is:

```text
source code
    ↓
translation/build
    ↓
executable
    ↓
operating system loads it
    ↓
process
    ↓
runtime objects and state
    ↓
CPU executes instructions
```

Therefore:

```text
source code ≠ executable ≠ process ≠ object
```

## 5. A First C++ Program

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello C++\n";
    return 0;
}
```

Compile:

```bash
g++ hello.cpp -o hello
```

Run:

```bash
./hello
```

At this stage, do not try to memorize every line. We are interested in the program lifecycle.

## 6. The Compilation Pipeline

A simplified build pipeline is:

```text
.cpp source
    ↓
preprocessing
    ↓
compilation
    ↓
object file
    ↓
linking
    ↓
executable
```

### Preprocessing
Handles directives such as:

```cpp
#include <iostream>
#define VALUE 10
```

### Compilation
The compiler parses C++, checks types and names, and generates machine-oriented code.

### Object file
A separately compiled piece of the program. It is not necessarily a complete runnable program.

### Linking
The linker combines object files and required libraries into an executable.

## 7. Why Headers and Libraries Exist

Large C++ programs are split into components:

```text
robot.hpp / robot.cpp
planner.hpp / planner.cpp
controller.hpp / controller.cpp
```

Headers commonly expose declarations/interfaces; source files commonly contain implementations.

Libraries package reusable functionality such as:

```text
Eigen
OpenCV
PCL
ROS2
CUDA
TensorRT
```

The important build idea is:

```text
your source
    +
separately compiled code
    +
libraries
    ↓
linker
    ↓
executable
```

This distinction will become important when we study CMake and ROS2.

## 8. Translation Units

A `.cpp` file together with its preprocessed content forms a translation unit.

For example:

```text
main.cpp    → translation unit → main.o
robot.cpp   → translation unit → robot.o
planner.cpp → translation unit → planner.o
```

These pieces can be compiled separately and later linked together. This is one reason large projects can be organized into many source files.

## 9. Executable vs Process

An executable is a file containing program code and related information.

A process is a running instance of that program.

```text
executable file
      ↓
Operating System
      ↓
running process
```

The same executable can be started multiple times, creating multiple processes.

## 10. What Is Memory?

A running process needs memory for code, data, objects, execution state, and resources.

A simplified model is:

```text
Process
┌──────────────────────┐
│ Program code         │
├──────────────────────┤
│ Global/static data   │
├──────────────────────┤
│ Dynamic storage      │
├──────────────────────┤
│ Stack / call state   │
└──────────────────────┘
```

This is intentionally simplified. Modern systems use virtual memory, mappings, shared libraries, guard pages, and other mechanisms.

For now, remember:

> A running program needs storage in which its code, data, objects, and execution state can exist.

## 11. Object, Type, Value, Name, and Memory

Consider:

```cpp
int x = 10;
```

A useful decomposition is:

```text
x
│
├── name: x
├── type: int
├── object: the runtime entity
├── value: 10
├── storage: memory used to represent it
└── lifetime: period during which it exists
```

These terms are related but not identical.

A **type** describes what kind of object/value is involved.

An **object** is a runtime entity with storage and a lifetime.

A **value** is information represented by an object/expression.

A **name** is how source code refers to an entity.

An **address** identifies a storage location.

This distinction becomes crucial when we later study pointers.

## 12. Objects and Values

Two objects can contain the same value without being the same object:

```cpp
int x = 10;
int y = 10;
```

Conceptually:

```text
x → [10]

y → [10]
```

Same value, different objects.

Compare this later with:

```cpp
int& y = x;
```

where `y` refers to the existing object `x`.

This distinction is foundational for references, pointers, copying, and object identity.

## 13. Initialization vs Assignment

These are different operations:

```cpp
int x = 10;   // initialization
x = 20;       // assignment
```

Initialization establishes the initial state of an object. Assignment changes the value/state of an already-existing object.

This distinction becomes important later for constructors, copy assignment, move assignment, and object lifetime.

## 14. Lifetime and Scope

Every object has a lifetime:

```text
creation
   ↓
initialization
   ↓
use
   ↓
modification
   ↓
destruction
```

**Scope** is about where a name can be used in source code.

**Lifetime** is about how long the corresponding object exists.

They often appear together for local variables, but they are not the same concept.

This distinction will matter for pointers, references, dynamic storage, temporaries, static objects, and concurrency.

## 15. Compile Time vs Runtime

**Compile time** is when the program is being translated/built. The compiler can perform syntax checking, many type checks, template instantiation, overload resolution, and compile-time computation.

**Runtime** is when the executable is actually running.

```text
compile time → build the program
runtime      → execute the program
```

Modern C++ deliberately moves some work to compile time using mechanisms such as templates and `constexpr`.

## 16. Stack and Dynamic Storage — First Introduction

A local object such as:

```cpp
void f()
{
    int x = 10;
}
```

is associated with automatic storage duration and is commonly implemented using the call stack.

Dynamic allocation such as:

```cpp
int* p = new int(10);
```

uses dynamically allocated storage, commonly called the heap/free store.

Do not memorize the oversimplification `local = stack` and `new = heap` as universal rules. C++ specifies lifetimes and storage durations; implementations realize them using concrete mechanisms.

We will study this properly when we reach memory and lifetime.

## 17. CPU Instructions and Optimization

The CPU executes machine instructions. C++ source statements do not necessarily map one-to-one to instructions.

For example:

```cpp
int z = x + y;
```

might compile into several instructions, fewer instructions, or be optimized away if the result has no observable effect.

Therefore:

> C++ source code describes required program behavior; the compiler chooses an appropriate machine-level implementation subject to the language rules.

This becomes important when we later study performance, undefined behavior, inlining, and concurrency.

## 18. Undefined Behavior — First Introduction

C++ defines rules for what programs may do. If a program performs an operation for which the language provides no defined behavior, it can have **undefined behavior**.

For now, remember:

```text
defined behavior
    → language specifies requirements

undefined behavior
    → the standard places no requirements on the result
```

We will study this carefully later because it is one of the most important advanced C++ topics.

## 19. Why C++ Feels Difficult

C++ combines several layers:

```text
Language syntax
      +
Type system
      +
Object model
      +
Memory model
      +
Resource management
      +
Compiler behavior
      +
Operating system
      +
Hardware
```

A line such as:

```cpp
std::unique_ptr<Robot>
```

eventually involves:

```text
Robot
  → type

unique_ptr
  → ownership abstraction

template argument
  → generic type relationship

resource
  → lifetime

RAII
  → automatic cleanup
```

The solution to C++ confusion is therefore not more memorization. It is learning these layers in the correct order.

## 20. Robotics Connection

C++ concepts directly affect robotics systems.

Consider:

```text
Camera
  ↓
Image
  ↓
Inference
  ↓
Detections
  ↓
Tracking
  ↓
Planning
  ↓
Control
```

At every stage we eventually need to answer:

```text
Who owns the data?
Where is it stored?
How long does it exist?
Is it copied?
Can it be moved?
Who can modify it?
Can multiple threads access it?
When is it destroyed?
```

Examples include:

```text
Image
PointCloud
GridMap
Detection
Trajectory
Costmap
RobotModel
```

This is why the Level 0 foundation is directly relevant to robotics C++.

## 21. How to Read C++ Code

When encountering unfamiliar code, use this sequence:

1. What is the type?
2. What object/name is being declared or referred to?
3. What value/state does it contain?
4. Where is its storage?
5. Who owns the resource?
6. What is its lifetime?
7. Can it be copied?
8. Can it be moved?
9. Can another part of the program modify it?
10. What happens when its lifetime ends?

For example, when we eventually see:

```cpp
std::unique_ptr<Planner> planner;
```

we should not see mysterious syntax. We should ask:

```text
What is Planner?
What is unique_ptr?
Who owns the Planner?
When is it destroyed?
Can ownership move?
Why use this instead of a raw pointer?
```

Those questions are the actual learning process.

## 22. Common Misconceptions

### Misconception: "The compiler runs my C++ code."
The compiler translates source code. The resulting program is executed by the runtime environment/CPU.

### Misconception: "A variable is just a memory address."
A variable/name refers to an object. The object has storage; that storage has an address.

### Misconception: "A class is an object."
A class defines a type. An object can be created from that type.

```cpp
class Robot {};
Robot r;
```

`Robot` is the type; `r` is an object.

### Misconception: "Every statement becomes one CPU instruction."
The compiler may generate many instructions, fewer instructions, or no instructions for a source-level operation.

### Misconception: "Heap and stack are simply slow and fast memory."
They represent different storage/lifetime/allocation mechanisms. Performance depends on many additional factors.

## 23. A Complete Level 0 Analysis of `int x = 10;`

For:

```cpp
int x = 10;
```

reason through:

```text
int
 ↓
type

x
 ↓
name

10
 ↓
integer literal / initial value

initialization
 ↓
object receives initial state

storage
 ↓
program needs a place to represent the object

lifetime
 ↓
object exists for a defined period

compiler
 ↓
determines how this can be implemented

runtime
 ↓
program establishes and uses the object's state
```

The mental model is:

```text
x
│
├── name
├── type → int
├── object
├── value → 10
├── storage
└── lifetime
```

## 24. Level 0 Mental Model

Keep this diagram as the core reference:

```text
                         C++ PROGRAM
                              │
                              ↓
                         SOURCE CODE
                              │
                              ↓
                       PREPROCESSOR
                              │
                              ↓
                          COMPILER
                              │
                              ↓
                       OBJECT FILES
                              │
                              ↓
                           LINKER
                              │
                              ↓
                          EXECUTABLE
                              │
                              ↓
                     OPERATING SYSTEM
                              │
                              ↓
                           PROCESS
                              │
                 ┌────────────┴────────────┐
                 ↓                         ↓
               CODE                      DATA
                 │                         │
                 └────────────┬────────────┘
                              ↓
                            MEMORY
                              │
                              ↓
                           OBJECTS
                              │
             ┌────────────────┼────────────────┐
             ↓                ↓                ↓
           TYPE             VALUE           LIFETIME
             │                │                │
             └────────────────┴────────────────┘
                              ↓
                       CPU EXECUTION
```

The central idea is:

> **C++ source code is translated into executable behavior, and that behavior creates and manipulates objects with types, values, storage, and lifetimes.**

## 25. Level 0 Checklist

- [ ] Explain what C++ is.
- [ ] Explain the difference between source code and executable code.
- [ ] Explain preprocessing at a high level.
- [ ] Explain compilation at a high level.
- [ ] Explain what an object file is.
- [ ] Explain what the linker does.
- [ ] Explain why headers exist.
- [ ] Explain why libraries exist.
- [ ] Explain what a translation unit is.
- [ ] Explain the difference between an executable and a process.
- [ ] Explain what memory is at a conceptual level.
- [ ] Distinguish a value from an address.
- [ ] Distinguish a type from an object.
- [ ] Distinguish a name from the object it refers to.
- [ ] Explain object lifetime.
- [ ] Distinguish scope from lifetime.
- [ ] Distinguish compile time from runtime.
- [ ] Explain why compiler output is not necessarily a one-to-one translation of source statements.
- [ ] Explain the first idea of undefined behavior.
- [ ] Explain why these foundations matter for robotics C++.

## 26. Mini Exercises

### Exercise 1

For:

```cpp
int x = 10;
```

identify:

- type
- name
- object
- initial value
- storage
- lifetime

### Exercise 2

For:

```cpp
int x = 10;
int y = 10;
```

answer:

- How many objects?
- How many names?
- What values do they contain?
- Are they the same object?

### Exercise 3

For:

```cpp
int x = 10;
x = 20;
```

explain the difference between initialization and assignment.

### Exercise 4

Draw the build pipeline for:

```text
main.cpp
robot.cpp
planner.cpp
```

through:

```text
translation units
→ object files
→ linker
→ executable
→ process
```

### Exercise 5

Explain in your own words:

> Why is `int x = 10;` not itself "a memory address containing 10"?

Do not worry about using textbook terminology. The goal is correct reasoning.

## 27. What Comes Next

Level 0 can be explored in the following sequence:

### Level 0A — Program and Execution
- source code
- executable
- process
- CPU
- memory
- runtime

### Level 0B — Build System Foundations
- preprocessing
- headers
- translation units
- compilation
- object files
- linking
- libraries

### Level 0C — Memory
- bytes
- addresses
- object representation
- alignment
- stack
- dynamic storage

### Level 0D — Objects and Lifetime
- creation
- initialization
- lifetime
- scope
- destruction
- storage duration

### Level 0E — Machine Behavior
- generated instructions
- optimization
- observable behavior
- first deeper look at undefined behavior

Only after this foundation is comfortable will we move to Level 1: **Types, Values, Variables, and Expressions**.

## 28. Rule for the Entire C++ Project

Whenever something feels like:

> "I know how to write this, but I don't understand why it works this way."

we stop.

We do not fix that gap by memorizing more syntax.

We go one layer deeper:

```text
C++ syntax
    ↓
language rule
    ↓
type/object model
    ↓
memory/lifetime
    ↓
compiler
    ↓
machine/system
```

Then we come back up.

That is the reasoning-first method for this entire C++ journey.