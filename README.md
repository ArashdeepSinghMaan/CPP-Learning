# C++ From First Principles to Advanced — Reasoning-First Learning Path

## 1. Purpose

This project is a structured way to learn **C++ from basic to advanced level without following a conventional course**.

The goal is not to memorize C++ syntax or complete a list of chapters.

The goal is to develop the ability to:

- Understand **why** a C++ feature exists.
- Reason about what the compiler and runtime are doing.
- Understand objects, memory, lifetime, ownership, and types.
- Decide when a feature should and should not be used.
- Read unfamiliar C++ code and explain what it is doing.
- Design clean, efficient C++ systems.
- Eventually understand production-level robotics C++ code such as ROS2, navigation, perception, and planning systems.

The central question throughout this project is:

> **What problem is this C++ concept solving, and why is this particular solution useful?**

---

# 2. Learning Philosophy

We will **not** learn C++ as:

```text
Syntax
  ↓
Definition
  ↓
Example
  ↓
Exercise
  ↓
Next chapter
```

Instead, we will learn it as:

```text
Real Problem
     ↓
Simple Solution
     ↓
Limitations of Simple Solution
     ↓
Need for a Better Abstraction
     ↓
C++ Feature
     ↓
What Happens in Memory / Compiler / Runtime?
     ↓
Trade-offs
     ↓
When to Use It
     ↓
When NOT to Use It
     ↓
Practical Implementation
```

This is the core learning loop for the entire project.

---

# 3. The Six Questions We Will Ask for Every Concept

Whenever we encounter a new C++ feature, we will answer these questions:

### 1. What problem existed before this feature?

Example:

Why do references exist?

We first examine the problems associated with copying objects and directly manipulating addresses.

### 2. What is the simplest solution without this feature?

We deliberately solve the problem using simpler C++.

### 3. Why does that solution become insufficient?

This creates the motivation for the next concept.

### 4. What does C++ introduce to solve the problem?

Only now do we introduce the language feature.

### 5. What is actually happening?

We examine relevant:

- memory
- object lifetime
- stack/heap behavior
- types
- compilation
- runtime behavior
- ownership
- copying/moving
- generated operations

### 6. When should I use it?

We finish with:

- appropriate use cases
- inappropriate use cases
- alternatives
- trade-offs
- common mistakes

---

# 4. The Mental Model We Are Building

C++ becomes much easier when its concepts are connected instead of learned independently.

Our mental model will grow roughly like this:

```text
PROGRAM
  ↓
VALUES
  ↓
TYPES
  ↓
OBJECTS
  ↓
MEMORY
  ↓
REFERENCES / POINTERS
  ↓
LIFETIME
  ↓
OWNERSHIP
  ↓
FUNCTIONS
  ↓
ABSTRACTION
  ↓
CLASSES
  ↓
ENCAPSULATION
  ↓
COMPOSITION
  ↓
POLYMORPHISM
  ↓
GENERIC PROGRAMMING
  ↓
RESOURCE MANAGEMENT
  ↓
CONCURRENCY
  ↓
PERFORMANCE
  ↓
SYSTEM DESIGN
```

The important point is that later concepts should feel like consequences of earlier concepts.

---

# 5. Overall Roadmap

## Level 0 — What Is C++ Actually Doing?

Before learning language features, we establish the basic model.

Topics:

- Source code
- Compilation
- Preprocessing
- Compilation
- Linking
- Executables
- Program startup
- `main()`
- Machine instructions
- Stack and heap at a conceptual level
- Variables and objects
- What happens when code executes

Example starting point:

```cpp
int x = 10;
```

We will ask:

```text
What is x?
Where does 10 exist?
What is an int?
What does the compiler know?
What exists at runtime?
```

---

# 6. Level 1 — Values, Types, Variables and Expressions

Topics:

- Fundamental types
- `int`
- `float`
- `double`
- `char`
- `bool`
- Signed vs unsigned
- Type sizes
- Initialization
- Assignment
- Expressions
- Operators
- Type conversion
- `const`
- Scope

Core question:

> **Why does C++ need a type system?**

We will understand types as information about how an object should be interpreted and manipulated.

---

# 7. Level 2 — Functions and Program Structure

Topics:

- Functions
- Parameters
- Return values
- Pass-by-value
- Scope
- Local variables
- Function declarations
- Function definitions
- Header/source separation
- Namespaces
- `static` at an appropriate conceptual level
- Function overloading

Important question:

> **Why do we need functions instead of writing everything sequentially?**

We will connect functions to:

- abstraction
- reuse
- interfaces
- testing
- code organization

---

# 8. Level 3 — References, Pointers and Addresses

This is one of the most important foundations.

Topics:

- Memory addresses
- `&`
- Pointers
- Dereferencing
- References
- `const` references
- Pointer vs reference
- Null pointers
- Pointer arithmetic
- Arrays and pointer relationships
- Passing large objects efficiently

Mental model:

```text
Object
  ↓
Address
  ↓
Pointer
  ↓
Indirect access
```

We will not memorize:

```cpp
int* p;
int& r;
```

We will understand **why these mechanisms are needed**.

---

# 9. Level 4 — Lifetime, Stack, Heap and Resource Management

Topics:

- Object lifetime
- Automatic storage
- Dynamic storage
- `new`
- `delete`
- Allocation
- Deallocation
- Dangling pointers
- Memory leaks
- Double deletion
- Ownership

Then we ask:

> If manual memory management is dangerous, how should C++ manage resources safely?

This naturally leads to:

```text
Lifetime
   ↓
Ownership
   ↓
RAII
   ↓
Smart pointers
```

---

# 10. Level 5 — Classes and Object-Oriented Programming

We will NOT start with "OOP has four pillars."

Instead:

```text
Problem
  ↓
State becomes complicated
  ↓
Functions manipulate shared state
  ↓
Who owns the state?
  ↓
How do we protect invariants?
  ↓
Class
```

Topics:

- `class`
- `struct`
- Members
- Member functions
- `public`
- `private`
- `protected`
- Constructors
- Destructors
- Initialization
- Member initialization lists
- `this`
- `const` member functions
- Static members

Central idea:

> **A class is a boundary around state, behavior, and rules.**

---

# 11. Level 6 — OOP: Composition, Inheritance and Polymorphism

We will carefully distinguish:

### Composition

```text
Robot
 ├── Camera
 ├── LiDAR
 ├── Controller
 └── Planner
```

### Inheritance

```text
Planner
 ├── AStarPlanner
 ├── RRTPlanner
 └── HybridAStarPlanner
```

### Polymorphism

The user of a component can interact with a common interface without knowing the exact implementation.

Topics:

- Composition
- Inheritance
- Virtual functions
- Abstract classes
- Interfaces
- `override`
- Virtual destructors
- Runtime polymorphism
- Object slicing
- Upcasting/downcasting
- Why composition is often preferable to inheritance

Core question:

> **What problem does polymorphism actually solve?**

---

# 12. Level 7 — The Standard Library

We will learn the STL through problems rather than memorizing containers.

### Sequence containers

- `std::vector`
- `std::array`
- `std::deque`
- `std::list`

### Associative containers

- `std::map`
- `std::set`

### Hash-based containers

- `std::unordered_map`
- `std::unordered_set`

### Utilities

- `std::pair`
- `std::tuple`
- `std::optional`
- `std::variant`
- `std::any`

### Strings

- `std::string`
- `std::string_view`

The question will be:

> **What data organization problem does this container solve?**

For example:

```text
Need:
Fast random access
        ↓
std::vector
```

versus:

```text
Need:
Key → value lookup
        ↓
map / unordered_map
```

---

# 13. Level 8 — Iterators and Algorithms

Topics:

- Iterators
- Range-based loops
- STL algorithms
- `find`
- `sort`
- `transform`
- `remove`
- `accumulate`
- Predicates
- Function objects
- Lambdas

The deeper idea:

> **Separate what we want to do from how the data is stored.**

This introduces the foundation of generic programming.

---

# 14. Level 9 — Modern C++

We will focus heavily on modern C++ practices.

Topics:

- `auto`
- `decltype`
- `nullptr`
- `enum class`
- Range-based loops
- Lambdas
- `constexpr`
- `consteval` where useful
- Structured bindings
- `if constexpr`
- `std::optional`
- `std::variant`
- `std::string_view`
- Smart pointers

We will also learn what modern C++ is trying to improve:

```text
Older C++
   ↓
Manual ownership
Manual resource management
Unclear lifetime
Accidental copies

Modern C++
   ↓
Explicit ownership
RAII
Safer abstractions
Move semantics
Generic programming
```

---

# 15. Level 10 — Copying and Moving

This is a major milestone.

Topics:

- Copy constructor
- Copy assignment
- Move constructor
- Move assignment
- Copy elision
- Rvalues
- Lvalues
- Rvalue references
- `std::move`

Central question:

> **Why should an object sometimes be copied and sometimes have its resources transferred?**

Example:

```text
Large PointCloud
      ↓
Copy
      ↓
Millions of values duplicated

Move
      ↓
Transfer ownership of underlying resources
```

This leads to efficient modern C++.

---

# 16. Level 11 — RAII and Smart Pointers

Topics:

- RAII
- Ownership models
- `std::unique_ptr`
- `std::shared_ptr`
- `std::weak_ptr`
- Custom deleters
- Resource lifetime

We will always ask:

```text
Who owns this object?
Who creates it?
Who destroys it?
Can ownership transfer?
Can multiple components own it?
```

This is particularly important for robotics systems.

---

# 17. Level 12 — Templates and Generic Programming

Topics:

- Function templates
- Class templates
- Template parameters
- Type deduction
- Template specialization
- Variadic templates
- Fold expressions
- Generic algorithms
- Concepts

The fundamental motivation:

> **How can we write code once without unnecessarily restricting the type it operates on?**

Example:

```cpp
template<typename T>
T maximum(T a, T b)
{
    return a > b ? a : b;
}
```

We will eventually move from simple templates to modern constrained generic programming.

---

# 18. Level 13 — Advanced Type System

Topics:

- Type deduction
- `decltype`
- `decltype(auto)`
- References collapsing
- Perfect forwarding
- Universal/forwarding references
- Concepts
- Type traits
- `std::enable_if` conceptually
- `requires`
- Compile-time programming

We will focus on **why these features exist**, because this is where C++ can otherwise become extremely confusing.

---

# 19. Level 14 — Error Handling and Robustness

Topics:

- Return values
- Exceptions
- Exception safety
- RAII and exceptions
- `noexcept`
- `std::optional`
- `std::expected` where appropriate
- Assertions
- Defensive programming

We will ask:

> **How should a function communicate that something went wrong?**

And compare different strategies rather than treating exceptions as automatically good or bad.

---

# 20. Level 15 — Concurrency

Topics:

- Processes vs threads
- `std::thread`
- Mutex
- Lock guards
- Unique locks
- Condition variables
- Atomics
- Futures
- Promises
- Async execution
- Race conditions
- Deadlocks
- Data races

Robotics example:

```text
Camera Thread
      │
      ↓
Image Buffer

LiDAR Thread
      │
      ↓
Point Cloud Buffer

Planning Thread
      │
      ↓
Trajectory
```

We will understand the synchronization problems that arise from this architecture.

---

# 21. Level 16 — Performance and Systems Thinking

This is particularly important for robotics.

Topics:

- Copy vs reference
- Copy vs move
- Dynamic allocation
- Memory layout
- Contiguous memory
- Cache locality
- Data-oriented thinking
- Branching
- Virtual dispatch
- Compile-time vs runtime work
- Profiling
- Latency
- Throughput

We will connect C++ concepts to real performance problems such as:

```text
30 Hz sensor input
       ↓
Image processing
       ↓
Memory allocation
       ↓
Copy
       ↓
Inference
       ↓
Visualization
       ↓
ROS2 publish
```

The goal is to reason about where latency actually comes from.

---

# 22. Level 17 — Build Systems and Real Projects

Topics:

- Header/source organization
- Include guards
- `#pragma once`
- Translation units
- CMake
- Libraries
- Executables
- Linking
- Static libraries
- Shared libraries
- Debug vs Release
- Compiler warnings
- Sanitizers
- Debugging with GDB
- Unit testing

Eventually we should be comfortable with:

```text
project/
├── CMakeLists.txt
├── include/
├── src/
├── tests/
├── examples/
└── README.md
```

---

# 23. Level 18 — Production C++ Design

Topics:

- API design
- Interfaces
- Dependency management
- SOLID principles
- Composition
- Dependency injection
- Design patterns
- Error boundaries
- Testing
- Logging
- Configuration
- Thread safety
- Performance-aware architecture

We will avoid learning design patterns as names to memorize.

For example:

Instead of:

> "This is the Factory Pattern."

We ask:

> "Why do we need to create different implementations without making the calling code depend on their concrete types?"

Only then does the pattern name become useful.

---

# 24. Level 19 — Robotics C++

After the language itself is solid, we will apply the knowledge to robotics.

Possible areas:

```text
ROS2
 │
 ├── Nodes
 ├── Publishers
 ├── Subscribers
 ├── Services
 ├── Actions
 ├── Executors
 ├── Callback groups
 └── Lifecycle nodes
```

And:

```text
Perception
 │
 ├── OpenCV
 ├── Point Clouds
 ├── Eigen
 ├── Tensor inference
 └── Sensor fusion

Navigation
 │
 ├── Costmaps
 ├── Planners
 ├── Controllers
 ├── TF
 └── Behavior Trees
```

The purpose is to connect language-level understanding to real robotics software.

---

# 25. Robotics Projects We Can Use

Throughout the learning process, examples will come from robotics rather than artificial textbook examples.

Potential projects:

### Project 1 — Robot Pose

```text
x
y
theta
```

Use it to understand:

- structs
- classes
- functions
- references
- const
- constructors

### Project 2 — Sensor Abstraction

```text
Sensor
 ├── Camera
 ├── LiDAR
 └── IMU
```

Use it to understand:

- interfaces
- inheritance
- virtual functions
- polymorphism

### Project 3 — Point Cloud Container

Use it to understand:

- vectors
- memory
- copying
- moving
- ownership
- performance

### Project 4 — Planner Interface

```text
Planner
 ├── A*
 ├── RRT
 └── Hybrid A*
```

Use it to understand:

- abstraction
- polymorphism
- smart pointers
- dependency design

### Project 5 — Sensor Pipeline

```text
Camera
   ↓
Processing
   ↓
Fusion
   ↓
Planner
```

Use it to understand:

- concurrency
- ownership
- queues
- synchronization
- performance

### Project 6 — Mini ROS2-style Architecture

Eventually we can build a small C++ robotics framework ourselves before looking deeply into ROS2 internals.

---

# 26. How Each Discussion Will Work

Our conversations should generally follow this sequence:

```text
1. Introduce a problem
        ↓
2. Try the simplest solution
        ↓
3. Find its limitations
        ↓
4. Introduce the C++ feature
        ↓
5. Understand syntax
        ↓
6. Understand memory/runtime behavior
        ↓
7. Implement a small example
        ↓
8. Break the example deliberately
        ↓
9. Debug/reason about the failure
        ↓
10. Compare alternatives
        ↓
11. Apply it to robotics
        ↓
12. Move to the next concept
```

We will not move forward just because a chapter is "finished."

We move forward when the underlying reasoning is clear enough.

---

# 27. The "Why?" Rule

At any point, it is completely acceptable to ask:

> Why?

For example:

```cpp
const std::vector<int>& data
```

Instead of accepting it, we can decompose:

```text
Why vector?
Why const?
Why reference?
Why not pointer?
Why not copy?
Why not span?
What does the compiler do?
What happens if the vector dies?
```

This is encouraged.

**Confusion is not failure.**

If a concept doesn't make logical sense, we stop and rebuild the mental model.

---

# 28. What We Will NOT Do

We will avoid:

- Blindly memorizing syntax.
- Following a video course chapter-by-chapter.
- Learning design patterns by name first.
- Writing classes just because "OOP is important."
- Using pointers without understanding ownership/lifetime.
- Using `shared_ptr` everywhere.
- Treating advanced syntax as automatically better.
- Optimizing code before understanding correctness.
- Memorizing STL containers without understanding their trade-offs.
- Jumping into templates before understanding types.
- Jumping into ROS2 C++ before understanding the language foundations.

---

# 29. The Standard for Understanding

For every major concept, the target is eventually:

### Beginner understanding

> "I know how to write it."

### Intermediate understanding

> "I know what it does."

### Strong understanding

> "I know why it exists."

### Advanced understanding

> "I understand its trade-offs and alternatives."

### Expert-level reasoning

> "I can decide whether this abstraction is appropriate for the system I am designing."

Our goal is to move toward the last two levels.

---

# 30. Progress Tracking

We will maintain progress conceptually rather than by course completion.

```text
[ ] Program / Compilation Model
[ ] Types
[ ] Variables / Objects
[ ] Expressions
[ ] Functions
[ ] Scope
[ ] References
[ ] Pointers
[ ] Memory
[ ] Lifetime
[ ] Ownership
[ ] RAII
[ ] Classes
[ ] Constructors / Destructors
[ ] Encapsulation
[ ] Composition
[ ] Inheritance
[ ] Polymorphism
[ ] STL Containers
[ ] Iterators
[ ] Algorithms
[ ] Lambdas
[ ] Modern C++
[ ] Copy Semantics
[ ] Move Semantics
[ ] Smart Pointers
[ ] Templates
[ ] Concepts
[ ] Error Handling
[ ] Concurrency
[ ] Performance
[ ] CMake
[ ] Debugging
[ ] Testing
[ ] System Design
[ ] Robotics C++
[ ] ROS2 C++
```

This list is a **map**, not a deadline.

---

# 31. Final Goal

At the end of this learning path, the objective is not merely to say:

> "I know C++."

The objective is to be able to look at something like:

```cpp
std::unique_ptr<nav2_core::GlobalPlanner>
planner =
    std::make_unique<SmacPlannerHybrid>();

planner->configure(...);
```

and reason about:

```text
Types
Ownership
Lifetime
Dynamic allocation
Polymorphism
Interfaces
Move semantics
RAII
Architecture
Performance
```

without feeling that the code is mysterious.

More importantly, when designing a new robotics component, the goal is to be able to ask:

```text
What owns this?
What is its lifetime?
Who can modify it?
Should this be copied?
Should this be moved?
Does this need dynamic allocation?
Should this be a class?
Should this use composition?
Do I actually need inheritance?
What interface should it expose?
What happens if it fails?
Can it run concurrently?
Where will performance matter?
```

That is the reasoning skill this project is designed to build.

---

# 32. Starting Point

We will begin at the absolute foundation:

## Lesson 1 — What Actually Happens When We Write:

```cpp
int x = 10;
```

We will not assume that this is "too basic."

We will use it to establish the mental model for:

```text
Source Code
     ↓
Compiler
     ↓
Type
     ↓
Object
     ↓
Memory
     ↓
Address
     ↓
Value
     ↓
Lifetime
     ↓
Machine Code
```

Once this foundation is solid, pointers, references, classes, OOP, RAII, smart pointers, templates, and advanced C++ will have a much more logical place in the overall picture.

---

# Core Principle

> **Do not learn C++ as a collection of features.**
>
> **Learn it as a collection of solutions to problems involving data, behavior, types, memory, lifetime, ownership, abstraction, and performance.**

That principle will guide the entire discussion.
