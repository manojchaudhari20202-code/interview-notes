## Java Fundamentals

### Java platform overview
- Java is a platform-independent, object-oriented programming language based on the principle of "Write Once, Run Anywhere" (WORA) due to its bytecode execution on the Java Virtual Machine (JVM).
- The platform consists of the Java Virtual Machine (JVM), the Java Runtime Environment (JRE), and the Java Development Kit (JDK), providing a complete environment for developing and running Java applications.
- Java is widely used for enterprise applications, Android development, web applications, and big data technologies, owing to its robustness, security, and extensive ecosystem.
- The platform includes a rich set of APIs (Application Programming Interfaces) for tasks such as I/O, networking, collections, concurrency, and GUI development.
- Java's architecture is designed to be secure, with features like bytecode verification, class loaders, and a security manager to protect against malicious code.

### JVM, JRE, JDK differences
- **JDK (Java Development Kit)** is a software development kit that includes the JRE, development tools (like javac, jar, javadoc), and additional libraries for developing Java applications.
- **JRE (Java Runtime Environment)** provides the libraries, JVM, and other components necessary to run Java applications but does not include development tools.
- **JVM (Java Virtual Machine)** is the abstract machine that executes Java bytecode, providing runtime environment features like memory management, garbage collection, and security.
- The JVM is platform-specific; there are different implementations for different operating systems, but they all execute the same bytecode, ensuring platform independence.
- Understanding the distinction is crucial for deployment: JDK is needed for development, JRE for running applications, and JVM is the core runtime engine.

### Java compilation process
- Java source code (.java files) is compiled by the Java compiler (javac) into bytecode (.class files), which is platform-independent.
- The bytecode is then interpreted and/or compiled by the JVM into native machine code at runtime, using Just-In-Time (JIT) compilation for performance.
- The compilation process involves lexical analysis, parsing, semantic analysis, and bytecode generation, ensuring type safety and other language constraints.
- Unlike compiled languages like C++, Java's compilation to bytecode allows for dynamic class loading and linking at runtime.
- The compilation process can be customized with annotation processors that generate additional source files or resources during compilation.

### Bytecode
- Bytecode is an intermediate, platform-independent representation of Java code, stored in .class files, consisting of instructions for the JVM.
- Each bytecode instruction is one byte in length, giving the JVM a compact instruction set (e.g., iload, invokespecial, etc.).
- Bytecode is executed by the JVM through interpretation or JIT compilation, which translates frequently executed bytecode sequences into native machine code for performance.
- The JVM verifies bytecode before execution to ensure it doesn't violate security or type constraints (bytecode verification).
- Tools like javap can disassemble class files to view bytecode, which is useful for debugging and understanding JVM behavior.

### Java program structure
- A Java program consists of one or more classes, with at least one class containing a main method as the entry point: `public static void main(String[] args)`.
- Classes can be organized into packages to avoid naming conflicts and provide access control.
- Java source files typically have the same name as the public class they contain, and the file extension is .java.
- The program can include import statements to bring other classes into scope, and comments for documentation.
- Beyond classes, Java programs may use interfaces, enums, annotations, and records, all of which are top-level types.

### Data types
- Java has two categories of data types: primitive types (byte, short, int, long, float, double, char, boolean) and reference types (classes, interfaces, arrays, enums).
- Primitive types hold their values directly in memory, while reference types hold references to objects stored in the heap.
- Each primitive type has a fixed size and range, e.g., int is 32-bit signed, float is 32-bit IEEE 754 floating point.
- Java also supports literals for primitives (e.g., 42, 3.14, true, 'a') and special literals like null for references.
- Understanding default values: primitives have defaults (e.g., 0 for numeric types, false for boolean), while references default to null.

### Primitive vs reference types
- Primitive types store actual values, while reference types store addresses to objects in memory.
- Primitives are stored on the stack for local variables, whereas reference variables are on the stack but the object they point to is on the heap.
- Operations on primitives are generally faster than on objects due to no indirection or garbage collection overhead.
- Reference types can be null, while primitives cannot; this can lead to NullPointerException when using reference types.
- Primitives are passed by value, while references are also passed by value (the reference itself is copied, but both refer to the same object).

### Variables and scope
- Variables in Java have a specific scope: class-level (static), instance-level (non-static), local (method/block), and parameter scope.
- Local variables must be initialized before use; instance and class variables get default values if not initialized.
- Scope determines visibility and lifetime: local variables exist only within the block, instance variables live as long as the object, static variables live for the entire program.
- Shadowing occurs when a variable in an inner scope has the same name as a variable in an outer scope, hiding the outer variable.
- The `this` keyword is used to refer to the current instance variable when shadowed by a local variable or parameter.

### Operators
- Java provides a rich set of operators: arithmetic (`+`, `-`, `*`, `/`, `%`), relational (`<`, `>`, `<=`, `>=`, `==`, `!=`), logical (`&&`, `||`, `!`), bitwise (`&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`), assignment (`=`, `+=`, etc.), and ternary (`? :`).
- Operators have precedence and associativity rules that determine evaluation order; parentheses can override.
- The `instanceof` operator checks whether an object is an instance of a particular class or interface.
- Short-circuit logical operators (`&&` and `||`) evaluate the second operand only if necessary, which can prevent errors or improve performance.
- Bitwise operators are used for low-level operations, such as flags or performance-critical code.

### Control flow statements
- Control flow statements include decision-making (`if-else`, `switch`), looping (`for`, `while`, `do-while`), and branching (`break`, `continue`, `return`).
- The `switch` statement can now use strings, enums, and primitive types; since Java 12, switch expressions with arrow syntax and yield return values.
- `if-else` can be chained, and nested loops are common, but excessive nesting can reduce readability.
- Enhanced for-each loop (`for (Type var : iterable)`) simplifies iteration over arrays and collections.
- `break` and `continue` can have labels to break out of nested loops, though this can make code harder to follow.

### Loops
- `for` loop: classic three-part loop (initialization, condition, update) used when iteration count is known.
- `while` loop: condition checked before entry, used when number of iterations is unknown.
- `do-while` loop: executes body at least once, condition checked after.
- Enhanced for-each loop: simpler syntax for iterating over arrays and collections, but does not provide index access.
- Loop performance considerations: avoid repeated method calls in condition, use primitive arrays for performance, and consider Stream API for functional style.

### Arrays
- Arrays in Java are objects that hold a fixed number of values of a single type, created with `new` or by initializer (`int[] arr = {1,2,3}`).
- Arrays are zero-indexed, have a `length` property, and provide bounds checking at runtime (throws `ArrayIndexOutOfBoundsException`).
- Multidimensional arrays are arrays of arrays, which can be non-rectangular (jagged arrays).
- Arrays are covariant: an array of a subtype can be assigned to an array of its supertype, which can lead to runtime `ArrayStoreException`.
- Utility methods: `java.util.Arrays` provides sorting, searching, copying, and conversion to List.

### Strings and immutability
- Strings in Java are immutable; once created, their value cannot be changed. Any operation that modifies a string creates a new String object.
- Immutability provides benefits like thread safety, security (e.g., sensitive data can't be altered), and caching (String pool).
- String concatenation using `+` inside loops can be inefficient due to many intermediate objects; `StringBuilder` or `StringBuffer` should be used.
- String class is `final`, so it cannot be subclassed, preserving its immutability guarantees.
- Methods like `substring()`, `concat()`, `replace()` return new strings, leaving original unchanged.

### String pool
- The String pool is a special area in the heap (or metaspace in some JVMs) that stores literal strings and interned strings to save memory and improve performance.
- When a string literal is created, JVM checks the pool; if exists, returns reference; else creates new string and puts in pool.
- Strings created with `new String("...")` are not automatically placed in the pool; they can be manually interned using `intern()` method.
- The pool is maintained by the String class and is garbage-collected like other objects, though interned strings may have longer lifetime.
- Use of `intern()` can reduce memory footprint for duplicate strings, but can also cause performance overhead due to pool management.

### Wrapper classes
- Wrapper classes (Integer, Long, Float, Double, Byte, Short, Character, Boolean) provide object representations of primitive types.
- They are used in collections (which require objects), generics, and for utility methods (e.g., parsing strings to numbers).
- Wrapper classes are immutable and final; they override `equals()`, `hashCode()`, and `toString()`.
- Each wrapper class has caching for common values (e.g., Integer cache for -128 to 127) to improve performance of autoboxing.
- Conversions: `valueOf()` returns cached instances where possible, while `new Integer()` always creates new object (deprecated in Java 9+).

### Autoboxing and unboxing
- Autoboxing is the automatic conversion of primitive types to their corresponding wrapper classes (e.g., `int` to `Integer`) by the compiler.
- Unboxing is the reverse conversion, from wrapper to primitive.
- These conversions happen in assignments, method arguments, and operations (e.g., arithmetic on wrapper objects triggers unboxing).
- Performance considerations: frequent autoboxing/unboxing in loops can create many temporary objects, impacting performance; prefer primitives for heavy numeric computations.
- Null pointer issues: unboxing a null wrapper throws `NullPointerException`; careful handling required.

### Type casting
- Type casting converts a value from one type to another; implicit casting (widening) happens automatically for compatible primitive types (e.g., int to long).
- Explicit casting (narrowing) is required for converting larger to smaller types, potentially losing data; done with `(targetType) value`.
- Reference type casting: upcasting (subclass to superclass) is automatic; downcasting (superclass to subclass) requires explicit cast and may throw `ClassCastException` if object is not of the target type.
- The `instanceof` operator should be used before downcasting to avoid exceptions.
- Casting with generics involves type erasure, leading to unchecked warnings; best to avoid raw types.

### Pass by value
- Java is strictly pass-by-value: when passing a primitive, a copy of the value is passed; when passing an object reference, a copy of the reference is passed.
- This means that inside a method, reassigning the parameter variable does not affect the original variable outside.
- However, modifications to the object's state via the reference are visible to the caller because both references point to the same object.
- Understanding this is crucial for avoiding confusion in method design and debugging.
- This concept is often tested in interviews; be prepared to explain with examples.

### Java keywords
- Java has about 50 reserved keywords (e.g., `class`, `public`, `static`, `void`, `if`, `else`, `for`, `while`, `return`, `new`, `try`, `catch`, `finally`, `throw`, `throws`, `import`, `package`, `extends`, `implements`, `interface`, `enum`, `synchronized`, `volatile`, `transient`, `native`, `strictfp`, `assert`, `goto` (not used), etc.)
- Keywords have special meaning and cannot be used as identifiers (variable names, class names, etc.)
- `const` and `goto` are reserved but not used; `true`, `false`, `null` are literals, not keywords.
- Newer Java versions have added keywords like `var` (reserved type name), `yield`, `record`, `sealed`, `permits` (contextual keywords).
- Understanding keywords is fundamental to writing correct Java code; misuse leads to compilation errors.

## Object-Oriented Programming

### OOP principles
- The four fundamental OOP principles are Encapsulation, Inheritance, Polymorphism, and Abstraction.
- Encapsulation bundles data and methods that operate on that data, hiding internal state and requiring interaction through public methods.
- Inheritance allows a class to inherit properties and behaviors from a parent class, promoting code reuse and establishing a hierarchical relationship.
- Polymorphism enables objects of different classes to be treated as objects of a common superclass, with methods behaving appropriately based on actual object type (runtime polymorphism).
- Abstraction hides complex implementation details and exposes only essential features, often achieved through abstract classes and interfaces.

### Encapsulation
- Encapsulation is achieved by declaring instance variables as private and providing public getter and setter methods to access and modify them.
- It protects the integrity of data by controlling how it is accessed and updated, allowing validation or other logic in setters.
- Encapsulation helps in achieving loose coupling between components, as internal implementation can change without affecting external code.
- In Java, encapsulation is also supported by access modifiers (private, protected, public) and packages.
- Proper encapsulation reduces complexity and increases maintainability by clearly defining the interface of a class.

### Inheritance
- Inheritance in Java is implemented using the `extends` keyword for classes and `implements` for interfaces.
- A subclass inherits all non-private members (fields, methods) from its superclass, but constructors are not inherited.
- Java supports single inheritance for classes (a class can have only one direct superclass) to avoid diamond problem, but multiple inheritance of type is allowed through interfaces.
- The `super` keyword is used to refer to superclass members and invoke superclass constructors.
- Inheritance can lead to fragile base class problems if not designed carefully; composition is often preferred over inheritance for code reuse.

### Polymorphism
- Polymorphism in Java comes in two forms: compile-time (static) polymorphism via method overloading, and runtime (dynamic) polymorphism via method overriding.
- Runtime polymorphism relies on inheritance and virtual methods (non-static, non-final, non-private methods) to invoke the overridden method based on the actual object type, not reference type.
- Polymorphism allows code to be written against interfaces or abstract classes, making it flexible and extensible.
- It enables the implementation of design patterns like Strategy, Template Method, and Factory.
- Understanding polymorphism is key to leveraging Java's object-oriented capabilities effectively.

### Abstraction
- Abstraction focuses on hiding complex implementation details and showing only the essential features of an object.
- In Java, abstraction can be achieved through abstract classes (which can have abstract and concrete methods) and interfaces (which provide pure abstraction until Java 8 introduced default and static methods).
- Abstract classes are used when there is a common base with some shared implementation; interfaces are used to define a contract that multiple unrelated classes can implement.
- Abstraction helps in reducing code duplication and improving maintainability by separating what an object does from how it does it.
- It also supports modularity and scalability in large systems.

### Method overloading
- Method overloading allows multiple methods in the same class to have the same name but different parameter lists (different type, number, or order).
- Overloading is resolved at compile time based on the method signature (static polymorphism).
- Return type alone is not sufficient to distinguish overloaded methods; the parameter list must differ.
- Overloading is commonly used in constructors and utility methods (e.g., `System.out.println()` overloaded for all primitive types).
- Care must be taken with autoboxing and varargs in overloaded methods, as they can lead to ambiguous calls.

### Method overriding
- Method overriding occurs when a subclass provides a specific implementation of a method already defined in its superclass, with the same name, parameters, and return type (covariant return types allowed).
- The overridden method in the subclass must not have a more restrictive access modifier, and cannot throw broader checked exceptions than the superclass method.
- Overriding is the basis of runtime polymorphism; the `@Override` annotation is recommended to catch errors at compile time.
- Static methods cannot be overridden, only hidden (if subclass defines a static method with same signature, it hides the superclass method).
- Final methods cannot be overridden; private methods are not inherited and thus cannot be overridden.

### Constructors
- Constructors are special methods invoked when an object is created, used to initialize the object's state.
- They have the same name as the class and no return type (not even void).
- If no constructor is defined, the compiler provides a default no-argument constructor.
- Constructors can be overloaded; one constructor can call another using `this()` (must be first statement).
- In inheritance, the subclass constructor must call a superclass constructor, either implicitly (super() added by compiler) or explicitly with `super(...)`.

### Object lifecycle
- The lifecycle of an object in Java includes creation, usage, and destruction.
- Creation: memory allocated on heap, constructor executed, object becomes reachable.
- Usage: object referenced and used; its state may change.
- Destruction: when object becomes unreachable (no live references), it becomes eligible for garbage collection; finalization (deprecated) or cleaner can be used for cleanup before GC.
- The JVM's garbage collector automatically reclaims memory, but objects with non-memory resources (files, connections) should be closed explicitly (try-with-resources).
- Understanding object lifecycle is crucial for managing resources and avoiding memory leaks.

### this vs super
- `this` refers to the current object instance; it can be used to access instance variables, methods, and call other constructors (`this(...)`) in the same class.
- `super` refers to the immediate superclass; it is used to access superclass members (fields, methods) and call superclass constructors (`super(...)`).
- Both `this` and `super` cannot be used in static contexts.
- In case of method overriding, `super.method()` can be used to invoke the superclass version.
- `this` can be passed as an argument to another method or returned from a method to enable method chaining.

### Static members
- Static members (variables, methods, blocks) belong to the class itself, not to any instance; they are shared across all instances.
- Static variables are initialized when the class is first loaded; they have a single copy in memory.
- Static methods can only directly access other static members (cannot access instance variables/methods without an object reference).
- Static blocks are used for static initialization; they execute once when the class is loaded.
- Static members are accessed via class name, though they can also be accessed via instance references (not recommended, as it can be misleading).

### Final keyword
- The `final` keyword can be applied to variables, methods, and classes, with different effects:
  - final variable: cannot be reassigned; for primitives, value fixed; for references, reference cannot point to another object (but object state can change).
  - final method: cannot be overridden by subclasses; useful to prevent modification of critical behavior.
  - final class: cannot be subclassed; used for immutability or security (e.g., String, Integer).
- Final variables must be initialized either at declaration or in constructor (for instance finals) or static initializer (for static finals).
- Using final can help with thread safety and performance (compiler optimizations).

### Access modifiers
- Access modifiers control visibility of classes, methods, and fields: `private`, `protected`, `public`, and package-private (default, no modifier).
- `private`: accessible only within the same class.
- Package-private (default): accessible within the same package.
- `protected`: accessible within same package and by subclasses (even in different packages).
- `public`: accessible from anywhere.
- Proper use of access modifiers enforces encapsulation and defines the API surface.

### Composition vs inheritance
- Composition is a design principle where a class contains references to objects of other classes, representing a "has-a" relationship.
- Inheritance represents an "is-a" relationship; a subclass extends a superclass.
- Composition is often favored over inheritance because it promotes greater flexibility, looser coupling, and easier testing.
- Inheritance can lead to fragile code if the superclass changes; composition uses interfaces to define behavior.
- Both can be used together; for example, using composition with interfaces to achieve polymorphism (strategy pattern).

### Association, aggregation
- Association represents a relationship between two classes where objects of one class know about objects of another class; it can be unidirectional or bidirectional.
- Aggregation is a specialized form of association representing a whole-part relationship where the part can exist independently of the whole (e.g., Department and Employee).
- Composition is a stronger form of aggregation where the part cannot exist without the whole; the whole is responsible for creation/destruction of parts (e.g., House and Room).
- In UML, aggregation is denoted by a hollow diamond, composition by a filled diamond.
- These concepts are important in object-oriented design to model real-world relationships appropriately.

### Object equality (equals/hashCode)
- The `equals()` method determines logical equality of objects; by default, it compares references (same as `==`), so it must be overridden for value-based equality.
- The `hashCode()` method returns an integer hash code, used in hash-based collections (HashMap, HashSet) to efficiently locate objects.
- Contract: if two objects are equal according to `equals()`, they must have the same hashCode; if not equal, they may have same hashCode (but collisions degrade performance).
- Overriding `equals` requires following properties: reflexive, symmetric, transitive, consistent, and non-null comparison.
- Always override `hashCode` when overriding `equals` to maintain the contract; use Objects.hash() or a well-defined algorithm.

### toString contract
- The `toString()` method returns a string representation of the object; by default, it returns class name + @ + hashCode in hex.
- It's recommended to override `toString()` to provide meaningful information about the object's state for debugging and logging.
- There's no formal contract like for equals/hashCode, but it should generally return a concise, informative representation.
- The `toString()` method is implicitly called when an object is concatenated with a string using `+` or passed to `System.out.println()`.
- Override `toString()` in domain objects to improve log readability; consider using Apache Commons ToStringBuilder or IDE-generated code.

## Java Classes & Core APIs

### Object class methods
- The `Object` class is the root of the class hierarchy; all classes inherit its methods: `equals()`, `hashCode()`, `toString()`, `clone()`, `finalize()`, `getClass()`, `wait()`, `notify()`, `notifyAll()`.
- `getClass()` returns the runtime class of the object, which can be used for reflection.
- `clone()` creates a shallow copy of an object; the class must implement `Cloneable` marker interface to avoid `CloneNotSupportedException`.
- `finalize()` is deprecated (Java 9) as it's unpredictable; use `Cleaner` or `PhantomReference` for resource cleanup.
- `wait()`, `notify()`, `notifyAll()` are used for inter-thread communication and must be called from synchronized context.

### String vs StringBuilder vs StringBuffer
- `String` is immutable; any modification creates a new object, making it thread-safe but inefficient for frequent changes.
- `StringBuilder` is mutable and not thread-safe, providing better performance for single-threaded string manipulations.
- `StringBuffer` is mutable and thread-safe (synchronized methods), but slower than StringBuilder due to synchronization overhead.
- Choose `String` for fixed strings, `StringBuilder` for single-threaded concatenation, `StringBuffer` for multi-threaded scenarios (rare).
- All three implement `CharSequence` interface, allowing polymorphic usage.

### Math API
- The `java.lang.Math` class provides static methods for common mathematical operations: `abs()`, `ceil()`, `floor()`, `round()`, `max()`, `min()`, `pow()`, `sqrt()`, `sin()`, `cos()`, `tan()`, `log()`, `exp()`, etc.
- It also contains constants `Math.PI` and `Math.E`.
- Methods like `random()` generate a pseudorandom double between 0.0 and 1.0.
- For more advanced random number generation, use `java.util.Random` or `ThreadLocalRandom`.
- StrictMath ensures reproducible results across platforms, while Math may use platform-specific optimizations.

### Date and time API (Java 8+)
- The `java.time` package (JSR-310) provides a comprehensive date-time API, addressing flaws in legacy `Date` and `Calendar`.
- Key classes: `LocalDate`, `LocalTime`, `LocalDateTime` (no time zone), `ZonedDateTime`, `Instant` (machine timestamp), `Duration`, `Period`.
- The API is immutable and thread-safe, with fluent methods for manipulation.
- It supports time zones via `ZoneId`, formatting/parsing with `DateTimeFormatter`.
- Legacy code can be converted using `Date.from(instant)` and `Date.toInstant()`.

### Optional class
- `Optional<T>` is a container object that may or may not contain a non-null value, used to avoid null checks and `NullPointerException`.
- Common methods: `of()`, `ofNullable()`, `empty()`, `isPresent()`, `ifPresent()`, `orElse()`, `orElseGet()`, `orElseThrow()`, `map()`, `flatMap()`, `filter()`.
- It is not intended to replace all null checks but to indicate that a value may be absent, promoting better API design.
- Avoid using `Optional` for fields, method parameters, or collections (use empty collections instead).
- Overuse can lead to performance overhead and code complexity; use judiciously.

### UUID
- `UUID` (Universally Unique Identifier) is a 128-bit value represented as a hexadecimal string, used for unique identifiers.
- The `java.util.UUID` class provides methods to generate random (type 4) and time-based (type 1) UUIDs via `randomUUID()` and `nameUUIDFromBytes()`.
- UUIDs are immutable and can be parsed from strings using `UUID.fromString()`.
- They are commonly used as primary keys in distributed systems, session IDs, or file names.
- Collisions are extremely unlikely, but not impossible; suitable for most non-security-critical applications.

### Enums
- Enums in Java are special classes that define a fixed set of constants, using the `enum` keyword.
- They are implicitly `final`, extend `java.lang.Enum`, and cannot be instantiated with `new`.
- Enums can have fields, methods, and constructors (private), allowing behavior attached to each constant.
- They provide type safety and can be used in `switch` statements; also have built-in methods like `values()`, `valueOf()`, and `ordinal()`.
- Enums can implement interfaces, making them useful for implementing singleton patterns or strategy patterns.

### Records (Java 14+)
- Records are a special kind of class designed to model immutable data carriers, with a concise syntax: `record Point(int x, int y) {}`.
- They implicitly have a canonical constructor, `equals()`, `hashCode()`, and `toString()` based on all components.
- Records are `final` and cannot be extended; they can implement interfaces but cannot extend other classes.
- They can have additional static fields, methods, and custom constructors (with validation), but cannot have instance fields beyond the components.
- Records reduce boilerplate code and are ideal for DTOs, value objects, and tuple-like structures.

### Annotations
- Annotations provide metadata about code, which can be processed at compile time or runtime via reflection.
- Built-in annotations: `@Override`, `@Deprecated`, `@SuppressWarnings`, `@FunctionalInterface`.
- Meta-annotations: `@Retention` (source, class, runtime), `@Target` (where annotation can be applied), `@Inherited`, `@Documented`.
- Custom annotations can be defined with `@interface` and can have elements (methods) with default values.
- Annotation processing can be done at compile-time using annotation processors (e.g., Lombok) or runtime via reflection.

### Packages
- Packages organize classes into namespaces, preventing naming conflicts and providing access control.
- They correspond to directories in the file system, following naming conventions (reverse domain, e.g., `com.example.myapp`).
- Classes in the same package can access each other's package-private members.
- The `import` statement allows using classes from other packages without fully qualified names.
- Modularization in Java 9+ with modules (JPMS) adds another layer of encapsulation beyond packages.

### Class loading basics
- Class loading is the process of loading class files into memory, performed by the ClassLoader subsystem.
- Java has a hierarchical class loader architecture: Bootstrap (core classes), Extension (optional), and Application/System (classpath).
- Class loaders follow delegation model: when asked to load a class, they delegate to parent first; if parent cannot find, they attempt to load themselves.
- Custom class loaders can be created by extending `ClassLoader` to load classes from non-standard sources (network, database).
- Understanding class loading is important for application servers, dynamic code loading, and avoiding issues like `ClassNotFoundException` or `NoClassDefFoundError`.

## Collections Framework

### Collection hierarchy
- The Java Collections Framework provides a unified architecture for storing and manipulating groups of objects.
- Core interfaces: `Collection` (root interface), `List`, `Set`, `Queue`, `Deque`, and `Map` (separate hierarchy).
- `Collection` extends `Iterable`, providing ability to iterate using for-each loop.
- Main implementations: `ArrayList`, `LinkedList`, `HashSet`, `LinkedHashSet`, `TreeSet`, `ArrayDeque`, `HashMap`, `LinkedHashMap`, `TreeMap`, etc.
- The framework also includes utility classes `Collections` and `Arrays` for sorting, searching, and synchronization.

### List interface
- `List` is an ordered collection (sequence) that allows duplicate elements and provides positional access.
- Main implementations: `ArrayList` (resizable array), `LinkedList` (doubly-linked list), `Vector` (legacy, synchronized), `Stack`.
- Key methods: `add(index, element)`, `get(index)`, `set(index, element)`, `remove(index)`, `indexOf()`, `subList()`.
- `List` supports `ListIterator` for bidirectional traversal.
- Lists can be sorted using `Collections.sort()` or `List.sort()` (since Java 8).

### Set interface
- `Set` is a collection that cannot contain duplicate elements, modeling mathematical set abstraction.
- Main implementations: `HashSet` (hash table, unordered), `LinkedHashSet` (maintains insertion order), `TreeSet` (sorted, based on natural order or comparator).
- `Set` uses `equals()` and `hashCode()` to determine element uniqueness.
- No positional access or ordering guarantees except for specific implementations.
- Useful for eliminating duplicates and membership testing.

### Map interface
- `Map` stores key-value pairs, with each key mapping to at most one value; keys are unique.
- Main implementations: `HashMap` (hash table, unordered), `LinkedHashMap` (maintains insertion or access order), `TreeMap` (sorted by keys), `Hashtable` (legacy, synchronized), `ConcurrentHashMap`.
- Key methods: `put(key, value)`, `get(key)`, `remove(key)`, `containsKey()`, `containsValue()`, `keySet()`, `values()`, `entrySet()`.
- Maps are not part of `Collection` hierarchy but are integral to the framework.
- `Map.Entry` interface represents a key-value pair.

### Queue interface
- `Queue` represents a collection designed for holding elements prior to processing, typically following FIFO order.
- Main implementations: `LinkedList` (also implements `Queue`), `PriorityQueue` (priority heap), `ArrayDeque` (resizable array, double-ended).
- Queue methods: `offer()` (insert), `poll()` (retrieve and remove head, or null if empty), `remove()` (similar but throws exception), `peek()` (retrieve head without removal, or null), `element()` (throws exception).
- `Deque` (double-ended queue) extends `Queue`, allowing insertion/removal at both ends.

### ArrayList internals
- `ArrayList` is a resizable array implementation of `List` interface, backed by an Object array.
- Initial capacity is 10 by default (since Java 8), but can be specified; grows dynamically when needed by creating a new array and copying (using `Arrays.copyOf`).
- Growth factor is roughly 1.5x (oldCapacity + (oldCapacity >> 1)).
- Random access is O(1), but insertion/removal at arbitrary positions is O(n) due to shifting elements.
- Not thread-safe; concurrent modifications can cause `ConcurrentModificationException`; use `CopyOnWriteArrayList` for concurrent scenarios.

### LinkedList internals
- `LinkedList` is a doubly-linked list implementation of `List` and `Deque` interfaces.
- Each element (node) contains data and references to previous and next nodes.
- Insertion/removal at beginning or end is O(1); at arbitrary position requires traversal O(n).
- Implements both `List` and `Deque`, so can be used as stack, queue, or double-ended queue.
- More memory overhead per element compared to `ArrayList` due to node objects.

### HashMap internals
- `HashMap` stores key-value pairs in an array of buckets (Node<K,V>[] table), using hashing to determine bucket index.
- Default initial capacity 16, load factor 0.75; when threshold exceeded, resizes (doubles capacity) and rehashes.
- In case of hash collisions, entries are stored in linked lists (or tree bins since Java 8 for performance when many collisions, converting to balanced tree when list length > 8 and capacity >= 64).
- `hashCode()` of key determines bucket; `equals()` used to find exact key within bucket.
- Not thread-safe; can cause infinite loops in concurrent resize (pre-Java 8) or data corruption; use `ConcurrentHashMap` for concurrency.

### HashSet internals
- `HashSet` is implemented using a `HashMap` internally, where each element is stored as a key in the map with a constant dummy value (present).
- It uses the key's `hashCode()` and `equals()` for uniqueness.
- Operations like `add()`, `remove()`, `contains()` delegate to the underlying `HashMap`.
- Order is not guaranteed; iteration order may change over time.
- `LinkedHashSet` extends `HashSet` but uses `LinkedHashMap` to maintain insertion order.

### TreeMap and TreeSet
- `TreeMap` is a Red-Black tree based implementation of `NavigableMap`, storing keys in sorted order (natural ordering or by provided `Comparator`).
- `TreeSet` is backed by a `TreeMap`, with elements as keys, providing sorted set.
- Operations like `put()`, `get()`, `remove()` have O(log n) time complexity.
- They implement `NavigableSet`/`NavigableMap` offering methods like `lower()`, `floor()`, `ceiling()`, `higher()` for range queries.
- Not thread-safe; can be wrapped with `Collections.synchronizedSortedMap()`.

### Comparable vs Comparator
- `Comparable` is an interface (java.lang) that allows an object to compare itself to another of the same type, defining natural ordering via `compareTo()` method.
- `Comparator` is an interface (java.util) that compares two objects of possibly different types, allowing custom ordering strategies.
- If a class implements `Comparable`, its objects can be sorted automatically (e.g., `Collections.sort(list)`).
- `Comparator` can be passed to sort methods to override natural order or for classes not implementing `Comparable`.
- Both should be consistent with `equals()` to avoid unexpected behavior in sorted collections.

### Sorting mechanisms
- Sorting in collections can be done using `Collections.sort()` (for lists) or `Arrays.sort()` (for arrays).
- `Collections.sort()` internally uses `Arrays.sort()` which for objects uses TimSort (a hybrid stable sort) or legacy MergeSort.
- For primitive arrays, `Arrays.sort()` uses dual-pivot Quicksort (for ints, etc.) for performance.
- Custom sorting can be achieved by providing a `Comparator`.
- Sorting stability: stable sorts preserve the relative order of equal elements; TimSort is stable.

### Iterators
- Iterators allow traversal of collections without exposing underlying structure.
- `Iterator` interface provides `hasNext()`, `next()`, and `remove()` (optional).
- `ListIterator` extends `Iterator` for bidirectional traversal and modification of lists.
- Enhanced for-each loop uses `Iterator` behind the scenes; any class implementing `Iterable` can be used in for-each.
- Iterators are fail-fast: if collection is structurally modified after iterator creation (except via iterator's own `remove()`), they throw `ConcurrentModificationException`.

### Fail-fast vs fail-safe
- Fail-fast iterators detect concurrent modification and throw `ConcurrentModificationException` to prevent undefined behavior (e.g., iterators of `ArrayList`, `HashMap`).
- Fail-safe iterators operate on a snapshot of the collection or allow modification without throwing exceptions, typically used in concurrent collections (e.g., `CopyOnWriteArrayList`, `ConcurrentHashMap` iterators).
- Fail-safe iterators do not throw `ConcurrentModificationException` but may not reflect the latest changes.
- The distinction is important in multi-threaded environments to choose appropriate collections.
- Note: The term "fail-safe" is sometimes used loosely; in Java, concurrent collections provide weakly consistent iterators.

### Concurrent collections
- Java provides thread-safe collections in `java.util.concurrent` package for high concurrency.
- Examples: `ConcurrentHashMap` (highly concurrent, lock striping), `CopyOnWriteArrayList` (for read-heavy lists), `ConcurrentLinkedQueue` (non-blocking queue), `BlockingQueue` implementations (`ArrayBlockingQueue`, `LinkedBlockingQueue`).
- These collections use advanced techniques like lock-free algorithms, volatile variables, and CAS (compare-and-swap) to achieve thread-safety with better performance than synchronized wrappers.
- They are designed for concurrent access and provide weakly consistent iterators.
- Choosing the right concurrent collection is crucial for scalable multi-threaded applications.

### Immutable collections
- Immutable collections cannot be modified after creation, providing thread safety and predictability.
- In Java 9+, `List.of()`, `Set.of()`, `Map.of()` create immutable collections (unmodifiable, not just views).
- Prior to Java 9, `Collections.unmodifiableList()` etc. create unmodifiable views of existing collections, but the backing collection can still change.
- Immutable collections do not allow `null` elements and throw `UnsupportedOperationException` on modification attempts.
- They are useful for defining constants and sharing data without defensive copying.

### Collection performance tradeoffs
- `ArrayList`: O(1) random access, O(n) insertion/removal in middle; good for frequent reads and appends.
- `LinkedList`: O(n) random access, O(1) insertion/removal at ends; better for frequent modifications at ends.
- `HashSet`/`HashMap`: O(1) average for add/remove/contains, assuming good hash distribution; but higher memory overhead.
- `TreeSet`/`TreeMap`: O(log n) operations, sorted, but slower than hash-based.
- Choosing the right collection depends on access patterns, concurrency requirements, and memory constraints.

## Exception Handling

### Exception hierarchy
- All exceptions and errors are subclasses of `Throwable` (java.lang).
- Two main branches: `Exception` (for recoverable conditions) and `Error` (for serious problems, typically not handled).
- `RuntimeException` and its subclasses are unchecked exceptions; all other `Exception` subclasses are checked.
- Common checked exceptions: `IOException`, `SQLException`, `ClassNotFoundException`.
- Common unchecked exceptions: `NullPointerException`, `IllegalArgumentException`, `IndexOutOfBoundsException`, `ArithmeticException`.

### Checked vs unchecked exceptions
- Checked exceptions are checked at compile time; the method must either handle them (try-catch) or declare them in its throws clause.
- Unchecked exceptions (runtime exceptions and errors) are not checked at compile time; they can be left unhandled (though they may cause program termination).
- Checked exceptions represent conditions outside the program's control (e.g., file not found), while unchecked typically indicate programming bugs (e.g., null pointer).
- Overuse of checked exceptions can lead to cluttered code; many modern frameworks prefer unchecked exceptions.
- Decision to use checked vs unchecked should be based on whether the caller can reasonably recover.

### try-catch-finally
- `try` block encloses code that may throw exceptions.
- `catch` blocks handle specific exception types; multiple catch blocks can be used (order matters: more specific first).
- `finally` block executes regardless of whether an exception is thrown or caught, typically for cleanup (closing resources).
- Since Java 7, try-with-resources automatically closes resources that implement `AutoCloseable`, reducing need for finally.
- `catch` can also catch multiple exceptions in one block using multi-catch (e.g., `catch (IOException | SQLException e)`).

### throw vs throws
- `throw` is used to explicitly throw an exception from a method or block (e.g., `throw new IllegalArgumentException("msg")`).
- `throws` is used in method signature to declare that the method may throw certain checked exceptions (e.g., `void readFile() throws IOException`).
- `throw` transfers control to the nearest catch block; `throws` is part of the method contract for callers.
- A method can `throw` multiple exceptions, but only need to declare them if they are checked.
- Runtime exceptions can be thrown without declaration.

### Custom exceptions
- Custom exceptions are created by extending `Exception` (for checked) or `RuntimeException` (for unchecked).
- Typically, include constructors that mimic superclass constructors (e.g., with message, cause).
- Can add custom fields and methods to provide additional error information.
- Should follow naming convention: class name ending with "Exception".
- Use custom exceptions to represent domain-specific error conditions, improving code clarity and maintainability.

### Exception propagation
- When an exception is thrown and not caught in the current method, it propagates up the call stack to the calling method, and so on.
- If no catch block handles it, the program terminates (for main thread) and prints stack trace.
- Checked exceptions must be either caught or declared in every method along the propagation chain.
- Propagation can be controlled by rethrowing exceptions, possibly after logging or wrapping.
- Understanding propagation helps in designing error handling strategies.

### Best practices
- Catch specific exceptions rather than generic `Exception` or `Throwable`.
- Avoid catching `Error` or `RuntimeException` unless you have a specific reason.
- Use try-with-resources for auto-closable resources.
- Log exceptions with appropriate details (stack trace, context) for debugging.
- Don't ignore exceptions with empty catch blocks; at least log them.
- Throw exceptions early (fail-fast) and handle them late at appropriate layers.
- Consider using unchecked exceptions for programming errors and checked for recoverable conditions.

## Java Generics

### Generic classes
- Generic classes allow type parameters, enabling type-safe operations on various types without casting.
- Syntax: `class Box<T> { private T t; ... }` where T is a type parameter.
- Multiple type parameters: `class Pair<K, V> { ... }`.
- Type parameters can be used for fields, method return types, and parameter types.
- Instantiations: `Box<String> box = new Box<>();` (diamond operator infers type).
- Generic classes are checked at compile time for type consistency.

### Generic methods
- Generic methods introduce their own type parameters, independent of class generics.
- Syntax: `public <T> T method(T t) { ... }` (type parameter before return type).
- Can be static or non-static; can have multiple bounds.
- Type inference: the compiler deduces type arguments from method invocation.
- Useful for utility methods (e.g., `Collections.sort()` with generic type).

### Type erasure
- Generics are implemented via type erasure: generic type information is removed at compile time and replaced with bounds or Object.
- This ensures backward compatibility with pre-generics code (raw types).
- After erasure, `List<String>` becomes `List` (raw), and type casts are inserted where needed.
- Bridge methods may be generated to preserve polymorphism for overriding.
- Type erasure has implications: cannot use instanceof with parameterized types, cannot create arrays of parameterized types, cannot use primitive type arguments.

### Bounded types
- Bounded type parameters restrict the types that can be used as type arguments.
- Upper bound: `<T extends Number>` means T must be Number or its subclass; can also specify interfaces with `&`.
- Lower bound: not directly on type parameter, but used in wildcards (`? super Integer`).
- Multiple bounds: `<T extends Number & Serializable>`; class first, then interfaces.
- Bounded types allow calling methods defined in the bound.

### Wildcards
- Wildcards (`?`) represent unknown types in generics, used in method signatures and field declarations.
- Unbounded wildcard: `List<?>` means list of unknown type; can only read as Object, cannot add (except null).
- Upper bounded wildcard: `List<? extends Number>` can read as Number, cannot add (except null) because actual type could be subclass.
- Lower bounded wildcard: `List<? super Integer>` can add Integer or its subclasses, but reading yields Object.
- Wildcards provide flexibility for API design, enabling covariance and contravariance.

### Covariance and contravariance
- Covariance: allows a subtype to be used where a supertype is expected (e.g., `List<? extends Number>` is covariant for reading).
- Contravariance: allows a supertype to be used where a subtype is expected (e.g., `List<? super Integer>` is contravariant for writing).
- Arrays are covariant: `Number[] numArr = new Integer[10];` but this can cause `ArrayStoreException`.
- Generics are invariant by default: `List<Number>` is not a subtype of `List<Integer>`.
- Wildcards enable covariance and contravariance for generics, providing type safety.

### PECS principle
- PECS stands for "Producer Extends, Consumer Super" (from Effective Java by Joshua Bloch).
- When a collection is a producer of elements (you read from it), use `? extends T` to accept any subtype of T.
- When a collection is a consumer of elements (you write to it), use `? super T` to accept any supertype of T.
- If both reading and writing, do not use wildcard; use exact type.
- This principle ensures type safety and flexibility when using wildcards in APIs.

## Java I/O & NIO

### File handling
- File handling in Java traditionally uses `java.io.File` class (represents file/directory path) but has limitations (e.g., lack of symbolic link support, error handling).
- Since Java 7, `java.nio.file.Path` and `Files` classes provide more robust file operations.
- Common operations: creating, deleting, moving, copying files; checking permissions; reading/writing attributes.
- `Files` utility class has methods like `readAllLines()`, `write()`, `copy()`, `move()`, `delete()`, `exists()`.
- Use try-with-resources for streams to ensure proper closure.

### Streams API (I/O streams)
- I/O streams are sequences of data, either byte-oriented (`InputStream`, `OutputStream`) or character-oriented (`Reader`, `Writer`).
- Streams are used for reading/writing data from/to sources like files, network sockets, memory.
- They are blocking I/O by default; for non-blocking, use NIO channels.
- Streams can be chained with decorators (e.g., `BufferedInputStream` adds buffering).
- Key methods: `read()`, `write()`, `close()`, `flush()`.

### Byte streams vs character streams
- Byte streams handle raw binary data (bytes); classes end with `Stream` (e.g., `FileInputStream`, `ByteArrayOutputStream`).
- Character streams handle text data, converting bytes to characters using charset encoding; classes end with `Reader`/`Writer` (e.g., `FileReader`, `BufferedWriter`).
- Character streams are built on top of byte streams with encoding/decoding.
- For text files, character streams are recommended; for images, binary data, byte streams.
- Always specify charset explicitly (e.g., `StandardCharsets.UTF_8`) to avoid platform-dependent behavior.

### Buffered streams
- Buffered streams wrap unbuffered streams to reduce the number of I/O operations by using an internal buffer.
- `BufferedInputStream` and `BufferedOutputStream` for bytes; `BufferedReader` and `BufferedWriter` for characters.
- They improve performance by reading/writing larger chunks at once.
- Important to `flush()` buffered output streams to ensure data is written.
- `BufferedReader` provides `readLine()` for convenient line-by-line reading.

### Serialization
- Serialization converts an object into a byte stream to save to file, send over network, etc.; deserialization reconstructs object.
- A class must implement `Serializable` (marker interface) to be serialized.
- `transient` fields are not serialized; `static` fields belong to class, not object, so not serialized.
- Default serialization can be customized by implementing `writeObject()` and `readObject()` methods.
- `serialVersionUID` is a version control identifier; if not declared, JVM generates one, which can cause `InvalidClassException` if class changes.

### Externalization
- `Externalizable` interface extends `Serializable`, giving full control over serialization process via `writeExternal()` and `readExternal()` methods.
- It requires a public no-arg constructor for deserialization.
- Externalization can improve performance by allowing custom format and selective serialization.
- Unlike default serialization, externalization does not automatically write object fields; you must explicitly write/read them.
- Use when default serialization is insufficient or too slow.

### NIO overview
- Java NIO (New I/O, introduced in Java 1.4, enhanced in Java 7 with NIO.2) provides non-blocking I/O, buffers, channels, and selectors.
- NIO.2 added improved file system APIs (`Path`, `Files`, `FileSystem`), asynchronous I/O, and file system operations.
- NIO is more scalable for high-concurrency applications (e.g., servers) due to non-blocking capabilities.
- Core concepts: Channels (connections to entities like files, sockets), Buffers (data containers), Selectors (multiplexing of channels).
- NIO also includes memory-mapped files and file locking.

### Channels and buffers
- Channels are similar to streams but bidirectional; can read and write concurrently (e.g., `FileChannel`, `SocketChannel`).
- Buffers are fixed-size blocks of memory used to transfer data between channel and application.
- Buffer types: `ByteBuffer`, `CharBuffer`, `IntBuffer`, etc.; have properties: capacity, position, limit, mark.
- Data flow: read from channel into buffer, then process buffer; or fill buffer and write to channel.
- Channels can operate in non-blocking mode, allowing efficient thread usage.

### File system APIs (NIO.2)
- `Path` interface represents a file or directory path; created via `Paths.get()` or `FileSystem.getPath()`.
- `Files` class provides static methods for file operations: `copy()`, `move()`, `delete()`, `createDirectory()`, `walkFileTree()`, `find()`, `lines()`.
- `FileSystem` represents the underlying file system; can access file stores, root directories.
- Supports symbolic links, file attributes (e.g., `BasicFileAttributes`), and file change notifications via `WatchService`.
- These APIs are more robust and feature-rich than legacy `java.io.File`.

### Memory-mapped files
- Memory-mapped files allow direct reading/writing of file data as if it were in memory, using virtual memory.
- Achieved via `FileChannel.map()` returning `MappedByteBuffer`.
- Useful for large files and high-performance I/O, as data is loaded lazily and can be shared between processes.
- Changes to the buffer are eventually written to disk; can be forced with `force()`.
- Potential issues: file locking, size limitations, and platform-specific behavior.

## Multithreading & Concurrency

### Thread lifecycle
- Thread states: NEW (created but not started), RUNNABLE (executing or ready to run), BLOCKED (waiting for monitor lock), WAITING (waiting indefinitely), TIMED_WAITING (waiting for specified time), TERMINATED (completed).
- Transitions: `start()` moves from NEW to RUNNABLE; scheduler decides running.
- `wait()`, `notify()`, `join()`, `sleep()` cause state changes.
- `interrupt()` can wake a thread from waiting/sleeping, setting interrupt flag.
- Understanding lifecycle helps in debugging concurrency issues.

### Runnable vs Callable
- `Runnable` is a functional interface with `run()` method (no return value, cannot throw checked exceptions).
- `Callable<V>` is a functional interface with `call()` method (returns a value, can throw checked exceptions).
- `Callable` is used with `ExecutorService.submit()` returning `Future`.
- `Runnable` tasks can be adapted to `Callable` using `Executors.callable()`.
- Prefer `Callable` when result or exception handling is needed.

### Thread creation
- Two ways: extend `Thread` class and override `run()`; or implement `Runnable` and pass to `Thread` constructor.
- Implementing `Runnable` is preferred because it separates task from execution and allows extending other classes.
- Starting a thread: call `start()` (not `run()`, which executes in current thread).
- Threads can also be created via `ExecutorService` for better management.
- Daemon threads (setDaemon(true)) run in background and terminate when all user threads finish.

### Synchronization
- Synchronization controls access to shared resources by multiple threads to prevent data inconsistency.
- Achieved using `synchronized` keyword on methods or blocks, using an intrinsic lock (monitor).
- Synchronized methods lock on the instance (for instance methods) or class (for static methods).
- Synchronized blocks allow locking on any object, providing finer control.
- Synchronization ensures visibility and atomicity but can cause contention and deadlocks.

### Intrinsic locks
- Every Java object has an intrinsic lock (monitor) associated with it.
- When a thread enters a synchronized method/block, it acquires the lock; other threads block until release.
- Locks are reentrant: a thread holding a lock can acquire it again without deadlock.
- Intrinsic locks are simple but lack flexibility (no timeout, interruptible acquisition).
- For advanced locking, use `java.util.concurrent.locks` package.

### Deadlocks
- Deadlock occurs when two or more threads are blocked forever, each waiting for a lock held by another.
- Necessary conditions: mutual exclusion, hold and wait, no preemption, circular wait.
- Prevention strategies: lock ordering, timeout, using tryLock, avoiding nested locks, using higher-level concurrency utilities.
- Detection: tools like jstack, thread dumps, visual VM.
- Deadlocks are hard to debug; design to avoid them.

### Race conditions
- Race condition occurs when the outcome of a program depends on the timing of thread execution.
- Common in read-modify-write operations (e.g., counter increment) without synchronization.
- Can lead to data corruption and unpredictable behavior.
- Prevention: use synchronization, atomic variables, or immutable data.
- Even with synchronization, race conditions can exist if not all accesses are protected.

### Volatile keyword
- `volatile` ensures that reads and writes to a variable are directly from/to main memory, not thread-local caches.
- It provides visibility guarantee: changes by one thread are visible to others immediately.
- Does not provide atomicity (e.g., increment operation still needs synchronization).
- Useful for flags, state variables, and double-checked locking (with proper semantics).
- Since Java 5, volatile has acquire/release semantics.

### Thread safety
- A class is thread-safe if it behaves correctly when accessed from multiple threads.
- Achieved through immutability, synchronization, or using thread-safe classes (e.g., `ConcurrentHashMap`).
- Stateless objects are inherently thread-safe.
- Thread-safe classes can be composed with care; need to ensure atomicity of compound actions.
- Document thread-safety guarantees.

### Executor framework
- The `Executor` framework decouples task submission from execution.
- `Executor` interface has `execute(Runnable)`. `ExecutorService` extends with `submit()`, `shutdown()`, etc.
- Provides thread pool management, task scheduling, and future results.
- Common implementations: `ThreadPoolExecutor`, `ScheduledThreadPoolExecutor`, `ForkJoinPool`.
- Simplifies concurrent programming and resource management.

### Thread pools
- Thread pools manage a pool of worker threads, reusing them for multiple tasks to reduce overhead.
- Fixed thread pool: `Executors.newFixedThreadPool(n)`.
- Cached thread pool: `Executors.newCachedThreadPool()` creates threads as needed, reuses idle ones.
- Single thread executor: `Executors.newSingleThreadExecutor()` ensures sequential execution.
- Scheduled thread pool: `Executors.newScheduledThreadPool()` for delayed/periodic tasks.
- Proper sizing: consider CPU cores, I/O vs CPU-bound tasks.

### Future and CompletableFuture
- `Future` represents the result of an asynchronous computation, with methods to check completion, wait, and retrieve result.
- Limitations: blocking `get()`, no chaining, no manual completion.
- `CompletableFuture` (Java 8) implements `Future` and `CompletionStage`, providing non-blocking operations, chaining, and combining.
- Supports callbacks (`thenApply`, `thenAccept`, `thenRun`), combining (`thenCombine`), error handling (`exceptionally`), and composing.
- Used for asynchronous programming and parallel tasks.

### Locks and ReentrantLock
- `Lock` interface in `java.util.concurrent.locks` provides more flexible locking than intrinsic locks.
- `ReentrantLock` is a reentrant mutual exclusion Lock with features: tryLock (with timeout), lockInterruptibly, fairness policy.
- Requires explicit lock/unlock, usually in try-finally block.
- Condition objects (from `newCondition()`) allow wait/notify-like behavior with multiple wait sets.
- Prefer `ReentrantLock` when advanced features needed; otherwise, synchronized is simpler.

### ReadWriteLock
- `ReadWriteLock` maintains a pair of locks: one for read-only operations, one for writes.
- Multiple threads can hold read lock concurrently; write lock is exclusive.
- Useful for data structures with frequent reads and occasional writes, improving concurrency.
- `ReentrantReadWriteLock` is the primary implementation.
- Beware of lock downgrading (from write to read) and potential starvation.

### Concurrent collections
- Already covered earlier but important for concurrency: `ConcurrentHashMap`, `CopyOnWriteArrayList`, `ConcurrentLinkedQueue`, `BlockingQueue` implementations.
- They provide thread-safe operations without explicit synchronization.
- `ConcurrentHashMap` uses lock striping and volatile semantics.
- `CopyOnWriteArrayList` creates a new copy on modification, suitable for read-heavy scenarios.
- `BlockingQueue` supports producer-consumer patterns with blocking put/take.

### Atomic variables
- `java.util.concurrent.atomic` package provides classes like `AtomicInteger`, `AtomicLong`, `AtomicReference` for lock-free thread-safe operations on single variables.
- They use CAS (Compare-And-Swap) instructions at hardware level for high performance.
- Operations like `incrementAndGet()`, `compareAndSet()`, `updateAndGet()`.
- Avoids synchronization overhead and provides better scalability.
- Useful for counters, accumulators, and non-blocking algorithms.

### Fork/Join framework
- Fork/Join framework (Java 7) is designed for parallel processing of tasks that can be recursively split.
- Core classes: `ForkJoinPool`, `ForkJoinTask`, `RecursiveTask` (with result), `RecursiveAction` (no result).
- Uses work-stealing algorithm: idle threads steal tasks from busy threads' queues to improve load balancing.
- Ideal for divide-and-conquer problems like parallel sorting, matrix multiplication.
- Parallel streams (Java 8) use Fork/Join under the hood.

### Parallel streams
- Parallel streams enable concurrent processing of stream elements using `parallel()` or `parallelStream()`.
- They leverage the common ForkJoinPool to execute operations in parallel.
- Useful for CPU-intensive tasks with large data sets; but overhead may outweigh benefits for small data or I/O-bound tasks.
- Must ensure that operations are stateless, non-interfering, and associative to avoid concurrency issues.
- Parallel streams can cause performance degradation if not used carefully; monitor with benchmarks.

### Producer-consumer pattern
- Classic concurrency pattern where producers generate data and consumers process it, often with a shared buffer.
- Implemented using `BlockingQueue` (e.g., `ArrayBlockingQueue`) which handles synchronization automatically.
- Producer calls `put()` (blocks if queue full); consumer calls `take()` (blocks if empty).
- Alternative: use `wait()` and `notifyAll()` with explicit synchronization, but `BlockingQueue` simplifies.
- Important to handle interruption and shutdown properly.

## Java Memory Model & JVM Internals

### JVM architecture
- JVM consists of class loader subsystem, runtime data areas (heap, stack, PC registers, method area, native method stack), execution engine, and native interface.
- Runtime data areas: heap (shared), method area (shared), Java stacks (per thread), PC registers (per thread), native method stacks (per thread).
- Execution engine includes interpreter, JIT compiler, and garbage collector.
- JVM specifications are implemented by various vendors (HotSpot, OpenJ9, etc.).
- Understanding architecture aids in performance tuning and troubleshooting.

### Heap vs stack
- Heap is the runtime data area where objects and arrays are allocated; shared among all threads.
- Stack is per-thread, storing frames (local variables, partial results, method calls). Each frame has operand stack and local variable array.
- Stack memory is faster, but limited; heap is larger but slower and managed by GC.
- Stack stores primitive local variables and object references; objects themselves are in heap.
- StackOverflowError occurs when stack depth exceeded; OutOfMemoryError when heap full.

### Method area
- Method area (part of heap in HotSpot) stores class-level data: runtime constant pool, field and method data, constructor and method code, special methods.
- It is shared among all threads.
- Before Java 8, method area was part of PermGen (permanent generation); now replaced by Metaspace.
- Contains static variables, which are references to objects (objects themselves on heap).

### Metaspace
- Metaspace (introduced in Java 8) replaces PermGen for storing class metadata.
- Unlike PermGen, Metaspace is not contiguous to heap; it uses native memory, sized by operating system's available memory.
- This change reduces the risk of `OutOfMemoryError: PermGen space`.
- Metaspace size can be controlled with `-XX:MaxMetaspaceSize`.
- Class metadata is garbage collected when classes are unloaded.

### Class loaders
- Class loaders are responsible for loading class files into memory.
- Three built-in class loaders: Bootstrap (loads core Java classes), Platform/Extension (loads extensions), Application/System (loads from classpath).
- Delegation model: a class loader delegates loading to parent before trying itself.
- Custom class loaders can be created for dynamic loading (e.g., web servers).
- Class loading is dynamic and lazy (classes loaded when first referenced).

### Garbage collection basics
- Garbage collection automatically reclaims memory occupied by unreachable objects.
- Objects become unreachable when no references point to them (from roots: stack variables, static fields, JNI references).
- GC roots include active threads, static fields, local variables, JNI references.
- GC algorithms involve marking reachable objects, and sweeping/compacting unreachable ones.
- Tuning GC is critical for application performance.

### GC algorithms
- Common algorithms: Serial (single-threaded, stop-the-world), Parallel (multithreaded), CMS (concurrent mark-sweep, deprecated), G1 (garbage-first), ZGC (low-latency), Shenandoah.
- Serial GC suitable for small apps; Parallel for throughput; G1 for balanced; ZGC/Shenandoah for low latency.
- Each has trade-offs in throughput, latency, and footprint.
- GC logs and monitoring tools help choose appropriate algorithm.

### G1, ZGC, Shenandoah
- G1 (Garbage First) is the default since Java 9, divides heap into regions, prioritizes garbage collection in regions with most garbage, aims for low pause times.
- ZGC (Java 11+) is a scalable low-latency collector, handles heaps up to multi-terabytes, pause times <10ms, uses colored pointers and load barriers.
- Shenandoah (Java 12+, not in all builds) also low-latency, concurrent evacuation, reduces pause times.
- These collectors are designed for applications requiring predictable and low pause times.

### Memory leaks
- In Java, memory leaks occur when objects are no longer needed but still referenced, preventing GC.
- Common causes: static collections accumulating objects, unclosed resources, listeners not deregistered, ThreadLocal misuse, cache without weak references.
- Memory leaks degrade performance and lead to OutOfMemoryError.
- Detection: profiling tools (VisualVM, JProfiler), heap dumps, GC logs.
- Prevention: use weak references where appropriate, clear collections, close resources, etc.

### Object allocation
- Objects are typically allocated on heap (eden space) using TLAB (Thread Local Allocation Buffer) for performance.
- Escape analysis (since Java 6) can determine if an object is not shared, allowing allocation on stack or even scalar replacement (object fields as local variables), reducing GC pressure.
- Allocation can also happen in old generation if object is large (direct to old).
- Object allocation is fast due to bump-pointer allocation and TLABs.

### Escape analysis
- Escape analysis is an optimization technique that analyzes object scope.
- If an object does not escape method or thread, JIT can allocate it on stack instead of heap, or even eliminate it (scalar replacement).
- This reduces GC overhead and improves performance.
- Enabled by default in modern JVMs; controlled with `-XX:+DoEscapeAnalysis`.
- It's a compiler optimization, not visible at source level.

### JIT compilation
- Just-In-Time (JIT) compiler compiles bytecode to native machine code at runtime for frequently executed code (hot spots).
- Two main compilers in HotSpot: C1 (client, quick) and C2 (server, more optimized); tiered compilation uses both.
- JIT improves performance significantly by adaptive optimization.
- It profiles code to identify bottlenecks and applies inlining, loop unrolling, etc.
- JIT-compiled code is stored in code cache.

### Bytecode execution
- Bytecode can be executed by interpreter (slow) or JIT-compiled (fast).
- Initially, interpreter runs, collecting profiling data; hot methods are JIT-compiled.
- Stack-based architecture: operations use operand stack.
- Bytecode instructions are simple (e.g., iload, iadd, invokevirtual).
- Execution engine ensures type safety and stack integrity.

### JVM tuning basics
- JVM tuning involves adjusting heap size, GC algorithm, and other parameters for optimal performance.
- Common options: `-Xms` (initial heap), `-Xmx` (max heap), `-XX:+UseG1GC`, `-XX:MaxGCPauseMillis`.
- Monitor GC logs, heap usage, thread dumps to identify issues.
- Tuning aims to balance throughput, latency, and memory footprint.
- Over-tuning can be counterproductive; start with defaults and adjust based on profiling.

## Functional Programming & Streams

### Lambda expressions
- Lambda expressions provide a concise way to represent anonymous functions, enabling functional programming in Java.
- Syntax: `(parameters) -> expression` or `(parameters) -> { statements; }`.
- They can be used where a functional interface is expected.
- Lambdas capture effectively final variables from enclosing scope.
- Improve code readability and enable behavior parameterization.

### Functional interfaces
- A functional interface has exactly one abstract method (SAM), may have default and static methods.
- Annotated with `@FunctionalInterface` for compile-time check.
- Common functional interfaces: `Predicate<T>`, `Function<T,R>`, `Consumer<T>`, `Supplier<T>`, `Comparator<T>`, `Runnable`, `Callable`.
- Java 8 provides many in `java.util.function` package.
- Lambdas can be assigned to functional interface types.

### Method references
- Method references are shorthand for lambdas calling a specific method.
- Four types: static method (`Class::staticMethod`), instance method of a particular object (`instance::method`), instance method of any object of a particular type (`Class::instanceMethod`), constructor (`Class::new`).
- They are more concise and often more readable.
- Can be used where lambda would just call an existing method.

### Stream API
- Stream API (java.util.stream) enables functional-style operations on sequences of elements.
- Streams are not data structures; they pull data from sources (collections, arrays, I/O channels).
- Operations are divided into intermediate (lazy, return new stream) and terminal (eager, produce result).
- Streams can be sequential or parallel.
- Designed for internal iteration, allowing optimizations like lazy evaluation and short-circuiting.

### Intermediate operations
- Intermediate operations transform a stream into another stream, e.g., `filter()`, `map()`, `flatMap()`, `distinct()`, `sorted()`, `peek()`, `limit()`, `skip()`.
- They are lazy; execution only occurs when a terminal operation is invoked.
- `map()` applies a function to each element; `flatMap()` flattens nested structures.
- `filter()` selects elements based on predicate.
- Intermediate operations can be chained.

### Terminal operations
- Terminal operations produce a result or side effect, e.g., `forEach()`, `collect()`, `toArray()`, `reduce()`, `count()`, `anyMatch()`, `allMatch()`, `noneMatch()`, `findFirst()`, `findAny()`.
- After terminal operation, the stream is consumed and cannot be reused.
- `reduce()` performs associative aggregation.
- `collect()` accumulates elements into a collection or other result using `Collectors`.

### Collectors
- `Collectors` utility class provides implementations for `collect()`.
- Common collectors: `toList()`, `toSet()`, `toMap()`, `joining()`, `groupingBy()`, `partitioningBy()`, `summarizingInt()`, `averagingInt()`.
- Custom collectors can be created using `Collector.of()`.
- `groupingBy()` classifies elements by a classifier function.
- `partitioningBy()` splits into two groups based on predicate.

### Parallel streams
- Parallel streams use common ForkJoinPool to process elements concurrently.
- Enable via `parallelStream()` or `stream().parallel()`.
- Suitable for CPU-intensive tasks with large data; but overhead of splitting and combining can degrade performance for small data or non-independent operations.
- Operations must be stateless, non-interfering, and associative.
- Use `forEachOrdered()` to preserve encounter order when needed.

### Stream performance
- Streams can have overhead due to lambda creation, boxing, and pipeline setup.
- For simple operations on small data, traditional loops may be faster.
- Parallel streams can speed up large data processing if operations are CPU-bound and data can be split evenly.
- Use primitive streams (`IntStream`, `LongStream`, `DoubleStream`) to avoid boxing overhead.
- Profiling and benchmarking (JMH) recommended to evaluate performance.

### Optional usage
- `Optional` is used to represent potentially absent values in stream operations.
- Terminal operations like `findFirst()` return `Optional`.
- Use `ifPresent()`, `orElse()`, `orElseGet()` to handle optional results gracefully.
- Avoid using `Optional` as method parameters or fields; it's designed for return types.
- In streams, `Optional` can be chained with methods like `map()` and `filter()`.

## Reflection & Class Loading

### Reflection API
- Reflection allows examining or modifying runtime behavior of classes, interfaces, fields, and methods.
- Key classes: `Class`, `Field`, `Method`, `Constructor`, `Modifier`, `Array`.
- Can access private members (via `setAccessible(true)`), but security manager may restrict.
- Used in frameworks (Spring, Hibernate) for dependency injection, ORM, and serialization.
- Performance overhead due to runtime type checking; avoid in performance-critical code.

### Dynamic class loading
- Dynamic class loading loads classes at runtime using `Class.forName()` or class loaders.
- `Class.forName(String className)` loads class and returns `Class` object, initializing it.
- Can also load with custom class loader.
- Used in plugin architectures, JDBC drivers (`Class.forName("com.mysql.jdbc.Driver")`).
- Enables runtime flexibility but adds complexity.

### ClassLoader hierarchy
- As mentioned earlier, class loaders follow delegation model.
- Understanding custom class loaders is essential for application servers and modular applications.
- `Thread.currentThread().getContextClassLoader()` often used to load classes in web apps.
- Class loaders can define packages and resolve classes from different sources.
- ClassLoader leaks can occur if classes are not unloaded, often in PermGen/Metaspace.

### Annotations processing
- Annotation processing can occur at compile-time (via annotation processors) or runtime (via reflection).
- Compile-time processing: apt tool (old) or `javac` with processors, generating source files, etc.
- Runtime processing: reflection to read annotations and take actions.
- Popular libraries use runtime processing (e.g., JUnit, Spring) or compile-time (Lombok).
- `@Retention` policy determines if annotation is available at runtime.

### Runtime introspection
- Introspection is the ability to examine object properties and methods at runtime.
- Reflection provides introspection capabilities, e.g., `getFields()`, `getMethods()`, `getDeclaredAnnotations()`.
- JavaBeans introspection uses `Introspector` to discover properties.
- Used in frameworks for dynamic behavior.
- Introspection can be slow; caching results can mitigate.

### Dynamic proxies
- Dynamic proxies allow creation of proxy instances for interfaces at runtime.
- `java.lang.reflect.Proxy` creates proxy classes that implement specified interfaces.
- Method calls are dispatched to an `InvocationHandler`.
- Used in AOP frameworks (Spring AOP) to add cross-cutting concerns.
- For classes (not interfaces), use bytecode manipulation libraries like CGLIB or ByteBuddy.

## Java Networking

### Sockets
- Sockets are endpoints for communication between two machines over a network.
- Java provides `Socket` (client) and `ServerSocket` (server) for TCP communication.
- `DatagramSocket` for UDP.
- Sockets allow reading/writing data via streams.
- Example: client connects to server IP and port, exchanges data.

### TCP vs UDP
- TCP (Transmission Control Protocol) is connection-oriented, reliable, ordered, and error-checked; used for HTTP, email, FTP.
- UDP (User Datagram Protocol) is connectionless, unreliable, but faster; used for streaming, DNS, gaming.
- TCP ensures data integrity but has overhead; UDP is lightweight but can lose packets.
- Java supports both via `Socket`/`ServerSocket` (TCP) and `DatagramSocket` (UDP).
- Choice depends on application requirements.

### HTTP basics
- HTTP (Hypertext Transfer Protocol) is the foundation of web communication.
- Request methods: GET, POST, PUT, DELETE, etc.
- Status codes: 200 OK, 404 Not Found, 500 Internal Server Error.
- Headers provide metadata (content type, caching, authorization).
- Java provides `HttpURLConnection` (since 1.1) and newer `HttpClient` (since Java 11).

### URL connections
- `URL` class represents a Uniform Resource Locator.
- `URLConnection` (and `HttpURLConnection`) allows reading from and writing to a URL.
- Can set request properties, handle redirects, timeouts.
- Since Java 11, `HttpClient` provides a more modern API for HTTP/1.1 and HTTP/2, with asynchronous support.
- `HttpClient` supports synchronous and asynchronous requests, WebSocket.

### REST clients
- REST (Representational State Transfer) is an architectural style for web services.
- Java REST clients: `HttpClient`, Jersey Client, Apache HttpClient, Spring RestTemplate (now WebClient).
- `HttpClient` (Java 11) is recommended for new projects.
- REST clients interact with REST APIs using HTTP methods, JSON/XML payloads.
- Serialization often handled by libraries like Jackson or Gson.

## Advanced Java Language Features

### Inner classes
- Inner classes are non-static nested classes defined within another class.
- They have access to all members (including private) of the outer class.
- Each inner class instance is associated with an outer class instance.
- Used for logically grouping classes that are used in only one place, increasing encapsulation.
- Types: member inner class, local inner class (inside method), anonymous inner class.

### Anonymous classes
- Anonymous classes are inner classes without a name, declared and instantiated in a single expression.
- They are useful for implementing interfaces or extending classes on the spot (e.g., event handlers).
- Syntax: `new Interface() { ... }` or `new Class() { ... }`.
- Can capture effectively final variables from enclosing scope.
- Since Java 8, often replaced by lambda expressions for functional interfaces.

### Nested classes
- Nested classes are classes defined within another class, either static (static nested) or non-static (inner).
- Static nested classes behave like top-level classes but are nested for packaging convenience.
- They cannot access instance members of outer class directly (only via object reference).
- Static nested classes are often used as helpers or builders.

### Var keyword (local variable type inference)
- `var` (Java 10+) allows local variable type inference; the compiler infers type from initializer.
- Can be used in local variables, for-loop indices, and try-with-resources (Java 11).
- Not allowed for fields, method parameters, or return types.
- Improves readability by reducing boilerplate, but overuse can obscure type.
- Type is inferred at compile time, not runtime; `var` is not a dynamic type.

### Switch expressions (Java 12+)
- Switch expressions extend switch statement to return a value and have new syntax.
- Arrow syntax: `case X -> result;` (no fall-through).
- Use `yield` to return value from a block.
- Switch expressions are exhaustive (cover all cases) for enums and sealed types.
- They improve code conciseness and reduce bugs from missing break.

### Pattern matching (Java 14+ preview, 16+ standard)
- Pattern matching for `instanceof` allows variable declaration and casting in one step.
- Example: `if (obj instanceof String s) { System.out.println(s.length()); }`
- Pattern matching for switch (preview in later versions) allows patterns in case labels.
- Reduces boilerplate and improves safety.
- Evolving feature with more patterns (deconstruction) in future.

### Sealed classes (Java 15+ preview, 17+ standard)
- Sealed classes restrict which classes or interfaces may extend/implement them.
- Declared with `sealed` modifier, then `permits` clause listing permitted subclasses.
- Permitted subclasses must be `final`, `sealed`, or `non-sealed`.
- Enable exhaustive pattern matching and better domain modeling.
- Enhance encapsulation by controlling inheritance.

### Modules (Java 9+)
- Java Platform Module System (JPMS) introduces modules for better encapsulation and reliable configuration.
- Module descriptor `module-info.java` specifies module name, exports packages, requires other modules.
- Modules help in creating maintainable and scalable applications.
- Strong encapsulation: public types are not accessible unless package is exported.
- Enables reliable configuration and reduced footprint.

## JDBC

### JDBC architecture
- JDBC (Java Database Connectivity) is an API for connecting and executing queries on databases.
- Consists of interfaces (Driver, Connection, Statement, ResultSet) implemented by database-specific drivers.
- Two-tier architecture: Java app directly talks to DB via driver.
- Three-tier: app server in between (e.g., using connection pooling).
- JDBC provides both high-level and low-level access.

### Drivers
- JDBC drivers are implementations of JDBC interfaces for specific databases.
- Four types: Type 1 (JDBC-ODBC bridge, deprecated), Type 2 (native API, partially Java), Type 3 (network protocol), Type 4 (pure Java, thin driver).
- Type 4 drivers are most common (e.g., MySQL Connector/J, PostgreSQL JDBC).
- DriverManager loads drivers and manages connections.
- Also DataSource for connection pooling.

### Connections
- Connection represents a session with the database; obtained via `DriverManager.getConnection()` or `DataSource`.
- Connection properties: URL, username, password.
- Connection can set auto-commit mode, transaction isolation, etc.
- Must be closed after use (try-with-resources).
- Connection pooling reuses connections to improve performance.

### Statements
- Statement: used to execute static SQL queries (no parameters). Vulnerable to SQL injection.
- PreparedStatement: precompiled SQL with placeholders (parameters), preventing SQL injection and improving performance for repeated queries.
- CallableStatement: for stored procedures.
- Execute methods: `executeQuery()` for SELECT, `executeUpdate()` for INSERT/UPDATE/DELETE, `execute()` for any.

### Prepared statements
- PreparedStatement extends Statement, allows parameterized SQL with `?` placeholders.
- Parameters set using setXxx methods (e.g., `setString(1, value)`).
- Benefits: performance (precompiled), security (escapes input), readability.
- Important for preventing SQL injection attacks.
- Can be reused with different parameter values.

### ResultSet handling
- ResultSet represents the result of a query; cursor initially before first row.
- `next()` advances cursor, returns false when no more rows.
- Retrieve column values with getXxx methods (by index or column name).
- Types: forward-only, scrollable, updatable; concurrency: read-only, updatable.
- Must close ResultSet when done (usually automatically when Statement closed).

### Transactions
- Transactions ensure ACID properties (Atomicity, Consistency, Isolation, Durability).
- By default, each SQL statement is auto-committed.
- To group multiple statements, disable auto-commit: `connection.setAutoCommit(false)`.
- Commit: `connection.commit()`, rollback: `connection.rollback()`.
- Savepoints allow partial rollback.

### Batch processing
- Batch processing sends multiple SQL statements in one database round-trip for performance.
- Statement: `addBatch()`, `executeBatch()`.
- PreparedStatement can batch multiple parameter sets.
- Returns array of update counts.
- Useful for bulk inserts/updates.

### Connection pooling
- Connection pooling manages a pool of database connections for reuse, reducing overhead of creating connections.
- Implemented by DataSource (e.g., HikariCP, Apache DBCP, c3p0).
- Application borrows connection from pool, returns it after use.
- Improves scalability and response time.
- Configure pool size based on workload.

## Performance & Optimization

### Profiling Java apps
- Profiling tools (JProfiler, VisualVM, YourKit, Java Flight Recorder) help identify performance bottlenecks.
- CPU profiling shows hot methods; memory profiling reveals leaks and allocation patterns.
- Thread profiling detects contention and deadlocks.
- Use profiling during development and under realistic load.
- Focus on optimizing hot spots, not premature optimization.

### Memory tuning
- Tune heap sizes: `-Xms`, `-Xmx` to avoid frequent GC and OutOfMemoryError.
- Adjust young generation size (`-XX:NewRatio`, `-Xmn`) based on application object allocation patterns.
- Set appropriate Metaspace size.
- Monitor heap usage with tools; look for memory leaks.
- Use `-XX:+HeapDumpOnOutOfMemoryError` to capture dumps.

### GC tuning
- Goal: minimize GC overhead while meeting pause time requirements.
- Choose GC algorithm based on application needs: G1 for balanced, Parallel for throughput, ZGC for low-latency.
- Tune parameters like `-XX:MaxGCPauseMillis` for G1, `-XX:G1HeapRegionSize`.
- Monitor GC logs with tools (GCViewer, gceasy) to understand behavior.
- Aim for high throughput and low pauses.

### Thread tuning
- Thread pool sizing: number of threads depends on CPU cores and I/O vs CPU-bound tasks.
- For CPU-bound, pool size around number of cores; for I/O-bound, can be larger.
- Use `ThreadPoolExecutor` with bounded queues to prevent resource exhaustion.
- Monitor thread usage, stack traces, and deadlocks.
- Avoid excessive context switching.

### CPU optimization
- Identify CPU-intensive operations: algorithms, loops, object creation.
- Optimize code: use efficient data structures, avoid unnecessary boxing, reduce method calls.
- Use parallel streams or Fork/Join for parallelizable CPU tasks.
- JIT optimizations like inlining can help, but code clarity matters.
- Profiling guides where to focus.

### I/O optimization
- I/O operations (disk, network) are often bottlenecks.
- Use buffering (BufferedInputStream, BufferedWriter) to reduce system calls.
- For file I/O, consider memory-mapped files or NIO for large files.
- For network, use non-blocking I/O (NIO) or asynchronous I/O.
- Connection pooling for databases and HTTP clients.

## Security

### Java security model
- Java provides a comprehensive security model: bytecode verification, class loaders, security manager, access control, cryptography.
- Sandboxing: untrusted code runs in restricted environment.
- Security manager enforces permissions (file, network, reflection) based on policy.
- Since Java 17, Security Manager is deprecated for removal, but still used in legacy apps.
- Modern security relies on module system and encapsulation.

### Cryptography basics
- Java Cryptography Architecture (JCA) provides APIs for encryption, decryption, hashing, digital signatures.
- Key classes: `Cipher`, `KeyGenerator`, `SecretKey`, `MessageDigest`, `Signature`.
- Supports algorithms: AES, RSA, SHA, HMAC.
- Use `SecureRandom` for cryptographically strong random numbers.
- Java Cryptography Extension (JCE) provides unlimited strength jurisdiction policy files (now bundled).

### SSL/TLS
- SSL/TLS provides secure communication over networks.
- Java supports SSL/TLS via `javax.net.ssl` package: `SSLSocket`, `SSLServerSocket`, `HttpsURLConnection`.
- Key management: keystores (private keys) and truststores (trusted certificates).
- `SSLContext` can be configured with specific protocols and cipher suites.
- In Java, default TLS version can be set; ensure weak protocols disabled.

### Secure coding practices
- Validate input to prevent injection attacks (SQL, command, XSS).
- Use prepared statements for SQL queries.
- Avoid exposing sensitive data (passwords, tokens) in logs or exceptions.
- Use proper access control: least privilege principle.
- Keep libraries updated to avoid known vulnerabilities.
- Use static analysis tools (FindBugs, SonarQube) to detect security flaws.

### Authentication & authorization
- Authentication verifies identity (who you are); authorization determines access rights (what you can do).
- Java EE/Spring Security provides frameworks for authentication/authorization.
- JAAS (Java Authentication and Authorization Service) is a pluggable authentication module.
- Use secure password storage (hashing with salt, bcrypt).
- Implement role-based access control (RBAC) or OAuth2 for APIs.

Here are detailed study notes for a Senior Developer preparing for a senior position interview, covering all specified topics and subtopics with at least 5 bullet points each.

---

## Java Fundamentals

### Java Platform Overview
- **Write Once, Run Anywhere (WORA):** The core philosophy where Java code is compiled into platform-independent bytecode, which can then be executed on any platform with a compatible Java Virtual Machine (JVM).
- **Strong Ecosystem:** Benefits from a vast and mature ecosystem of libraries, frameworks (Spring, Hibernate), and tools, making it a preferred choice for enterprise-grade applications.
- **Community-Driven:** Developed through the Java Community Process (JCP), allowing individuals and organizations to contribute to the evolution of the Java specification.
- **Multi-Paradigm:** Primarily object-oriented but supports other paradigms like functional programming (via lambdas and streams), concurrent programming, and imperative programming.
- **Focus on Stability and Backward Compatibility:** A key design principle is maintaining backward compatibility across major versions, ensuring that applications written years ago can still run on modern JVMs.

### JVM, JRE, JDK Differences
- **JDK (Java Development Kit):** The full-featured software development kit for Java developers. It includes the JRE, compilers (`javac`), debuggers, documentation tools (javadoc), and other utilities needed to develop Java applications.
- **JRE (Java Runtime Environment):** Provides the libraries, Java Virtual Machine (JVM), and other components to run applications written in Java. It does not contain development tools like a compiler.
- **JVM (Java Virtual Machine):** The abstract computing machine that executes Java bytecode. It provides a runtime environment, manages memory, and ensures security. It is the core of the "write once, run anywhere" principle.
- **The Relationship:** JDK $\supset$ JRE $\supset$ JVM. The JDK includes the JRE, and the JRE includes one or more JVM implementations.
- **Implementation Variations:** While the JVM is a specification, there are multiple implementations (e.g., HotSpot, OpenJ9) from different vendors, each with its own performance characteristics and optimizations.

### Java Compilation Pipeline
- **Source Code to Bytecode:** The Java compiler (`javac`) reads `.java` source files, performs syntax and semantic checks, and translates them into platform-independent `.class` files containing bytecode.
- **Loading and Verification:** The ClassLoader loads the `.class` files. The bytecode verifier then checks the code for security violations, stack under/overflow, and illegal data type conversions to ensure it's safe to execute.
- **Execution:** The JVM executes the verified bytecode. This can be done by interpreting it, or by compiling it to native machine code using the Just-In-Time (JIT) compiler for performance gains.
- **Linking:** This is a crucial runtime phase involving resolution of symbolic references (like class and method names) into direct references (memory addresses), and preparation of static fields.
- **Initialization:** The final step where static initializers and static blocks are executed, and class/interface initialization begins.

### Bytecode Fundamentals
- **Instruction Set:** Bytecode is a set of single-byte instructions (hence the name), each representing a specific operation like loading/storing values, arithmetic operations, or method invocation.
- **Stack-Oriented Architecture:** The JVM is a stack-based machine. Most bytecode instructions operate by popping operands from the operand stack and pushing results back onto it.
- **Platform Independence:** This intermediate representation is what enables Java's portability. The same bytecode can be interpreted or JIT-compiled to native code on any platform with a JVM.
- **Human-Readable Form:** Bytecode can be viewed using the `javap` command (e.g., `javap -c MyClass.class`), which disassembles the `.class` file and provides a human-readable mnemonics version of the instructions.
- **Key Instructions:** Common instructions include `aload` (load a reference), `iload` (load an integer), `invokevirtual` (call a method), `getfield` (access an instance variable), and `return`.

### Java Syntax and Semantics
- **C-Style Syntax:** Inherits its basic syntax from C and C++, making it familiar to a wide range of developers. This includes curly braces `{}` for blocks, semicolons `;` for statement termination, and common keywords like `if`, `else`, `for`, `while`.
- **Strictly Typed:** Java is a statically-typed language. Every variable, parameter, and return type must be declared at compile-time, enabling early error detection and compiler optimizations.
- **Case Sensitivity:** The language is case-sensitive, meaning identifiers like `myVariable` and `MyVariable` are distinct.
- **Keywords and Modifiers:** A rich set of predefined keywords (e.g., `class`, `interface`, `extends`, `implements`) and modifiers (e.g., `public`, `private`, `static`, `final`, `abstract`) control access, behavior, and structure.
- **Exception Handling:** Provides a robust mechanism for error management using `try`, `catch`, `finally`, `throw`, and `throws` keywords, enforcing either handling or declaration of checked exceptions.

### Primitive Data Types
- **The 8 Primitives:** Java defines eight primitive types: `byte` (8-bit), `short` (16-bit), `int` (32-bit), `long` (64-bit), `float` (32-bit), `double` (64-bit), `char` (16-bit Unicode), and `boolean` (true/false).
- **Stored by Value:** Variables of primitive types hold the actual value directly in the memory location assigned to the variable (on the stack for local variables), unlike reference types which hold an address to an object.
- **Default Values:** If declared as class-level (instance or static) variables, primitives are given default values (e.g., `0` for numeric types, `false` for boolean, `\u0000` for char). Local variables do not get default values and must be initialized.
- **Performance:** Operations on primitives are significantly faster than on their corresponding wrapper classes because they avoid object overhead and boxing/unboxing.
- **Fixed Size and Behavior:** The size and behavior of each primitive type are strictly defined by the Java Language Specification, ensuring platform-independent behavior for numeric operations.

### Reference Types
- **Holding References:** Reference type variables (classes, interfaces, arrays, enums) do not hold the object itself, but rather a reference (a pointer or memory address) to the object stored in the heap.
- **Class Types:** Variables declared as an instance of a class (or subclass) can point to objects of that class. This is the most common kind of reference type.
- **Interface Types:** A variable of an interface type can hold a reference to any object that implements that interface, enabling polymorphism.
- **Array Types:** Arrays are objects in Java. An array variable holds a reference to an array object, whether it's an array of primitives (`int[]`) or an array of objects (`String[]`).
- **`null` Value:** Any reference variable that does not currently point to an object can be assigned the special literal `null`. Dereferencing a `null` reference (e.g., calling a method on it) results in a `NullPointerException`.

### Variables and Scope
- **Local Variables:** Declared inside a method, constructor, or block. They are created when the block is entered and destroyed when it exits. They must be initialized before use and reside on the stack.
- **Instance Variables (Non-Static Fields):** Declared in a class but outside any method. They are associated with a specific instance of a class. Each object has its own copy, and they live in the heap as part of the object.
- **Class Variables (Static Fields):** Declared with the `static` modifier. There is exactly one copy of this variable per classloader, regardless of how many objects are created. They live in a special runtime area of the heap called the "method area" or "static storage."
- **Scope Rules:** Scope defines where a variable is accessible. A variable is in scope from its declaration to the end of the block in which it was defined. Variable shadowing can occur when an inner scope declares a variable with the same name as one in an outer scope.
- **Final Variables:** A variable can be declared `final`, meaning it can be assigned only once. For primitives, this makes the value constant; for references, it makes the reference constant (the object it points to can still be mutated).

### Memory Model Basics
- **The Java Memory Model (JMM):** Not a physical partition of memory, but a formal specification that defines how threads interact through memory and what behaviors are allowed in concurrent execution. It guarantees visibility of changes across threads.
- **Happens-Before Guarantee:** A set of rules that ensures that actions in one thread are visible to another thread in a predictable order. For example, unlocking a monitor *happens-before* every subsequent locking of that same monitor.
- **Main Memory and Working Memory:** The JMM conceptually divides memory into main memory (shared by all threads) and working memory (thread-local caches, similar to CPU caches). Variables must be copied from main to working memory to be operated on.
- **Visibility:** The JMM addresses the problem of when a write to a variable by one thread becomes visible to other threads. Without proper synchronization (e.g., using `volatile` or `synchronized`), updates may not be visible.
- **Reordering:** To optimize performance, compilers and processors can reorder instructions. The JMM allows this as long as the reordering does not change the program's outcome from the perspective of a single thread. The `volatile` keyword and synchronization impose restrictions to prevent problematic reorderings in multi-threaded contexts.

### Control Flow Statements
- **Decision-Making:** `if-else` and `switch` statements allow the program to execute different branches of code based on a condition. Modern `switch` expressions (Java 12+) can return values and are more concise.
- **Looping:** `for` (including the enhanced for-each loop), `while`, and `do-while` statements allow a block of code to be executed repeatedly based on a condition. The enhanced for loop is preferred for iterating over collections and arrays.
- **Branching:** `break` and `continue` alter the normal flow of loops. `break` exits the loop entirely, while `continue` skips the rest of the current iteration and proceeds to the next one. Labeled `break` and `continue` can jump out of nested loops.
- **Exception-Based Flow:** `try-catch-finally` and `try-with-resources` blocks provide structured ways to handle exceptional conditions and manage resources, representing a non-local transfer of control.
- **Return:** The `return` statement exits from the current method and optionally passes a value back to the caller. In a `void` method, `return` can be used without a value.

### Arrays and Strings
- **Arrays as Objects:** In Java, arrays are objects that are dynamically created. They can hold primitives or object references and have a `length` property.
- **Fixed Size:** Once created, the length of an array cannot be changed. To have a dynamically sized collection, classes from the Collections Framework (like `ArrayList`) must be used.
- **Declaration and Creation:** Arrays can be declared with `int[] myArray;` and created with `myArray = new int[10];` or inline with `int[] myArray = {1, 2, 3};`.
- **Strings as Immutable Objects:** `String` is a class, not a primitive. String objects are immutable, meaning any operation that seems to modify a String (e.g., `concat()`) actually creates and returns a new String object.
- **String vs StringBuilder vs StringBuffer:** `String` is immutable. `StringBuilder` is mutable and not thread-safe (best for single-threaded string manipulation). `StringBuffer` is mutable and thread-safe (synchronized), but slower than `StringBuilder`.

### String Pool
- **Concept:** The String Pool (or String Intern Pool) is a special storage area in the JVM heap (previously in the PermGen/Metaspace) where `String` literals and interned strings are stored.
- **Purpose:** To save memory and improve performance by reusing immutable `String` objects. If a string literal already exists in the pool, a reference to the existing object is returned instead of creating a new one.
- **Literal Creation:** When a String is created using a literal (e.g., `String s1 = "Hello";`), the JVM first checks the pool. If "Hello" exists, `s1` points to it. If not, a new String object is created in the pool.
- **`new String()`:** Creating a String with the `new` keyword (e.g., `String s2 = new String("Hello");`) forces the creation of a new String object in the regular heap, even if the same string exists in the pool.
- **`intern()` Method:** This method can be called on any String object. It checks if an equal string exists in the pool. If it does, it returns the pooled reference. If not, it adds the current string to the pool and returns its reference.

### Immutability
- **Definition:** An object is immutable if its state cannot be changed after it is created. All fields are set during construction and cannot be modified.
- **How to Achieve:** Declare the class `final` (so it can't be subclassed), make all fields `final` and `private`, do not provide any setter methods that modify fields, and ensure that if the class holds references to mutable objects (like `Date` or collections), those objects are not exposed or directly modifiable (e.g., return defensive copies).
- **Benefits:** Immutable objects are inherently thread-safe, can be shared freely, simplify development (no complex state transitions), and are excellent keys for hash-based collections (`HashMap`, `HashSet`) because their hash code never changes.
- **Examples:** `String`, all primitive wrapper classes (`Integer`, `Long`), `BigInteger`, and `BigDecimal` are classic examples of immutable classes in Java.
- **Performance Considerations:** Creating a new object for every state change can lead to performance overhead and increased garbage collection. Using mutable builders (like `StringBuilder`) for intermediate operations can be more efficient.

### Wrapper Classes
- **Purpose:** Provide an object representation of the eight primitive types (e.g., `Integer` for `int`, `Boolean` for `boolean`). This allows primitives to be used in contexts that require objects, such as Java Collections (`ArrayList<Integer>`).
- **Immutability:** All wrapper classes are immutable, just like `String`. Once an `Integer` object is created with a value, that value cannot be changed.
- **Utility Methods:** Wrapper classes provide useful static methods for converting to/from primitives and strings (e.g., `Integer.parseInt(String)`, `Integer.valueOf(int)`).
- **Caching:** Many wrapper classes (e.g., `Integer`, `Byte`, `Short`, `Long`, `Character`) cache frequently used values (e.g., `Integer` caches values from -128 to 127). The `valueOf()` method will return cached instances for these values, while the constructor `new Integer()` always creates a new object.
- **Performance Overhead:** Using wrapper classes introduces memory overhead (object header, fields) compared to primitives. Operations on them also involve the cost of object access.

### Autoboxing/unboxing
- **Autoboxing:** The automatic conversion that the Java compiler makes between a primitive type and its corresponding object wrapper class. For example, assigning an `int` to an `Integer`: `Integer i = 10;`. The compiler converts this to `Integer i = Integer.valueOf(10);`.
- **Unboxing:** The automatic conversion that the compiler makes from a wrapper class to its corresponding primitive. For example, using an `Integer` in an arithmetic expression: `Integer i = 10; int j = i + 5;`. The compiler converts this to `int j = i.intValue() + 5;`.
- **Method Arguments and Return Types:** Autoboxing/unboxing also occurs when passing primitives to methods expecting wrappers, and vice-versa, and when returning values from methods.
- **Performance Implications:** While convenient, autoboxing/unboxing can introduce hidden performance costs due to the creation of intermediate wrapper objects, especially in tight loops.
- **NullPointerException Risk:** Unboxing a `null` wrapper object leads to a `NullPointerException`. For example: `Integer i = null; int j = i; // Throws NullPointerException`.

### Pass-by-value Semantics
- **Always Pass-by-Value:** Java is strictly pass-by-value. This means that a copy of the variable's value is passed to a method.
- **Primitives:** When a primitive variable is passed, a copy of its actual value is passed. Any changes to the parameter inside the method do not affect the original variable.
- **References:** When a reference variable is passed, a copy of the reference (the memory address) is passed. The method now has a new reference that points to the same object in the heap.
- **Object Mutation vs. Reassignment:** Because both the original and the copied reference point to the same object, the method *can* modify the object's internal state. However, if the method reassigns the copied reference to a new object, the original reference outside the method still points to the old object.
- **Common Misconception:** This is often confused with "pass-by-reference." In a true pass-by-reference language, the method would receive the original variable itself, and reassigning it would affect the caller. This does not happen in Java.

### Object Lifecycle
- **Creation:** The lifecycle begins when an object is created using the `new` keyword, reflection, deserialization, or cloning. Memory is allocated on the heap, and the constructor(s) are executed to initialize the object's state.
- **In-Use:** The object is actively used by one or more threads. It is considered "reachable" (via a chain of strong references from GC roots) and is not eligible for garbage collection.
- **Invisible:** The object is still reachable, but it is no longer used by the application. This state is often not explicitly tracked by the JVM but is a conceptual step before becoming unreachable.
- **Unreachable:** The object is no longer referenced by any active part of the application. There are no strong references from any GC root (local variables, static fields, active threads, JNI references) pointing to it. At this point, it becomes eligible for garbage collection.
- **Finalization and Collection:** Before an object is collected, the JVM might run its `finalize()` method (if overridden), which is generally discouraged due to unpredictability and performance issues. After finalization (or if not needed), the object's memory is reclaimed by the garbage collector.

### Access Modifiers
- **`public`:** The member (class, method, field) is accessible from any other class in any package. This is the least restrictive level.
- **`protected`:** The member is accessible within its own package (like package-private) AND by subclasses of its class, even if those subclasses are in different packages.
- **Default (Package-Private):** When no modifier is specified, the member is accessible only within classes in the same package. This is useful for implementation details that are shared within a package but not exposed externally.
- **`private`:** The member is accessible only within the same class where it is declared. This is the most restrictive level and is the cornerstone of encapsulation.
- **Top-Level Classes:** A top-level class (not nested) can only be declared as `public` or with package-private (default). Inner classes can also be `private` or `protected`.

### Packages and Modules
- **Packages:** A namespace that organizes a set of related classes and interfaces. They prevent naming conflicts, provide access protection (package-private), and make code more manageable (e.g., `java.util`, `com.mycompany.myapp.model`).
- **Naming Conventions:** Package names are typically written in all lowercase, with underscores avoided. They are often based on the reverse domain name of the company (e.g., `com.google.common`).
- **Modules (Project Jigsaw - Java 9+):** A higher level of aggregation above packages. A module is a named, self-describing collection of code and data. Its `module-info.java` file declares which packages it *exports* for other modules to use and which external modules it *requires*.
- **Strong Encapsulation:** Modules allow for strong encapsulation by explicitly declaring which packages are accessible to other modules. Packages not exported are strictly internal to the module.
- **Reliable Configuration:** Modules help eliminate classpath issues (like `NoClassDefFoundError` or `ClassCastException` due to different versions) by ensuring that a module has access to all the code it needs at compile-time and runtime.

### Core Libraries Overview
- **`java.lang`:** The most fundamental package, automatically imported. Contains essential classes like `Object` (the root of the class hierarchy), `String`, `Math`, the wrapper classes, and core exceptions.
- **`java.util`:** The collections framework (`List`, `Set`, `Map`, etc.), concurrency utilities (`java.util.concurrent`), date and time API (`java.time`), and miscellaneous utility classes like `Scanner`, `Random`, and `Arrays`.
- **`java.io` and `java.nio`:** Provide classes for system input and output through data streams, serialization, and the file system. `java.nio` (New I/O) offers non-blocking I/O operations and buffer-oriented APIs for better performance.
- **`java.net`:** Contains classes for networking applications, including support for URLs, TCP sockets (`Socket`, `ServerSocket`), UDP sockets (`DatagramSocket`), and HTTP clients (modern `HttpClient` since Java 11).
- **`java.sql` and `javax.sql`:** The core JDBC (Java Database Connectivity) API for accessing and processing data stored in a data source (usually a relational database).

---

## Advanced Java

### Object-oriented design principles
- **Encapsulation:** Bundling data (fields) and methods that operate on that data within a single unit (a class), while restricting direct access to some of the object's internal components. Achieved using `private` fields and `public` getters/setters.
- **Inheritance:** Establishing an "is-a" relationship between classes, where a child class inherits fields and methods from a parent class. Promotes code reuse and establishes a hierarchical relationship. Achieved using the `extends` keyword.
- **Polymorphism:** The ability of an object to take on many forms. In Java, this is primarily achieved through method overriding (runtime polymorphism) where a subclass provides a specific implementation of a method defined in its superclass, and method overloading (compile-time polymorphism).
- **Abstraction:** Hiding complex implementation details and showing only the essential features of an object. Achieved through abstract classes and interfaces, which define a contract for what an object can do, without specifying how it does it.
- **Composition over Inheritance:** A design principle suggesting that classes should achieve code reuse and flexibility by containing instances of other classes that implement desired functionality, rather than inheriting from a base class. This leads to more loosely coupled and maintainable systems.

### SOLID principles
- **Single Responsibility Principle (SRP):** A class should have only one reason to change, meaning it should have only one job or responsibility. This leads to higher cohesion and makes classes easier to understand and maintain.
- **Open/Closed Principle (OCP):** Classes should be open for extension but closed for modification. This means you should be able to add new functionality to a class without changing its existing code, typically achieved through abstraction (interfaces, abstract classes) and polymorphism.
- **Liskov Substitution Principle (LSP):** Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. In essence, a subclass should not alter the expected behavior of the parent class.
- **Interface Segregation Principle (ISP):** Clients should not be forced to depend on interfaces they do not use. This means large, "fat" interfaces should be split into smaller, more specific ones so that implementing classes only need to know about the methods that are of interest to them.
- **Dependency Inversion Principle (DIP):** High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions. This is often implemented using dependency injection.

### Generics and type erasure
- **Type Safety:** Generics enable stronger type checks at compile time. They allow you to detect type mismatch errors (e.g., trying to add a `String` to a `List<Integer>`) during compilation, not at runtime.
- **Code Reuse:** They allow you to write classes, interfaces, and methods that can operate on objects of various types while maintaining type safety (e.g., `ArrayList<T>` can be used for any type `T`).
- **Type Erasure:** This is how Java implements generics for backward compatibility with pre-generics code. The compiler removes all information about generic type parameters and type arguments, replacing them with their upper bound (usually `Object`) and adding necessary type casts.
- **Runtime Behavior:** Because of type erasure, at runtime, a `List<String>` is just a raw `List`. You cannot, for example, use `instanceof` to check if a collection is of a specific generic type (e.g., `list instanceof List<String>` is a compile-time error).
- **Reifiable vs. Non-Reifiable Types:** Reifiable types are those whose type information is fully available at runtime (primitives, non-generic types, raw types). Non-reifiable types (like `List<String>`) have their type information partially or completely erased at compile time and are not fully available at runtime.

### Reflection API
- **Introspection at Runtime:** Reflection allows a program to inspect or "reflect" upon itself and manipulate the internal properties of classes, interfaces, fields, and methods at runtime, without knowing their names at compile time.
- **Core Classes:** The primary classes for reflection are in the `java.lang.reflect` package: `Class` (representing a class), `Method`, `Field`, `Constructor`, and `Array`.
- **Dynamic Instantiation and Invocation:** You can create instances of classes using `Class.forName("...").newInstance()` (or `getDeclaredConstructor().newInstance()`) and invoke methods using `method.invoke(object, args)`, even if the class and method names are determined at runtime.
- **Accessing Private Members:** Using reflection, you can bypass access control checks. By calling `setAccessible(true)` on a `Field` or `Method` object, you can read or modify `private` fields and invoke `private` methods.
- **Performance Overhead:** Reflection is powerful but has significant performance overhead due to dynamic resolution and type checking. It should be used judiciously and avoided in performance-critical code paths.

### Annotations
- **Metadata:** Annotations are a form of metadata that provide data about a program that is not part of the program itself. They have no direct effect on the operation of the code they annotate.
- **Use Cases:** Used by the compiler (e.g., `@Override`), by build tools (e.g., `@Deprecated`), or at runtime via reflection to configure behavior, generate code, or process them in frameworks (e.g., `@Test` in JUnit, `@Autowired` in Spring).
- **Built-in Annotations:** Java provides several built-in annotations like `@Override`, `@Deprecated`, `@SuppressWarnings`, and `@FunctionalInterface`.
- **Meta-Annotations:** Annotations used to annotate other annotations. They define how the custom annotation behaves. Key meta-annotations are `@Retention` (how long the annotation is retained: `SOURCE`, `CLASS`, `RUNTIME`), `@Target` (where the annotation can be applied), `@Inherited`, and `@Documented`.
- **Custom Annotations:** You can create your own annotation types by using the `@interface` keyword. They can have elements (which look like methods) to store values.

### Class loaders
- **Dynamic Loading:** The class loader is a part of the JRE that dynamically loads Java classes into the JVM. Classes are not loaded all at once; they are loaded on demand, when first referenced.
- **Delegation Model:** Class loaders follow a hierarchical parent-first delegation model. When asked to find a class, a class loader first delegates the request to its parent class loader. Only if the parent cannot find the class does it attempt to find it itself.
- **Bootstrap, Extension, and System:** The three built-in class loaders are the Bootstrap (loads core JDK classes from `rt.jar`), Extension (loads classes from the `ext` directory), and System/Application (loads classes from the application's classpath).
- **Custom Class Loaders:** Developers can create their own class loaders by extending the `java.lang.ClassLoader` class. This is useful for loading classes from non-standard sources (e.g., network, database), implementing hot deployment, or adding encryption/decryption.
- **Unique Namespaces:** Each class loader defines its own namespace. Classes loaded by different class loaders are considered distinct types, even if they originate from the same bytecode, leading to potential `ClassCastExceptions`.

### Dynamic proxies
- **Purpose:** Dynamic proxies allow you to create a proxy instance for a list of specified interfaces at runtime. Method calls on the proxy instance are dispatched to a single `InvocationHandler`.
- **`java.lang.reflect.Proxy`:** This class provides static methods for creating dynamic proxy classes and instances. It is the central API for this functionality.
- **`InvocationHandler`:** This is the interface implemented by the invocation handler. It has a single method, `invoke(Object proxy, Method method, Object[] args)`, which is called whenever a method is invoked on the proxy instance.
- **Use Cases:** Dynamic proxies are widely used for aspect-oriented programming (AOP) (e.g., in Spring to manage transactions), for implementing remote method invocation (RMI), and for creating mock objects in testing frameworks.
- **Limitation:** They can only proxy interfaces, not concrete classes. For proxying classes, you would need a bytecode manipulation library like CGLIB or Byte Buddy.

### Lambda expressions
- **Concise Representation:** Lambdas provide a clear and concise way to represent a single-method interface (a functional interface) using an expression. They enable functional programming in Java.
- **Syntax:** The basic syntax is `(parameters) -> { body }`. Type inference often allows you to omit the parameter types. If the body has a single expression, the curly braces and `return` keyword can also be omitted.
- **Target Typing:** The type of a lambda expression is inferred from the context in which it is used. The context must expect a functional interface (e.g., as a method parameter or assignment target).
- **Variable Capture:** Lambdas can capture (access) effectively final variables from the enclosing scope. This means the variable must not be modified after being initialized. Capturing instance variables is allowed as they are effectively captured via the `this` reference.
- **Comparison with Anonymous Inner Classes:** Lambdas are more concise and have slightly different scoping rules (e.g., `this` inside a lambda refers to the enclosing object, not the lambda itself). They are also more efficient as they don't necessarily create a new class file for each instance.

### Functional interfaces
- **Definition:** A functional interface is an interface that contains exactly one abstract method. It can have any number of default or static methods. The `@FunctionalInterface` annotation can be used to enforce this contract.
- **Purpose:** They are the target types for lambda expressions and method references. A lambda expression provides an implementation for the single abstract method.
- **`java.util.function` Package:** Java provides a rich set of built-in functional interfaces to cover common use cases, such as `Predicate<T>` (test a condition), `Function<T,R>` (map an input to an output), `Consumer<T>` (perform an action), `Supplier<T>` (provide an instance), and `UnaryOperator<T>` / `BinaryOperator<T>`.
- **Primitive Specializations:** To avoid autoboxing overhead, many functional interfaces have primitive specializations, like `IntPredicate`, `DoubleFunction<R>`, `LongConsumer`, etc.
- **Common Use:** They are the foundation of the Stream API and are heavily used in modern Java code to pass behavior as data.

### Stream API
- **Functional Data Processing:** The Stream API, introduced in Java 8, provides a functional approach to process sequences of elements (like collections). It supports operations like `map`, `filter`, `reduce`, and `collect`.
- **Pipelines:** A stream pipeline consists of a source (e.g., a collection, an array), zero or more intermediate operations (e.g., `filter`, `map`), and a terminal operation (e.g., `collect`, `forEach`). Intermediate operations return a new stream and are lazy.
- **Lazy Evaluation:** Intermediate operations are not executed until a terminal operation is invoked. This allows for optimizations like short-circuiting (`findFirst`, `limit`) and fusion (combining multiple operations into a single pass).
- **Parallel Streams:** Streams can be easily converted to parallel streams using the `parallelStream()` method or `parallel()` intermediate operation. This can leverage multi-core architectures but requires careful consideration of thread safety and ordering.
- **Stateless vs. Stateful Operations:** Most stream operations are stateless (`filter`, `map`), meaning they do not maintain state. Stateful operations (`distinct`, `sorted`, `limit`) may need to process the entire stream to produce a result and can have performance implications in parallel streams.

### Optional usage
- **Purpose:** `Optional<T>` is a container object which may or may not contain a non-null value. Its primary use is to represent the potential absence of a return value, providing a clearer alternative to returning `null`.
- **Avoiding `NullPointerException`:** It encourages developers to actively think about and handle the "no value" case, forcing explicit checks (like `isPresent()`) or providing default values (`orElse()`, `orElseGet()`).
- **Functional Style:** `Optional` supports functional operations like `map`, `filter`, and `flatMap`, allowing you to chain operations on the contained value in a safe way without explicit null checks.
- **Creation:** Optional objects are created using `Optional.of(value)` (value must not be null), `Optional.ofNullable(value)` (value can be null), or `Optional.empty()`.
- **Misuse:** It is not intended to be used for fields, method parameters, or collections. Its design is primarily for return types. Using it in other contexts can lead to unnecessary code complexity and overhead.

### Records
- **Purpose (Java 14+):** Records are a special kind of class in Java whose purpose is to be transparent carriers for immutable data. They are a "data carrier" or "nominal tuple."
- **Concise Syntax:** A record provides a much more concise way to define simple data aggregates. Declaring `public record Point(int x, int y) {}` automatically gives you a constructor, `equals()`, `hashCode()`, `toString()`, and public accessor methods (named `x()` and `y()`).
- **Immutability:** The fields of a record are implicitly `final`. The canonical constructor initializes all fields, and there are no setters, making the record deeply immutable (provided its components are also immutable).
- **Customization:** You can override the automatically generated methods, and you can add your own instance or static methods. You can also provide a compact constructor to validate or normalize the data before assignment.
- **Use Cases:** Records are perfect for returning multiple values from a method, creating Data Transfer Objects (DTOs), representing database rows, or as intermediate objects in stream pipelines.

### Sealed classes
- **Purpose (Java 17+):** Sealed classes and interfaces restrict which other classes or interfaces may extend or implement them. They provide a more exhaustive way to model a fixed set of possibilities, enhancing domain modeling.
- **Declaration:** A sealed class is declared using the `sealed` modifier, and the `permits` clause specifies the allowed subclasses (e.g., `public sealed class Shape permits Circle, Square, Triangle { ... }`).
- **Subclasses:** Permitted subclasses must directly extend the sealed class, must be in the same module or package (if in the unnamed module), and must themselves be declared `final`, `sealed`, or `non-sealed`.
- **Exhaustiveness in Pattern Matching:** Sealed classes work exceptionally well with pattern matching. The compiler can determine if a `switch` expression covers all possible subtypes of a sealed class, eliminating the need for a default clause.
- **Better Abstraction:** They offer a more controlled form of inheritance than `final` classes (which permit none) or unsealed classes (which permit all). You define a closed set of possible subtypes, making the code more maintainable and secure.

### Pattern matching
- **Ongoing Evolution:** Pattern matching is a feature being incrementally added to Java. It allows you to conditionally extract components from objects in a more concise and type-safe way.
- **`instanceof` Pattern Matching (Java 16+):** The traditional `if (obj instanceof String)` is extended to `if (obj instanceof String s)`. If the check succeeds, the variable `s` is declared and initialized within the scope of the `if` block, eliminating the need for an explicit cast.
- **Switch Pattern Matching (Java 17+ - Preview):** Extends pattern matching to `switch` expressions and statements. You can have case labels with patterns (e.g., `case Integer i -> ...`), and the compiler can use type information to improve exhaustiveness checks, especially with sealed classes.
- **Deconstruction Patterns (Future):** A future direction for pattern matching is deconstruction, which would allow you to directly access the components of a record in a pattern, like `case Point(int x, int y) -> ...`.
- **Benefits:** The primary benefits are increased readability, conciseness, and safety by reducing boilerplate code and eliminating common errors like forgetting to cast after an `instanceof` check.

### Modules system
- **Project Jigsaw:** The module system was introduced in Java 9 as part of Project Jigsaw to make the Java platform more scalable, improve security, and enable better application performance.
- **`module-info.java`:** This is the key artifact. It's placed in the root of a module's source code and declares the module's name, which packages it exports, and which modules it requires.
- **Exports and Requires:** A module *exports* specific packages to make their public types accessible to other modules. A module *requires* another module to depend on its exported packages. This is a form of configuration at the module level.
- **Strong Encapsulation:** The module system enforces strong encapsulation. If a package is not exported, its public types are not accessible from outside the module, even via reflection (unless explicitly opened with an `opens` directive).
- **Reliable Configuration:** The module system resolves dependencies at both compile-time and runtime, checking if all required modules are present and if there are no version conflicts (e.g., multiple versions of the same module), eliminating many classpath-related errors.

### NIO and asynchronous I/O
- **Non-blocking I/O (NIO - Java 1.4):** `java.nio` introduced buffers (`ByteBuffer`), channels (`FileChannel`, `SocketChannel`), and selectors. It allows a single thread to manage multiple channels using a `Selector`, enabling non-blocking I/O operations.
- **Channels vs Streams:** Channels are bidirectional, can read and write asynchronously, and always operate on buffers. This is a more low-level and flexible model than the unidirectional streams in `java.io`.
- **Asynchronous I/O (NIO.2 - Java 7):** `java.nio.channels.Asynchronous*` classes (e.g., `AsynchronousSocketChannel`, `AsynchronousFileChannel`) provide true asynchronous I/O. Operations return immediately, and you are notified of completion via a `Future` or a `CompletionHandler`.
- **CompletionHandler:** This is the callback interface for true async operations. It has `completed(result, attachment)` and `failed(throwable, attachment)` methods, allowing you to define what to do when the I/O operation finishes.
- **Performance Benefits:** NIO and NIO.2 are crucial for building highly scalable servers that need to handle thousands of concurrent connections with a limited number of threads, as threads are not blocked waiting for I/O.

### Serialization mechanisms
- **Purpose:** Serialization is the process of converting an object's state into a byte stream, and deserialization is the reverse process of reconstructing the object from that byte stream. It's used for persistence, RMI, and sending objects over networks.
- **`Serializable` Interface:** A class must implement the marker interface `java.io.Serializable` to be serializable by the default mechanism. If a superclass does not implement it, its fields are not serialized.
- **`transient` Keyword:** Fields marked `transient` are ignored by the default serialization process and are not part of the serialized byte stream (e.g., for sensitive data like passwords, or derived values).
- **`serialVersionUID`:** This is a unique identifier for each serializable class. It is used during deserialization to verify that the sender and receiver of a serialized object have loaded classes for that object that are compatible with respect to serialization.
- **Customization:** You can provide custom serialization logic by implementing `writeObject()` and `readObject()` private methods in your class. This allows for encryption, compression, or validation of the serialized data.
- **Security Concerns and Alternatives:** Java's default serialization is often considered risky (security vulnerabilities) and inefficient. Modern alternatives include JSON (Jackson), Protocol Buffers, Avro, and Kryo.

### Custom class loading
- **Extending `ClassLoader`:** To create a custom class loader, you extend `java.lang.ClassLoader` and override methods, most importantly `findClass(String name)`.
- **`findClass` Implementation:** Inside `findClass`, you need to obtain the bytecode for the class (e.g., read it from a file, database, network, or decrypt it), call `defineClass()` to convert the byte array into a `Class` object, and optionally `resolveClass()` to link it.
- **Use Cases:** Custom class loaders are used for hot deployment in application servers (reloading classes without restarting the JVM), loading classes from encrypted or non-standard sources, implementing class versioning, and in frameworks like OSGi.
- **Parent Delegation:** It is crucial to respect the parent-delegation model. Typically, you only override `findClass`, and `loadClass` (which implements the delegation) is left untouched. Overriding `loadClass` directly is risky and can break the JVM's type safety.
- **Class Identity:** A class in the JVM is uniquely identified by its fully qualified name **and** its defining class loader. This is why custom class loaders can lead to `ClassCastException` if you try to cast an object of a class loaded by one loader to a class of the same name loaded by another.

### JNI basics
- **Purpose:** The Java Native Interface (JNI) is a framework that allows Java code running in the JVM to call (and be called by) native applications and libraries written in other languages like C, C++, and assembly.
- **Use Cases:** JNI is typically used for performance-critical code (e.g., graphics processing, complex math), to interact with platform-specific hardware or operating system features not accessible via Java, or to reuse legacy code written in other languages.
- **`native` Keyword:** Methods that are implemented in native code are declared in a Java class with the `native` modifier. These methods have no body in the Java source file.
- **Header File Generation:** The `javac` and `javah` (now integrated into `javac`) tools are used to generate a C/C++ header file (`.h`) from the Java class. This header contains function prototypes for the native methods.
- **Performance and Complexity:** JNI calls have overhead. Crossing the boundary between Java and native code is expensive. It also introduces significant complexity and risk, as the native code can crash the JVM, cause memory leaks, and is not platform-independent.

---

## Java Performance

### JVM performance fundamentals
- **Runtime Optimization:** The JVM is not just an interpreter; it's a sophisticated runtime environment that continuously optimizes the running application. Its performance characteristics are dynamic and adapt to the actual code being executed.
- **Key Components:** Overall JVM performance is a function of the garbage collector (managing heap memory), the JIT compiler (optimizing bytecode to native code), and the thread system (managing concurrency).
- **Warm-up Time:** JVM performance is not instantaneous. It takes time to load classes, and the JIT compiler needs time to profile method executions and compile "hot" paths to native code. Short-lived applications may not realize full JIT benefits.
- **Tuning vs. Code Quality:** While tuning JVM parameters (heap size, GC choice) is important, the most significant performance gains usually come from writing efficient, well-designed Java code with appropriate data structures and algorithms.
- **Metrics:** Key performance metrics include latency (response time), throughput (requests per unit time), memory footprint, and startup time. Tuning often involves balancing trade-offs between these metrics.

### Heap vs stack memory
- **Stack Memory:** Used for thread execution. Each thread has its own private stack, which stores method call frames. Each frame contains local primitive variables, references to objects, and partial results. Stack memory is fast, automatically allocated/deallocated, and has a smaller, fixed size (configurable per thread).
- **Heap Memory:** The single, shared runtime data area from which memory for all class instances and arrays is allocated. It is created when the JVM starts and is managed by the garbage collector. It is larger but slower to access than the stack.
- **Key Differences:** The stack is thread-private and stores execution contexts; the heap is shared and stores objects. The stack uses Last-In-First-Out (LIFO) allocation; the heap is a more complex pool. Stack overflow leads to `StackOverflowError`; heap exhaustion leads to `OutOfMemoryError`.
- **Object Storage:** Objects are always stored in the heap. Local variables containing primitives are on the stack. Local variables containing references are also on the stack, but the objects they point to are on the heap.
- **Escape Analysis:** A JIT optimization that can determine if an object created within a method "escapes" that method or thread. If it doesn't, the object may be allocated on the stack instead of the heap, a technique called *stack allocation*, which can reduce GC pressure.

### Garbage collection algorithms
- **Marking:** The first phase. The GC traverses the object graph starting from GC roots (thread stacks, static variables, JNI handles) and marks all objects that are still reachable (live).
- **Sweeping:** The GC sweeps through the heap, identifying unmarked (unreachable) objects and reclaiming their memory. This can lead to memory fragmentation.
- **Copying:** This algorithm (used in young generation collectors) moves all live objects from one memory region (e.g., Eden) to another (e.g., Survivor space), effectively compacting the memory. The original region is then considered entirely free. It's efficient but requires more memory.
- **Mark-Compact:** A combination approach. It marks live objects, then slides them together to a contiguous area at the beginning of the heap, eliminating fragmentation. This is typical for old generation collectors (like in G1 and CMS).
- **Generational Hypothesis:** Most GC algorithms are based on the weak generational hypothesis: most objects die young. This leads to splitting the heap into a young generation (for short-lived objects) and an old generation (for longer-lived objects), allowing collectors to optimize accordingly.

### G1, ZGC, Shenandoah
- **G1 (Garbage-First) - Default since Java 9:** A server-style, region-based, generational collector designed to provide a good balance between high throughput and low latency, with a configurable pause time goal. It divides the heap into many regions and collects regions with the most garbage first.
- **ZGC (Java 15+ Production):** A scalable, low-latency, non-generational (for now) collector. Its primary goal is to keep pause times very low (sub-millisecond) regardless of heap size. It achieves this through concurrency and the use of colored pointers and load barriers.
- **Shenandoah (Java 15+ Production - Not in all builds):** Similar to ZGC in its goal of ultra-low pause times. It reduces pause times by doing more concurrent work, including concurrent evacuation (moving objects) with the application threads, using Brooks pointers.
- **Trade-offs:** G1 offers a good trade-off. ZGC and Shenandoah trade some throughput (CPU overhead) and potentially memory footprint for drastically reduced and consistent pause times, making them ideal for large heaps and latency-sensitive applications.
- **Use Cases:** G1 is a great all-rounder. ZGC and Shenandoah are for applications with very large heaps (hundreds of GB or TB) where pause times must be minimal, such as high-frequency trading platforms or large in-memory databases.

### GC tuning strategies
- **Goal Definition:** The first step in tuning is to define clear, measurable goals: e.g., maximum acceptable pause time (e.g., `< 100ms`), minimum throughput percentage (e.g., `> 98%`), or a specific memory footprint.
- **Right-Sizing the Heap:** The most fundamental tuning. A heap that is too small leads to frequent GC cycles and `OutOfMemoryError`. A heap that is too large can lead to longer full GC pauses (depending on the collector) and wasted resources.
- **Selecting the Collector:** Choose the collector based on the application's performance goals. Use G1 for a balance (default). Switch to ZGC or Shenandoah for ultra-low-latency requirements. The Parallel collector (`-XX:+UseParallelGC`) might be chosen for batch jobs where maximizing throughput is the only goal.
- **Tuning Generational Sizes:** For generational collectors, the size of the young generation is a critical parameter. A larger young generation reduces promotion frequency but increases young GC pause times. The ratio between Eden and Survivor spaces also matters.
- **Flags and Monitoring:** Use JVM flags like `-Xms` (initial heap), `-Xmx` (max heap), `-XX:NewRatio`, `-XX:MaxGCPauseMillis` (for G1) as starting points. Crucially, tuning must be guided by monitoring tools (like JVisualVM, GC logs) to measure the actual impact of changes.

### JIT compilation
- **Just-In-Time Compilation:** The JIT compiler is a key component of the JVM that improves performance by compiling "hot" (frequently executed) bytecode methods into native machine code at runtime.
- **Interpretation vs. Compilation:** The JVM starts by interpreting bytecode, which is slow. As the application runs, it profiles method invocations and loop counts. When a method's hotness threshold is crossed, the JIT compiles it to native code, which then runs much faster.
- **Tiered Compilation (Default since Java 7):** The JVM uses multiple compilation levels. It starts with the client compiler (C1), which compiles quickly but with fewer optimizations. For very hot methods, it may use the server compiler (C2), which performs more aggressive optimizations but takes longer to compile. This provides fast warm-up and high peak performance.
- **Code Cache:** The compiled native code is stored in a special memory area called the code cache. If the code cache becomes full, the JVM cannot compile more methods, leading to a performance drop.
- **Deoptimization:** In rare cases, the JIT may need to "deoptimize" previously compiled code. This can happen if new classes are loaded that change the assumptions under which the original optimization was made, forcing the JVM to revert to interpretation for a while.

### Escape analysis
- **Definition:** Escape Analysis is a code analysis technique performed by the JIT compiler to determine the dynamic scope of an object – i.e., where the object is used.
- **No Escape:** An object created in a method does not escape if it is not passed as a parameter to another method, not returned from the method, and not assigned to a static or instance field.
- **Stack Allocation (Optimization):** If an object does not escape the method, the JIT can perform *scalar replacement* and allocate the object's fields on the stack or directly in CPU registers, instead of allocating it on the heap. This dramatically reduces GC pressure.
- **Synchronization Removal (Optimization):** If an object that uses synchronization (e.g., a `StringBuffer`) is shown not to escape a thread, the JIT can remove the lock acquisition, a process known as *lock elision*.
- **On by Default:** Escape analysis is enabled by default in modern JVMs (`-XX:+DoEscapeAnalysis`). It's a powerful, behind-the-scenes optimization that doesn't require code changes but makes local object creation much cheaper.

### Allocation optimization
- **TLABs (Thread-Local Allocation Buffers):** To avoid contention when multiple threads allocate objects in the shared heap, each thread is given a private buffer (a TLAB) from the Eden space. The thread can allocate objects quickly into its own TLAB without needing synchronization.
- **Bump-the-Pointer Allocation:** Allocation within a TLAB (or a compacted region) is extremely fast. It's essentially just a pointer bump: the allocated memory address is the current pointer, and the pointer is then advanced by the object's size. This is much faster than a more complex free-list lookup.
- **Escape Analysis and Scalar Replacement:** As mentioned earlier, this is the ultimate allocation optimization. If an object doesn't escape, its fields can be "replaced" with primitives on the stack/registers, avoiding heap allocation entirely.
- **Object Pooling is Often Harmful:** In modern JVMs with fast allocation and efficient GC, manually pooling objects (like connection pools for short-lived objects) can actually be detrimental. It increases tenured generation pressure and can lead to more expensive full GCs than simply letting short-lived objects die young.
- **Allocation Profiling:** Tools like Java Flight Recorder (JFR) can profile allocation rates and identify hot allocation sites, helping developers pinpoint where to optimize code to reduce the number of created objects.

### Thread performance
- **Context Switching:** When the OS or JVM switches from executing one thread to another, a context switch occurs. This involves saving the state of the current thread and loading the state of the next. It's a relatively expensive operation that should be minimized.
- **Parking and Unparking:** Threads that are blocked (e.g., waiting for a lock, I/O, or a condition) are "parked" and do not consume CPU cycles. Efficiently managing when threads are parked vs. running is key to performance.
- **True Parallelism vs. Concurrency:** On a single-core machine, threads provide concurrency (interleaved execution) but not true parallelism. On multi-core systems, the JVM and OS can schedule threads to run in parallel on different cores, enabling true simultaneous execution.
- **Synchronization Overhead:** Using `synchronized` or `Lock`s ensures thread safety but introduces overhead. This overhead includes the cost of acquiring/releasing the lock and the potential for contention, which can serialize access and degrade performance.
- **Thread Locality:** Keeping data thread-local (using `ThreadLocal` variables) avoids contention and can improve cache performance, as the data is only accessed by one thread and can reside in that core's cache.

### Lock contention
- **Definition:** Lock contention occurs when one thread is trying to acquire a lock that is already held by another thread. The contending thread is forced to block (or spin) until the lock is released, which reduces parallelism and throughput.
- **Coarse-Grained vs. Fine-Grained Locking:** Coarse-grained locking (using a single lock for a large data structure) is simple but leads to high contention. Fine-grained locking (e.g., lock striping, using separate locks for different parts of a data structure like in `ConcurrentHashMap`) reduces contention but increases complexity and the risk of deadlocks.
- **Lock Elision:** An optimization where the JIT compiler removes unnecessary locking if it can prove that the lock object is not shared across threads (e.g., a local `StringBuffer` that doesn't escape the method).
- **Lock Coarsening:** An optimization where the JIT compiler merges a series of adjacent, fine-grained locking operations on the same object into a single, coarser-grained lock block. This reduces the overhead of frequent lock acquisitions.
- **Alternatives to Blocking:** Using non-blocking algorithms based on CAS (Compare-And-Swap) instructions, or using `java.util.concurrent` data structures like `ConcurrentLinkedQueue` and `Atomic*` classes, can avoid blocking contention altogether.

### CPU profiling
- **Purpose:** CPU profiling is used to identify methods or code paths that are consuming the most CPU time, also known as "hot spots." This helps developers focus optimization efforts where they will have the most impact.
- **Sampling Profilers:** These profilers periodically take snapshots of the call stack of all running threads. By statistically analyzing these samples, they can estimate where time is being spent. They have low overhead but may miss short-lived methods.
- **Instrumenting Profilers:** These profilers inject code into the compiled methods to record entry and exit times. They provide more precise data but introduce significant overhead, potentially altering the performance characteristics of the application.
- **On- vs Off-CPU Analysis:** CPU profiling focuses on time spent executing on the CPU. It's equally important to understand why threads are *not* on the CPU (off-CPU), such as being blocked on I/O, locks, or waiting for network. This requires tracing or other tools.
- **Tools:** Common tools include Java Flight Recorder (JFR), which is a low-overhead, built-in profiler, and standalone profilers like async-profiler (which can do both CPU and allocation profiling), JProfiler, and VisualVM.

### Memory profiling
- **Purpose:** Memory profiling helps identify memory-related issues like memory leaks, high garbage collection overhead, and inefficient memory usage. It focuses on object allocations, object lifetimes, and heap dumps.
- **Heap Dump Analysis:** A heap dump is a snapshot of all objects in the heap at a specific moment. Analyzing a heap dump with tools like Eclipse Memory Analyzer (MAT) or JProfiler can reveal which objects are consuming the most memory, who is holding references to them (GC roots), and potential leak suspects.
- **Allocation Profiling:** This tracks where objects are being created. It can identify "allocation hot spots" in the code, allowing you to see which methods are generating the most garbage and contributing to GC pressure.
- **Live Memory Tracking:** Tools can track the rate of object allocation and promotion between generations. Spikes in allocation rate or a steadily growing old generation are key indicators of problems.
- **GC Log Analysis:** Analyzing GC logs (using tools like GCeasy) provides insights into heap sizes, pause times, and frequency of collections, which are indirect but valuable memory profiling data.
- **Leak Detection:** By taking multiple heap dumps over time and comparing them, you can identify classes or data structures whose instance count or retained heap size is continuously growing, indicating a memory leak.

### Throughput vs latency tuning
- **Throughput:** The amount of work a system can handle in a given unit of time (e.g., requests per second, jobs per hour). High throughput means maximizing the system's processing capacity.
- **Latency:** The time it takes to process a single request or unit of work (e.g., response time in milliseconds). Low latency means the system is very responsive.
- **The Trade-off:** These two metrics are often at odds. Techniques that maximize throughput, like batching or using large, infrequent operations, can increase latency for an individual item. Similarly, striving for the lowest possible latency for every request (e.g., with very frequent, fine-grained operations) can reduce overall throughput due to overhead.
- **GC Tuning Trade-off:** In GC tuning, using a parallel collector might maximize throughput by pausing all application threads for a short period. Using a concurrent collector (like G1, ZGC) trades some CPU throughput to keep pauses (latency) low.
- **Application-Level Tuning:** At the code level, this trade-off appears in decisions like connection pool sizes (a very large pool might increase latency due to contention, while a very small pool might limit throughput), or caching strategies.

### Performance benchmarking
- **Purpose:** Benchmarking is the systematic process of running a series of tests to evaluate the relative performance of a system, component, or piece of code. It's about establishing a reliable performance baseline.
- **JMH (Java Microbenchmark Harness):** This is the gold standard for creating correct microbenchmarks in Java. It handles many common pitfalls like JVM warm-up, dead code elimination, and constant folding, ensuring that benchmarks measure what they intend to.
- **Common Pitfalls:** Microbenchmarking is notoriously difficult. Common mistakes include not warming up the JVM (so results include compilation time), not measuring the right thing (e.g., measuring I/O instead of computation), and having the optimizer eliminate the code being tested because its result is unused.
- **Macro-benchmarking:** This involves testing the application as a whole (end-to-end), using realistic workloads. It's essential for understanding overall system performance, including network, database, and other external factors, but results are harder to isolate to specific code changes.
- **Repeatability:** A good benchmark must be repeatable. Results should be consistent across multiple runs in a controlled environment to be meaningful. It's important to run benchmarks on a quiet system (not your development laptop) and to report statistical measures (mean, percentiles).

### Load testing
- **Purpose:** Load testing simulates real-world usage by applying expected or increasing levels of load to an application to observe its behavior under stress. The goal is to identify performance bottlenecks and determine the system's breaking point.
- **Concurrency and Ramp-Up:** Load tests involve simulating a specific number of concurrent users (threads) making requests. A proper test will often "ramp up" the number of users gradually to see how the system handles increasing load.
- **Tools:** Common tools for load testing include Apache JMeter, Gatling, and k6. These tools allow you to script user behavior, define the load profile, and collect performance metrics like response times and error rates.
- **Metrics to Watch:** During a load test, you need to monitor application-side metrics (CPU, memory, GC activity, thread pool states) as well as client-side metrics (throughput, latency percentiles - e.g., p95, p99). A spike in p99 latency often indicates the system is nearing its limit.
- **Identifying Bottlenecks:** Load testing helps pinpoint the weakest link. Is it the application's CPU? Is it the database connection pool? Is it the external API call? By monitoring various resources, you can identify where the bottleneck first appears.

### Thread pool tuning
- **Purpose:** Thread pools manage a pool of worker threads to execute tasks. They avoid the overhead of creating and destroying threads for each task and help control resource usage.
- **Core Pool Size:** This is the number of threads that are kept alive in the pool, even if they are idle. This should be sized based on the nature of the tasks and the number of available cores.
- **Maximum Pool Size:** This is the maximum number of threads that can be created. When the queue is full, new threads may be created up to this limit.
- **Work Queue:** The type of queue used for pending tasks is critical.
    - **Unbounded Queue** (e.g., `LinkedBlockingQueue`): Can lead to high memory consumption if tasks are produced faster than they can be consumed.
    - **Bounded Queue** (e.g., `ArrayBlockingQueue`): Allows for a rejection policy to be triggered when the queue is full and max pool size is reached.
- **Sizing Heuristics:** The optimal pool size depends on the task type.
    - **CPU-Bound Tasks:** Size should be close to the number of cores (`Runtime.getRuntime().availableProcessors() + 1`).
    - **I/O-Bound Tasks:** A much larger pool is often beneficial because threads will spend a lot of time blocked, allowing others to run. This can be calculated as `number of cores * (1 + wait time / service time)`.
- **Rejection Policies:** When a task cannot be submitted (queue full, pool at max), the `RejectedExecutionHandler` defines the behavior. Options include `AbortPolicy` (throws exception), `CallerRunsPolicy` (runs task in caller's thread, which can act as a back-pressure mechanism), `DiscardPolicy`, and `DiscardOldestPolicy`.

### I/O optimization
- **Buffering:** Using buffered streams (`BufferedReader`, `BufferedOutputStream`) is the most fundamental I/O optimization. It reduces the number of system calls by reading/writing data in larger chunks, dramatically improving performance.
- **Asynchronous I/O:** Using NIO.2's asynchronous channels (`AsynchronousFileChannel`, `AsynchronousSocketChannel`) allows a thread to initiate an I/O operation and continue doing other work, being notified upon completion. This improves thread utilization.
- **Non-blocking I/O with Selectors:** Using `java.nio.channels.Selector` with non-blocking channels allows a single thread to monitor multiple channels for readiness (e.g., ready to read/write). This is the foundation for highly scalable network servers.
- **Memory-Mapped Files:** `MappedByteBuffer` allows you to map a region of a file directly into memory. Reading and writing to the file becomes as simple as reading and writing to a byte array, with the OS handling the paging. This can be extremely fast for large files.
- **Zero-Copy:** Techniques like `FileChannel.transferTo()` and `transferFrom()` allow data to be copied directly from a file to a network socket (or vice-versa) without passing through the application's memory space, reducing CPU overhead and memory bandwidth usage.

### Cache optimization
- **CPU Cache Levels (L1, L2, L3):** Modern CPUs have multiple levels of cache, with L1 being the smallest and fastest, and L3 being larger and slower (but still much faster than main memory). Optimizing for cache means writing code that keeps frequently accessed data in these caches.
- **Locality of Reference:**
    - **Spatial Locality:** Accessing data elements that are stored close together in memory. Iterating through an array sequentially exhibits excellent spatial locality.
    - **Temporal Locality:** Accessing the same data elements repeatedly within a short time frame. This data can stay in the cache.
- **Data Structure Choice:** Linked data structures like `LinkedList` have poor spatial locality because nodes can be scattered across the heap. Arrays and `ArrayList` provide excellent spatial locality. Contiguous memory layouts (like that of primitive arrays) are best for cache performance.
- **False Sharing:** A performance anti-pattern where threads on different cores modify variables that happen to reside on the same CPU cache line. Even though the variables are independent, the cache coherency protocol forces the entire cache line to be treated as shared, leading to significant performance degradation.
- **Object Sizing and Padding:** The memory layout of Java objects (object headers, fields, alignment) affects how they occupy cache lines. Padding can be used (e.g., using `@Contended` annotation in Java) to ensure that highly contended fields are on their own cache lines to prevent false sharing.

### Object pooling tradeoffs
- **Definition:** Object pooling is a creational design pattern where a set of initialized objects is kept ready to use, rather than allocated and destroyed on demand. A client requests an object from the pool and returns it after use.
- **When it Can Be Beneficial:**
    - **Creation Cost:** When object creation is expensive (e.g., database connections, threads, large buffers).
    - **Resource Limitation:** To limit the total number of a certain resource in use at any time (e.g., database connections).
- **When it is Harmful (Modern JVM):** For most regular Java objects, pooling is **anti-pattern**.
    - **GC Efficiency:** Modern GCs are extremely efficient at handling short-lived objects. Object pools cause objects to be long-lived, promoting them to the old generation and increasing the cost of major GC collections.
    - **Memory Overhead:** The pool itself holds references to objects, preventing them from being GC'd. The pool also needs to be managed, which adds code complexity and potential concurrency bottlenecks.
- **Maintenance Complexity:** Pools require careful management: checking object validity, handling "stale" objects, and ensuring proper concurrency control for pool access. A leak in a pool can be disastrous.
- **Recommendation:** Only consider pooling for resources that are genuinely expensive to create or limited (like database connections in a connection pool, threads in a thread pool). Do not create pools for simple objects like `String`, collections, or DTOs.

### False sharing
- **Definition:** False sharing is a performance-degrading phenomenon that can occur in multi-threaded systems when threads on different processors modify variables that are logically independent but reside on the same CPU cache line.
- **Cache Coherency:** When one core modifies a byte in a cache line, the cache coherency protocol invalidates the entire cache line on other cores. Even though the other cores wanted to modify a *different* part of that line, they now have to fetch the updated line from main memory or a shared cache.
- **Consequences:** This results in unintended and excessive cache coherency traffic, making the threads appear to be contending for the same resource when they are not. It can severely impact scalability.
- **Detection:** False sharing is difficult to detect without performance counters. A symptom is that an application's performance doesn't scale with the number of cores. Profiling tools and hardware performance counters (e.g., for last-level cache misses) can help identify it.
- **Mitigation:**
    - **Padding:** Add unused fields (padding) around your shared variables to ensure they occupy their own cache lines. The `@sun.misc.Contended` annotation (or `@jdk.internal.vm.annotation.Contended` in newer Java) can be used to request this automatically.
    - **Restructure Data:** Change the design so that threads operate on thread-local data or on larger, independent data structures that are naturally cache-line-aligned.

### NUMA awareness
- **Definition:** NUMA (Non-Uniform Memory Access) is a computer memory design used in multi-socket systems. Each processor has its own local memory, and accessing that local memory is faster than accessing memory attached to another processor (remote memory).
- **The NUMA Problem:** In a NUMA system, if a thread running on socket A allocates memory, it will be allocated from socket A's local memory (its "home node"). If that thread is later migrated to socket B, all its memory accesses are now to a remote node, which is slower.
- **JVM NUMA Awareness (`-XX:+UseNUMA`):** On supported systems and with certain garbage collectors (like G1 and Parallel Scavenger), this flag enables the JVM to be NUMA-aware. It attempts to allocate memory for threads on the same node the thread is running on.
- **Heap Allocation Strategy:** With `UseNUMA` enabled, the JVM can split the young generation into multiple regions, one per NUMA node. A thread will allocate objects into its local node's young generation region, promoting good locality.
- **Benefits:** For large-scale applications running on multi-socket servers, enabling NUMA awareness can lead to significant performance improvements by reducing remote memory access latency. The effectiveness depends on the application's memory access patterns and the underlying hardware.

---

## Java Instrumentation

### Java agent architecture
- **Purpose:** A Java agent is a special type of library that can instrument bytecode running in a JVM. It is a powerful tool for monitoring, profiling, and manipulating applications without changing their source code.
- **Entry Points:** An agent must provide one of two static methods:
    - `premain(String agentArgs, Instrumentation inst)`: The entry point for an agent loaded at JVM startup (using `-javaagent`).
    - `agentmain(String agentArgs, Instrumentation inst)`: The entry point for an agent loaded dynamically into a running JVM (using the Attach API).
- **Manifest Attributes:** The agent JAR file must have specific attributes in its manifest (`MANIFEST.MF`) to be recognized: `Premain-Class` (for startup agents), `Agent-Class` (for dynamic agents), `Can-Redefine-Classes`, and `Can-Retransform-Classes`.
- **`Instrumentation` Interface:** The JVM passes an instance of `java.lang.instrument.Instrumentation` to the agent's entry method. This interface is the primary API for the agent to perform its tasks, such as adding transformers and querying class information.
- **Isolation:** Agent classes are loaded by a separate classloader (the system classloader), which isolates them from the target application's classes to a certain degree, reducing the risk of interference.

### Bytecode instrumentation
- **Definition:** Bytecode instrumentation is the process of modifying the bytecode of a compiled Java class to add, remove, or change instructions. This happens after the class is loaded but before it is defined by the JVM.
- **Transformers:** In Java agents, instrumentation is achieved by registering a `ClassFileTransformer`. The transformer's `transform` method is called whenever a class is loaded or redefined, allowing the agent to provide a new byte array representing the transformed class.
- **Use Cases:** Common uses include adding performance monitoring (timing methods), injecting logging, implementing security checks, profiling (counting method calls), and implementing AOP at the bytecode level.
- **In-Place vs. Build-Time:** Instrumentation can happen at runtime (as described with agents) or at build time (e.g., as part of the compilation process), where class files are modified before they are even packaged into a JAR.
- **Challenges:** Bytecode instrumentation is complex and risky. It requires a deep understanding of the JVM specification, can cause class verification errors, and must be handled carefully to avoid performance degradation or instability.

### java.lang.instrument API
- **Core Package:** The `java.lang.instrument` package provides the services that allow agents to instrument programs running on the JVM.
- **`Instrumentation` Interface:** This is the central interface. Key methods include:
    - `addTransformer(ClassFileTransformer transformer)`: Registers a transformer for future class definitions.
    - `retransformClasses(Class<?>... classes)`: Retransform already loaded classes using registered transformers.
    - `redefineClasses(ClassDefinition... definitions)`: Replace the definition of a class with a new one (more limited than retransformation).
    - `getAllLoadedClasses()`: Returns an array of all classes currently loaded by the JVM.
    - `isModifiableClass(Class<?> theClass)`: Checks if a class can be retransformed or redefined.
- **`ClassFileTransformer` Interface:** The single method `transform` is the heart of the instrumentation. It receives the class loader, class name, and the original class file bytes, and must return a new byte array (or `null` to indicate no transformation).
- **Retransformation vs. Redefinition:** Retransformation (`retransformClasses`) applies the existing registered transformers to an already loaded class. Redefinition (`redefineClasses`) replaces the class bytecode directly, bypassing transformers. Retransformation is more common.
- **Limitations:** There are restrictions on what changes can be made. You can generally change method bodies, but you typically cannot add, remove, or rename fields or methods, as that would break existing references.

### JVMTI basics
- **Definition:** The JVM Tool Interface (JVMTI) is a low-level native programming interface that allows for the creation of tools that can inspect and control a JVM. It is the foundation upon which higher-level tools like profilers, debuggers, and monitoring agents are built.
- **Native Interface:** Unlike the `java.lang.instrument` API which is Java-based, JVMTI is a native (C/C++) interface. It provides the most comprehensive and low-level access to the JVM's internals.
- **Capabilities:** JVMTI provides a vast array of events and functions, including being able to get information about all threads, monitor object allocation, control garbage collection, set breakpoints (for debuggers), and get stack traces.
- **Relationship with `java.lang.instrument`:** The `java.lang.instrument` API is often implemented on top of JVMTI. When you write a Java agent, the JVM uses JVMTI in the background to provide the `Instrumentation` capabilities.
- **Use Cases:** JVMTI is used for building sophisticated tools like profilers (YourKit, JProfiler), debuggers, and monitoring solutions. It is not typically used for application-level AOP or simple monitoring, which are better handled by the Java-level instrumentation API.

### Runtime instrumentation
- **Definition:** Runtime instrumentation refers to the process of modifying the bytecode of classes that are already loaded and running in the JVM, without restarting the application.
- **Dynamic Agent Attachment:** This is achieved by attaching a Java agent to a running JVM using the Attach API (`com.sun.tools.attach`). Once attached, the agent's `agentmain` method is invoked, and it can use the `Instrumentation` API to retransform classes.
- **Retransformation:** The key mechanism for runtime instrumentation is `instrumentation.retransformClasses()`. This triggers the registered `ClassFileTransformer` to run again on the specified, already-loaded classes, allowing you to modify their behavior on the fly.
- **Use Cases:** Runtime instrumentation is invaluable for production debugging (e.g., adding logging to a specific method to diagnose an issue), enabling/disabling features without a restart, or for dynamic profiling and monitoring.
- **Challenges:** Modifying classes at runtime is more complex and risky than startup instrumentation. It requires careful management of class state, and can lead to inconsistencies if not handled correctly. The JVM also imposes restrictions on what changes can be made (e.g., no schema changes to the class).

### Build-time instrumentation
- **Definition:** Build-time instrumentation modifies the bytecode of class files during the build process, before the application is packaged and deployed. The instrumented classes are then packaged into the final JAR/WAR.
- **How it Works:** This is typically done as a step in the build tool (Maven, Gradle). A plugin (e.g., for AspectJ) or a custom task processes the compiled `.class` files, applies transformations, and overwrites them or places them in the output directory.
- **Advantages:** It avoids the runtime overhead of a Java agent's transformation step (no transformer needs to run at class load time). It also simplifies deployment, as you just deploy a standard JAR with pre-instrumented classes. It works in environments where agents are not allowed.
- **Disadvantages:** The instrumentation is static. You cannot change the behavior without rebuilding and redeploying the application. It also couples the instrumentation logic with your build process.
- **Use Cases:** Build-time instrumentation is commonly used for Aspect-Oriented Programming (AOP) with compile-time weaving, for tools like JaCoCo (code coverage) that can instrument classes offline, and for certain performance optimizations or code enhancements that are known at build time.

### AOP instrumentation
- **Aspect-Oriented Programming:** AOP is a programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns (e.g., logging, security, transaction management) from the main business logic.
- **Weaving:** The process of integrating aspects with the main application code is called weaving. This can happen at different times: compile-time, load-time, or runtime.
- **Load-Time Weaving (LTW):** This is a form of runtime instrumentation where the weaving is performed by a Java agent as classes are being loaded into the JVM. The agent uses a `ClassFileTransformer` to apply aspect advice to the class bytecode on-the-fly.
- **Implementation:** Popular AOP frameworks like AspectJ support LTW. They provide a Java agent that reads aspect definitions (from `aop.xml`) and transforms classes as they load.
- **Advantages of AOP Instrumentation:** It provides a clean, declarative way to add behavior (e.g., `@Transactional`) without cluttering the core code. It makes the system more modular and easier to maintain by centralizing cross-cutting concerns.

### Bytecode manipulation libraries
- **Why Use Libraries?:** Directly editing bytecode is extremely tedious, error-prone, and requires deep knowledge of the JVM instruction set. Bytecode manipulation libraries provide a higher-level API to read and modify class files safely.
- **Key Tasks:** These libraries allow you to parse an existing class file, analyze its structure (methods, fields, annotations), insert new bytecode instructions at specific points, and write the modified class file back.
- **Common Uses:** They are the foundation for many tools, including Java agents, code coverage tools (JaCoCo), mocking frameworks (Mockito), persistence frameworks (Hibernate's bytecode enhancement), and AOP implementations.
- **Trade-offs:** The libraries differ in their API style (low-level event-based vs. high-level object-oriented) and performance characteristics. Choosing the right one depends on the complexity of the transformation and performance requirements.

### ASM basics
- **Nature:** ASM is a low-level, high-performance bytecode manipulation library. It is based on the Visitor and Tree design patterns and provides a very detailed and efficient way to work with Java bytecode.
- **Core Model:** ASM operates primarily through a *visitor* API. As it reads a class file, it generates events (like `visitMethod`, `visitField`, `visitLdcInsn`). You implement a `ClassVisitor` and override the methods you're interested in to modify the class.
- **Performance:** Because of its event-based, streaming approach, ASM is extremely fast and has a very small memory footprint. It is widely used in performance-critical tools and frameworks, including the JVM's own lambda generation.
- **Complexity:** The visitor-based API has a steep learning curve. You need to have a good understanding of the JVM instruction set and class file structure to use it effectively. A simple mistake can easily corrupt a class file.
- **Tree API:** ASM also offers a higher-level "Tree" API, which parses the entire class into an object tree, allowing for easier but less memory-efficient manipulation. This is often a better starting point for complex transformations before diving into the visitor API.

### Javassist
- **Nature:** Javassist (Java Programming Assistant) is a high-level, reflection-based bytecode manipulation library. Its main philosophy is to make bytecode manipulation simple, by allowing you to work with source-level syntax.
- **Core Model:** Javassist allows you to load a class file into a `CtClass` (compile-time class) object. You can then modify it by using high-level methods like `addMethod()`, `addField()`, or by inserting Java source code snippets directly (e.g., `insertBefore()`, `insertAfter()`).
- **Ease of Use:** The ability to insert source code snippets that Javassist compiles on the fly makes it much easier to use than ASM for many tasks. You don't need to understand bytecode instructions to do basic instrumentation.
- **Performance:** Javassist's higher-level abstraction comes with a performance cost. It is generally slower than ASM, both for the instrumentation process and for the resulting code (unless the code is carefully written). It also has a larger memory footprint during transformation.
- **Use Cases:** It is excellent for rapid prototyping, simple instrumentation tasks, and in scenarios where developer productivity is more important than absolute peak performance of the instrumentation engine.

### Byte Buddy
- **Nature:** Byte Buddy is a modern, high-level bytecode manipulation library that aims to provide an expressive, type-safe API while retaining good performance. It can be seen as a more advanced alternative to Javassist and a more user-friendly alternative to ASM.
- **Fluent, Type-Safe API:** Its API is designed to be fluent and readable. You create a `DynamicType.Builder`, define what you want to change (e.g., `method(named("toString")).intercept(FixedValue.value("Hello!"))`), and Byte Buddy generates the necessary bytecode.
- **Performance:** Byte Buddy is designed to have a small runtime footprint. It can generate code that performs as well as handwritten ASM code. It is the default bytecode engine for many modern frameworks, including Mockito and Hibernate.
- **Runtime and Build-Time:** Byte Buddy can be used both for runtime instrumentation (via a Java agent) and for build-time processing. It has excellent support for creating Java agents.
- **Flexibility:** It supports complex instrumentation scenarios, including advice (similar to AOP), member substitution, and the creation of new classes entirely from scratch. It abstracts away many of the low-level details of ASM, making it powerful yet accessible.

### Class transformation
- **Definition:** Class transformation is the core act of modifying the bytecode of a class. It is the fundamental operation performed by all instrumentation tools.
- **Transformation Stages:** A class can be transformed at different points in its lifecycle:
    - **Load-time:** When the class is first loaded by the JVM (using a `premain` agent).
    - **Runtime (Retransformation):** After the class is already loaded (using an `agentmain` agent).
    - **Build-time:** Before the class is even packaged into a JAR.
- **The Transformer Chain:** When multiple `ClassFileTransformer`s are registered, they are called in a chain. The output of one transformer becomes the input for the next. The order of registration can be important.
- **Input and Output:** The `transform` method receives the original class file bytes. It returns a new byte array. The returned byte array must be a valid class file format; otherwise, the JVM will throw a `ClassFormatError`.
- **Constraints:** The JVM imposes constraints on transformations. For example, you cannot generally change the class's parent, the interfaces it implements, or its fields, as this would break the JVM's internal type system and existing references. These constraints are slightly relaxed for retransformation, but still significant.

### Method interception
- **Definition:** Method interception is a specific type of bytecode instrumentation where the agent modifies a method to allow external code to run before, after, or instead of the method's original body. This is the foundation for many AOP implementations.
- **Techniques:**
    - **Advice Injection:** Bytecode is inserted directly into the method's body at the entry, exit, and around exception-throwing points. This is the most efficient technique.
    - **Proxy-based Interception:** The original object is replaced with a proxy (dynamic or bytecode-generated) that wraps the original and adds behavior before/after delegating. This is common in Spring AOP when not using load-time weaving.
    - **Method Entry/Exit Hooks:** The bytecode is modified to call a static hook method at the beginning and end, passing information about the method and its arguments.
- **Context Information:** Interception often requires capturing context, such as the current object (`this`), method arguments, and the return value. This information must be passed to the interceptor, which can be challenging at the bytecode level.
- **Performance Considerations:** The performance impact of method interception depends on the technique. Injected advice can be very fast. Complex proxy chains or reflection-based invocation in the interceptor can add significant overhead.
- **Use Cases:** This is used for implementing cross-cutting concerns like:
    - **Profiling:** Timing method execution.
    - **Logging/ Tracing:** Automatically logging method entry and exit.
    - **Security:** Checking permissions before method execution.
    - **Transactions:** Starting/committing a transaction around a method.

### Profiling agents
- **Purpose:** Profiling agents are Java agents specifically designed to collect performance data about a running application. They are the technology behind many commercial and open-source profilers.
- **How They Work:** A profiling agent uses instrumentation (and sometimes JVMTI) to:
    - **CPU Profiling:** Inject code to record method entry/exit times or, more commonly, use sampling by periodically capturing stack traces via `Thread.getAllStackTraces()` or JVMTI.
    - **Memory Profiling:** Track object allocations by instrumenting `new` bytecode instructions or by using JVMTI events for object allocation.
    - **Thread Profiling:** Monitor thread states, contention, and locks.
- **Data Collection and Reporting:** The agent collects this data, often aggregates it to reduce overhead, and then either writes it to a local file (like JFR) or sends it to a remote collector for analysis and visualization.
- **Low Overhead is Key:** The primary challenge for a profiling agent is to collect useful data with minimal impact on the application's performance. Sampling is used to keep overhead low, while instrumentation-based profilers are more precise but have higher overhead.
- **Async Profiler:** A notable example of a modern, low-overhead profiling agent that combines sampling techniques with `perf_events` and eBPF to get very accurate CPU and allocation profiles with minimal impact.

### Monitoring agents
- **Purpose:** Monitoring agents are Java agents designed to collect and expose telemetry data (metrics, traces, logs) about an application's health and performance for operational monitoring. They are the "observability" agents.
- **Data Collection:** They collect a wide range of data:
    - **JVM Internals:** GC activity, memory pool usage, thread counts, class loading.
    - **Application Metrics:** Request rates, error rates, custom business metrics.
    - **Distributed Tracing:** Intercepting HTTP requests or messaging calls to propagate trace context and record spans.
- **Integration with Observability Tools:** These agents typically integrate with back-end systems like Prometheus (exposing a metrics endpoint), Grafana, Zipkin/Jaeger (sending traces), and logging aggregators.
- **Examples:** OpenTelemetry Java Agent is a prime example. It automatically instruments many popular libraries and frameworks (like Spring Boot, JDBC, Apache HttpClient) to capture traces and metrics without any code changes.
- **Configuration:** A key feature is their configurability. Operators can enable/disable specific instrumentations, set sampling rates, and configure exporters without modifying the application code, often through environment variables or a configuration file.

---

## Java Observability

### Observability principles
- **Definition:** Observability is a property of a system that measures how well its internal states can be inferred from its external outputs (telemetry data). It's about understanding *why* something is happening, not just *what* is happening.
- **The Three Pillars:** Observability is traditionally built on three pillars:
    - **Logs:** Discrete events with a timestamp and a message (e.g., error stack traces, audit records).
    - **Metrics:** Aggregated, numeric measurements over time (e.g., request rate, CPU usage, error count).
    - **Traces:** Representation of a request's journey through a distributed system, showing causal relationships between services.
- **High Cardinality:** A key concept is the ability to handle high-cardinality data (e.g., user IDs, request IDs, credit card numbers in traces). Traditional monitoring tools often struggled with this, but modern observability tools are built for it.
- **Intent:** The ultimate goal is to be able to ask novel questions about a system's behavior and get answers without having to predict every possible failure mode in advance and write a specific dashboard for it.
- **Correlation:** The true power of observability comes from being able to correlate between the pillars. For example, a high latency metric (metrics) can be investigated by looking at a slow trace (traces), which may point to a specific log line (logs) with an error.

### Logging architecture
- **Layered Approach:** A robust logging architecture is typically layered: Application $\rightarrow$ Logging Facade $\rightarrow$ Logging Implementation $\rightarrow$ Aggregation.
- **Logging Facade:** Provides an abstraction layer, allowing the application code to be decoupled from the underlying logging framework. This is crucial for libraries, as they shouldn't force a specific logging implementation on the application.
- **Logging Implementation:** The actual logging engine that handles formatting, filtering, and writing logs to various destinations (appenders). Examples include Log4j2 and Logback.
- **Appenders:** These are the output destinations configured in the logging implementation. They can write to the console, files (rolled over daily), sockets, databases, or message queues for aggregation.
- **Asynchronous Logging:** To minimize performance impact on the application, logs are often written asynchronously. A common pattern uses a disruptor-based queue where the application thread hands off the log event to a background writer thread, allowing the app to continue processing immediately.

### Metrics collection
- **Purpose:** Metrics provide a quantitative, real-time pulse of a system. They are essential for understanding trends, setting up alerts, and capacity planning.
- **Types of Metrics:**
    - **Counters:** A cumulative value that only increases (or is reset to zero on restart). Used for things like total requests served.
    - **Gauges:** A value that can go up and down at a specific point in time. Used for current memory usage, thread pool size, queue depth.
    - **Histograms / Summaries:** Sample observations and provide statistical information like counts, sums, and quantiles (e.g., p99 latency). They are used for request durations and response sizes.
- **Collection Methods:**
    - **Pull-based:** A metrics collector (like Prometheus) periodically scrapes metrics from a known endpoint exposed by the application. This is the most common model.
    - **Push-based:** The application pushes metrics to a central aggregator (like Graphite or StatsD). This is useful for short-lived jobs.
- **Instrumentation:** Metrics can be collected manually in the code (e.g., increment a counter in a controller) or automatically via a monitoring agent that intercepts method calls.
- **Dimensionality:** Modern metrics systems support dimensions (labels/tags), allowing you to slice and dice data (e.g., `http_requests_total{method="GET", endpoint="/api/users", status="200"}`).

### Distributed tracing
- **Purpose:** Distributed tracing follows a single request as it travels through multiple microservices, databases, and message queues. It is essential for understanding performance bottlenecks and failure points in a distributed architecture.
- **Core Concepts:**
    - **Trace:** The complete picture of a request's journey. Represented as a tree of spans.
    - **Span:** A named, timed operation representing a unit of work (e.g., an HTTP request, a database call). A span contains a span ID, a trace ID, and a parent span ID.
    - **Context Propagation:** The mechanism for passing trace and span IDs across service boundaries, typically via HTTP headers (e.g., `traceparent`) or message metadata.
- **Instrumentation:** This can be done manually in the code, but more commonly, it's achieved through auto-instrumentation agents (like OpenTelemetry) that intercept library calls (e.g., Apache HttpClient, JDBC driver) to create spans automatically.
- **Sampling:** Tracing every request in a high-volume system would generate massive amounts of data. Sampling strategies (head-based, tail-based) are used to collect a representative subset of traces (e.g., trace 1 in 1000 requests, or trace all requests that resulted in an error).
- **Visualization:** Traces are visualized in tools like Jaeger or Zipkin, often as a Gantt-chart-like waterfall diagram, showing the timeline of each span and the parent-child relationships.

### Structured logging
- **Definition:** Structured logging is the practice of logging messages in a consistent, predefined format that can be easily parsed by machines, rather than as free-form text. The most common format is JSON.
- **Why it Matters:**
    - **Machine-Readability:** Tools like Logstash, Fluentd, and Splunk can automatically parse structured logs, making them searchable and indexable by fields.
    - **Querying:** It enables powerful queries like `find all logs where user_id = 123 and log_level = ERROR`.
    - **Context:** It forces the inclusion of key-value pairs, ensuring that critical context (e.g., `orderId`, `userId`, `requestId`) is always present and not just buried in a string that's hard to parse.
- **Implementation:** In code, instead of doing `log.info("User " + user + " placed order " + order)`, you do `log.info("Order placed", kv("user", user), kv("order", order))`. The logging framework then outputs a structured event.
- **Benefits for Automation:** Structured logs are essential for automated alerting and anomaly detection. You can set up an alert to trigger when the `errorRate` field in a log stream exceeds a threshold.
- **Correlation with Traces:** A key practice is to include the `traceId` and `spanId` in structured logs. This creates a powerful link between the log events and the distributed trace, allowing you to jump from a slow trace to the specific log lines that were emitted during that trace.

### Log aggregation
- **Problem:** In a distributed system with dozens or hundreds of microservices running on many servers, logs are scattered everywhere. It's impossible to `grep` individual servers to debug a cross-service issue.
- **Solution:** Log aggregation is the process of centralizing logs from all sources (servers, containers, applications) into a single, searchable platform.
- **Common Architecture (ELK/EFK Stack):**
    - **Log Shippers (e.g., Filebeat, Fluentd):** Lightweight agents installed on each node that read log files and forward them.
    - **Central Processor/Aggregator (e.g., Logstash):** Receives logs, parses them (e.g., from plain text to structured JSON), filters, and enriches them.
    - **Storage & Search Engine (e.g., Elasticsearch):** A scalable, distributed search and analytics engine that indexes the logs.
    - **Visualization (e.g., Kibana):** A web UI for searching, analyzing, and visualizing the log data.
- **Cloud-Native Alternatives:** Cloud providers offer managed log aggregation services, such as AWS CloudWatch Logs, Azure Monitor Logs, and Google Cloud Logging.
- **Benefits:** Centralized logs enable full-text search across all services, creation of dashboards, real-time monitoring, and historical analysis for post-incident review.

### SLF4J ecosystem
- **Role:** The Simple Logging Facade for Java (SLF4J) serves as an abstraction or facade for various logging frameworks (e.g., `java.util.logging`, Logback, Log4j). It allows the end-user to plug in the desired logging framework at deployment time.
- **Why Use It:** By coding against the SLF4J API, your library or application is decoupled from any specific logging implementation. This prevents the "classpath hell" of multiple logging frameworks conflicting.
- **Parameterized Logging:** SLF4J introduced a significant performance improvement over older logging methods with parameterized messages: `logger.debug("User {} placed order {}", user, order);`. This avoids constructing the log message string if the log level is disabled.
- **Bridging:** SLF4J provides bridges (e.g., `jcl-over-slf4j.jar`) to redirect calls from other logging APIs (like Apache Commons Logging) to SLF4J, ensuring all logging goes through a single, unified channel.
- **Bindings:** At runtime, you must include an SLF4J binding JAR (e.g., `slf4j-log4j12.jar`, `logback-classic.jar`) which links the SLF4J API to the concrete logging framework. Logback is the native and most performant implementation for SLF4J.

### Log4j / Logback
- **Logback:** Intended as a successor to the popular log4j 1.x. It is the native implementation for SLF4J, meaning it implements the SLF4J API directly without a binding layer. It is known for its speed, small footprint, and extensive documentation.
- **Log4j2:** The successor to log4j 1.x, developed under the Apache Logging Services project. It is also extremely high-performance, offering asynchronous loggers based on the LMAX Disruptor library, which can outperform Logback in high-concurrency scenarios.
- **Core Components (both):**
    - **Loggers:** Named entities that your application uses to log messages. They are hierarchical (e.g., `com.example` is parent of `com.example.service`).
    - **Appenders:** Output destinations (Console, File, Socket, Kafka, etc.).
    - **Layouts:** Control the format of log messages (e.g., `PatternLayout`, `JSONLayout`).
- **Configuration:** Both frameworks are heavily configurable, usually via XML, JSON, or programmatic configuration. You can set different log levels for different loggers, and even reconfigure at runtime without restarting the application.
- **Advanced Features:** Both support advanced features like asynchronous appenders, filters (to accept/deny log events based on various criteria), and rolling file policies (based on time, size, etc.).

### Micrometer metrics
- **Purpose:** Micrometer provides a simple facade over the instrumentation clients for the most popular monitoring systems, similar to what SLF4J does for logging. It allows you to instrument your code with vendor-neutral metrics.
- **Key Concepts:**
    - **Meter:** The interface for collecting a set of measurements. Examples: `Counter`, `Timer`, `Gauge`, `DistributionSummary`.
    - **MeterRegistry:** The central component for creating and managing meters. You interact with a `MeterRegistry` to register and update your metrics.
- **Supported Monitoring Systems:** It has built-in support for many back-ends, including Prometheus, Datadog, Graphite, InfluxDB, New Relic, and JMX. You just need to add the appropriate registry dependency at runtime.
- **Dimensional Metrics:** Micrometer is designed from the ground up to support dimensional (tagged) metrics. Tags (key-value pairs) are a first-class concept, allowing you to add rich context to your metrics (e.g., `Timer.builder("api.requests").tag("endpoint", "/users").register(registry)`).
- **Integration:** It is the default metrics library for Spring Boot Actuator (Spring Boot 2+), making it incredibly easy to get production-quality metrics out of Spring applications with minimal configuration.

### OpenTelemetry integration
- **What is OpenTelemetry?** OpenTelemetry (OTel) is a collection of tools, APIs, and SDKs that aims to standardize the generation, collection, and management of telemetry data (logs, metrics, and traces). It's the second-most active CNCF project after Kubernetes.
- **Unified Standard:** OTel provides a single, vendor-agnostic set of APIs and SDKs. This means you can instrument your code once with OTel and then export the data to any compatible backend (Jaeger, Zipkin, Prometheus, Datadog, etc.).
- **Java Implementation:** The OpenTelemetry Java project includes:
    - **API:** The set of interfaces your application code can use to create spans, metrics, and logs.
    - **SDK:** The implementation of the API, with configurable processors, exporters, and samplers.
    - **Java Agent:** A powerful, auto-instrumentation Java agent that can automatically capture telemetry from many popular libraries and frameworks (Spring, JDBC, gRPC, etc.) with zero code changes.
- **Context Propagation:** A core feature is its robust context propagation mechanism, which can seamlessly propagate trace context across different protocols and formats (W3C TraceContext, B3, etc.).
- **Current State:** OpenTelemetry is rapidly becoming the industry standard for observability. Its metrics API is stable, its tracing is stable and widely adopted, and its logs story is evolving. For new projects, it is the recommended approach to instrumentation.

### JVM metrics
- **Importance:** Monitoring the JVM itself is critical for understanding the health and performance of any Java application. These metrics provide insights into the runtime environment.
- **Key Metric Categories:**
    - **Memory:** Heap usage (committed, used, max) per generation (Eden, Survivor, Old), non-heap memory (Metaspace, CodeCache), and the number of pending finalizations.
    - **Garbage Collection:** GC count and time spent (both young and old collections). These are leading indicators of memory pressure and latency issues.
    - **Threads:** Current number of live threads, daemon threads, peak thread count, and details on thread states (runnable, blocked, waiting). A high number of blocked threads can indicate contention.
    - **Class Loading:** Number of classes currently loaded and total classes loaded/unloaded. A continuously increasing number of loaded classes can sometimes indicate a classloader leak.
    - **JIT Compilation:** Time spent in JIT compilation and the size of the code cache.
- **How to Collect:** These metrics are exposed via the JMX (Java Management Extensions) API. Micrometer can automatically capture many of these via its `JvmGcMetrics`, `JvmMemoryMetrics`, `JvmThreadMetrics`, etc.
- **Correlation:** JVM metrics should be correlated with application-level metrics. For example, a spike in old-gen GC time alongside an increase in request latency is a strong signal to investigate heap usage and potential memory leaks.

### Application health monitoring
- **Purpose:** To provide an at-a-glance view of whether an application instance is alive and well, and able to handle requests. This is crucial for load balancers, orchestrators (like Kubernetes), and operations teams.
- **Liveness vs. Readiness (The Kubernetes Model):**
    - **Liveness Probe:** Determines if the application is running. If it fails, the application might be stuck in a deadlock or an infinite loop, and the orchestrator will restart the container/pod.
    - **Readiness Probe:** Determines if the application is ready to accept traffic. If it fails (e.g., the app is still starting up, the database connection is down), the orchestrator will stop sending it traffic until the probe passes again.
- **Common Health Indicators:** Health checks typically verify connections to critical external dependencies:
    - Database connectivity (e.g., a simple SQL query).
    - Message broker (e.g., Kafka, RabbitMQ) connection.
    - Disk space availability.
    - Connection pool status (are there any connections left?).
- **Implementation:** In Spring Boot, this is elegantly handled by the Actuator module, which provides `/actuator/health` endpoints that aggregate the status of various `HealthIndicator` beans.
- **Custom Health Checks:** For application-specific logic, you can implement custom health checks (e.g., "Is the cache warm enough?" or "Are there enough items in a processing queue?").

### Alerting strategies
- **Purpose:** Alerting is the process of notifying a human (on-call engineer) when a problem is detected or imminent. The goal is to surface actionable information quickly, while avoiding "alert fatigue."
- **Alert on Symptoms, Not Causes:** A golden rule. Alert on user-facing problems (high error rate, high latency) rather than the internal causes (high CPU, a specific exception). A high CPU might be a symptom of a misconfiguration, but if the user isn't affected, it might not warrant a page.
- **Defining Meaningful Thresholds:**
    - **Static Thresholds:** Simple, e.g., `error_rate > 5%`. Good for known limits.
    - **Dynamic/Anomaly Detection:** More advanced, where the system learns a baseline and alerts on deviations. Useful for metrics that have seasonal patterns.
    - **Rate of Change:** Alerting on a sudden spike or drop in a metric, which can be more telling than a static threshold.
- **Multi-condition Alerts:** To reduce noise, you can create alerts that require multiple conditions to be true, e.g., `high latency` AND `high error rate` OR `database connection pool is full`.
- **The Four Golden Signals:** A good starting point for alerting rules, as defined by Google SRE:
    - **Latency:** Time to serve a request.
    - **Traffic:** How much demand is on the system.
    - **Errors:** Rate of failed requests.
    - **Saturation:** How "full" the service is (e.g., CPU, memory, queue depth).

### Production debugging
- **The Challenge:** Debugging in a production environment is fundamentally different from development. You cannot attach an interactive debugger, you cannot easily reproduce the issue, and the stakes are high.
- **Tools of the Trade:**
    - **Logs:** The first line of defense. Good structured logging with correlation IDs is essential to trace a request through the system.
    - **Metrics:** Spot trends and anomalies (e.g., a sudden increase in error rate or latency). They tell you *where* to look.
    - **Traces:** Drill down into a single failing or slow request to see exactly which service and operation caused the problem.
    - **JVM Tools:** For deep JVM introspection without restarting:
        - **jstack:** Get a thread dump to see what all threads are doing, identify deadlocks or threads stuck in a particular state.
        - **jmap:** Get a heap dump to analyze memory usage and find leaks.
        - **jcmd:** A versatile command-line tool for sending diagnostic commands to the JVM.
- **Java Flight Recorder (JFR):** A built-in, low-overhead profiling and event collection framework. It can be turned on in production to continuously record a wealth of information (GC, method profiling, exceptions, etc.) which can then be analyzed with Java Mission Control (JMC) to understand the events leading up to an issue.
- **Dynamic Agents:** Using runtime instrumentation agents to inject logging or diagnostic code into a running application without restarting it (e.g., using Byteman or a custom agent with `retransformClasses`). This is a last resort but can be invaluable for diagnosing tricky issues.

### Thread dumps analysis
- **What is a Thread Dump?** A snapshot of the state of all threads that are part of the JVM process at a particular moment. It shows for each thread its current stack trace (the list of method calls leading to its current point of execution), its name, its priority, and its state (RUNNABLE, BLOCKED, WAITING, TIMED_WAITING).
- **How to Capture:**
    - Using `jstack <pid>`
    - Using `kill -3 <pid>` (on Unix-like systems, triggers a thread dump to stdout)
    - Using `jcmd <pid> Thread.print`
    - From within Java Mission Control.
- **Common Analysis Scenarios:**
    - **Deadlock Detection:** The thread dump will explicitly indicate if a deadlock is detected, showing which threads are holding locks and which are waiting.
    - **Thread Contention:** Many threads in the `BLOCKED` state, waiting on the same monitor, indicate lock contention. The stack trace will show the line of code where the lock is being held.
    - **Stuck Threads:** Threads in `RUNNABLE` state for a long time without progress. The stack trace reveals what the thread is doing (e.g., complex calculation, network I/O that is not timing out properly).
    - **Analyzing CPU Spikes:** Taking multiple thread dumps over a period can show which method calls are consistently present in `RUNNABLE` threads, pointing to the code responsible for high CPU usage.
- **Tools:** Manual analysis is tedious. Tools like fastThread, Samurai, or even online analyzers can parse thread dumps and highlight problems like deadlocks, contention, and thread leaks.

### Heap dumps analysis
- **What is a Heap Dump?** A binary snapshot of all live objects in the JVM heap at a specific moment. It contains information about the objects, their class types, their fields and values, and most importantly, the references (GC roots) that keep them alive.
- **How to Capture:**
    - Using `jmap -dump:live,format=b,file=heap.hprof <pid>`
    - Using `jcmd <pid> GC.heap_dump filename=heap.hprof`
    - Automatically on `OutOfMemoryError` using `-XX:+HeapDumpOnOutOfMemoryError` and `-XX:HeapDumpPath=...`
- **Primary Tool: Eclipse Memory Analyzer (MAT):** The industry-standard tool for heap dump analysis. It's excellent at finding memory leaks.
- **Common Analysis with MAT:**
    - **Leak Suspects Report:** MAT's automatic report is a great starting point. It identifies objects that are likely responsible for a memory leak.
    - **Dominator Tree:** Shows which objects are retaining the largest amount of memory. This helps pinpoint the biggest memory consumers.
    - **Paths to GC Roots:** For a suspected object, you can trace all reference chains back to GC roots. This shows *why* the object is still alive when it shouldn't be, which is the key to finding a leak.
    - **Histogram:** Shows a list of classes with their instance count and total retained heap size. Sorting by retained size can quickly reveal classes that are using too much memory.
- **Offline and Online Analysis:** While MAT is a desktop tool, some commercial APM tools offer online, continuous heap analysis capabilities.

### JFR (Java Flight Recorder)
- **What it is:** Java Flight Recorder (JFR) is a low-overhead, built-in event-based profiling and diagnostics framework in the HotSpot JVM. It's designed to be always on in production.
- **Event-Driven:** JFR collects data from a vast number of predefined events (and you can create custom events). These events cover everything from GC pauses, JIT compilation, method sampling, thread sleeps, file I/O, socket I/O, and exception throws.
- **Low Overhead:** The key feature of JFR is its extremely low overhead (claimed to be less than 1%). This is achieved through highly optimized, asynchronous, and lock-free mechanisms, making it safe to run continuously in production.
- **Data Recording:** JFR records data into a ring buffer. You can either dump the current recording to a file on demand (e.g., when an incident occurs) or run a continuous recording that rolls over old data.
- **Analysis with JMC:** Java Mission Control (JMC) is the GUI client for analyzing JFR recordings. It provides a rich set of views (tables, graphs, flame graphs) to drill down into the recorded data, helping you identify performance bottlenecks, GC issues, and other anomalies.
- **Custom Events:** You can programmatically create your own JFR events to track business-level milestones (e.g., "order processed") or application-specific operations, integrating them into the same recording.

### JMC (Mission Control)
- **What it is:** Java Mission Control (JMC) is an advanced set of tools for monitoring, managing, profiling, and diagnosing performance issues with Java applications. It is the primary client for viewing and analyzing JFR recordings.
- **Key Features:**
    - **JFR Browser:** The central view for opening and exploring `.jfr` files. It organizes events into various tabs like "Memory," "Threads," "I/O," "Code," etc.
    - **Automated Analysis:** JMC can run a set of predefined rules on a JFR recording to produce an "Automated Analysis Report." This report highlights potential issues (e.g., "GC Pauses > 200ms," "High Lock Contention") with explanations and recommendations.
    - **Flame Graph View:** A powerful visualization for CPU sampling data, showing the most frequent code paths and where time is spent.
    - **Thread and Lock Views:** Visualize thread states over time, identify lock contention, and see which threads were holding which locks.
    - **Live Monitoring:** JMC can also connect to a running JVM using JMX to provide live dashboards for memory, threads, and CPU, though the JFR capabilities are its most powerful feature.
- **Maturity:** JMC has been open-sourced and continues to evolve. It is an indispensable tool for any senior Java developer for post-mortem analysis of production incidents and proactive performance tuning.
