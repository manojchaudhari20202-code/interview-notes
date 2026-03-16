
---

## 1. JVM Internals

- What are the components of JVM architecture?
- What is the role of ClassLoader? Explain parent delegation model.
- What are the types of ClassLoaders (Bootstrap, Extension/Platform, Application)?
- How do you create a custom ClassLoader and when would you need one?
- What is the difference between loading, linking, and initializing a class?
- What happens when you run `java MyClass`?
- What are the JVM Runtime Data Areas: Heap, Stack, Method Area, PC Register, Native Method Stack?
- What is the difference between Stack frame and Heap?
- What is Metaspace and how does it differ from PermGen?
- What is JIT (Just-In-Time) compilation? What are C1 and C2 compilers?
- What is interpreted mode vs compiled mode in JVM?
- What is tiered compilation?
- What is escape analysis and how does it enable stack allocation?
- What is method inlining and when does JIT perform it?
- What is deoptimization in JVM?
- What is the difference between JVM, JRE, and JDK?
- What is bytecode and how is it different from machine code?
- What are JVM flags? Give examples of performance-related flags.
- What is `java.lang.instrument` and how does a Java agent work?
- What is GraalVM and what is native image compilation (AOT)?

> **Best Practices:**
> - Always set `-Xms` and `-Xmx` to the same value in production to avoid heap resizing pauses.
> - Use G1GC (default since Java 9) for general workloads; prefer ZGC or Shenandoah for latency-sensitive applications.
> - Never call `System.gc()` explicitly — let the JVM decide.
> - Enable GC logging in production: `-Xlog:gc*:file=gc.log:time,uptime`.
> - Use `-XX:+HeapDumpOnOutOfMemoryError` in production to capture OOM dumps automatically.
> - Keep Metaspace bounded with `-XX:MaxMetaspaceSize` to prevent unbounded native memory growth.
> - Warm up the JVM (JIT) before benchmarking — use JMH, never `System.currentTimeMillis()` loops.
> - Avoid loading classes dynamically in hot paths — ClassLoader operations are expensive.

---

## 2. Data Types & Variables

- What are the 8 primitive types and their sizes?
- What is the default value of each primitive type vs object reference?
- What is autoboxing and unboxing? Where can it cause bugs or performance issues?
- What is the Integer cache? Why does `Integer.valueOf(127) == Integer.valueOf(127)` return true?
- Why does `new Integer(127) == new Integer(127)` return false?
- What is the difference between `==` and `equals()` for primitives vs objects?
- What is widening vs narrowing conversion? Give examples of implicit vs explicit casting.
- What happens when you cast a `double` to `int`?
- What is overflow and underflow in integer arithmetic?
- What is the difference between `float` and `double`? Why should you not use them for money?
- What is `BigDecimal` and when should you use it?
- What is `BigInteger`?
- What is a `char` in Java? How is it stored (UTF-16)?
- Can a `char` hold all Unicode code points? What about surrogate pairs?
- What does `final` mean for a variable? For a reference type vs primitive?
- What is a `static` variable? Where is it stored in memory?
- What is `volatile`? What guarantee does it provide?
- What is `transient`? When is it used?
- What is variable shadowing?
- What is the scope of a local variable vs instance variable vs class variable?
- Is Java pass-by-value or pass-by-reference? Explain with an object example.

> **Best Practices:**
> - Use `int` over `Integer` in local computation to avoid autoboxing overhead.
> - Never use `float` or `double` for monetary values — use `BigDecimal` with explicit scale and rounding mode.
> - Prefer `Long.valueOf()` / `Integer.valueOf()` over `new Long()` / `new Integer()` (deprecated) to leverage the cache.
> - Always use `equals()` to compare object values, never `==` (except for enums and interned strings).
> - Mark constants `static final` and name them in `UPPER_SNAKE_CASE`.
> - Declare variables as `final` wherever they are not reassigned — communicates intent and enables compiler optimizations.
> - Minimize variable scope — declare variables as close to their use as possible.
> - Avoid variable shadowing between instance fields and local variables to prevent subtle bugs.

---

## 3. Operators & Expressions

- What is operator precedence in Java? Give a tricky example.
- What is short-circuit evaluation? How do `&&` and `||` differ from `&` and `|`?
- What is the ternary operator? Can it be nested?
- What are bitwise operators (`&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`)?
- What is the difference between `>>` (signed right shift) and `>>>` (unsigned right shift)?
- How do you check if a number is even using bitwise operator?
- What does the `instanceof` operator do? What is pattern matching `instanceof` (Java 16+)?
- What is the string concatenation operator `+`? How does the compiler optimize it?
- What happens with `+` when mixing String and non-String types?
- What is compound assignment (`+=`, `-=`)? Does it perform implicit casting?
- Can you use `++` and `--` on a `char`?

> **Best Practices:**
> - Always use `&&` and `||` (short-circuit) instead of `&` and `|` for boolean expressions — avoids unnecessary evaluation and NPE.
> - Add parentheses to complex expressions to make precedence explicit rather than relying on memory.
> - Avoid deeply nested ternary operators — extract to a local variable or `if` block for readability.
> - Use `instanceof` pattern matching (Java 16+) instead of casting immediately after an `instanceof` check.

---

## 4. Control Flow

- What is the difference between `while`, `do-while`, and `for` loops?
- What is the enhanced for-each loop? What does it compile to?
- What are labeled `break` and `continue`? Give a use case.
- What is a `switch` statement? What types can be used as switch selector?
- What is fall-through in switch? How do you prevent it?
- What is a switch expression (Java 14+)? How does it differ from a switch statement?
- What is the arrow (`->`) syntax in switch expressions?
- What is `yield` in a switch expression?
- Can `switch` work with `String`? How is it implemented?
- What is the difference between `return`, `break`, `continue`, and `throw` in control flow?

> **Best Practices:**
> - Prefer switch expressions (Java 14+) over switch statements — they are exhaustive by default and avoid fall-through bugs.
> - Use the arrow (`->`) form in switch to eliminate the need for `break`.
> - Avoid labeled `break`/`continue` — refactor to a method call instead for readability.
> - Prefer `for-each` over index-based `for` when you don't need the index.
> - Avoid `do-while` unless the first iteration must always execute — `while` is clearer in most cases.
> - Exit early (guard clauses) rather than deeply nesting `if-else` blocks.

---

## 5. OOP — The Four Pillars

### 5.1 Encapsulation
- What is encapsulation? What problem does it solve?
- How do access modifiers enforce encapsulation: `private`, `default` (package-private), `protected`, `public`?
- What is the difference between `protected` and `default` access across packages?
- What is a JavaBean? What are the conventions for getters and setters?
- Why should fields be `private`? What is information hiding?
- What is the difference between encapsulation and abstraction?
- Can a class access the private members of another instance of the same class?
- What is package-private access? When is it appropriate?
- What is the principle of least privilege in terms of access modifiers?
- How does encapsulation support immutability?

> **Best Practices (Encapsulation):**
> - Make all fields `private` by default; expose only what callers truly need.
> - Return defensive copies of mutable fields (collections, arrays, dates) from getters to prevent external mutation.
> - Prefer immutable objects — no setters, all fields `final`.
> - Keep classes package-private unless they must be public — the smallest possible API surface.
> - Use `Collections.unmodifiableList()` or `List.copyOf()` when returning collections from getters.

### 5.2 Abstraction
- What is abstraction? How is it different from encapsulation?
- What are the two mechanisms for abstraction in Java: abstract classes and interfaces?
- What is an abstract class? What makes it abstract?
- Can an abstract class have a constructor? Why?
- Can an abstract class have concrete (non-abstract) methods?
- Can an abstract class have instance variables?
- Can you instantiate an abstract class directly?
- What happens if a subclass does not implement all abstract methods?
- What is the difference between an abstract class and a concrete class?
- What is the difference between an abstract method and a regular method?
- When should you use an abstract class vs an interface?
- Can an abstract class implement an interface without implementing its methods?
- What is abstraction at the design level (hiding implementation details behind a contract)?

> **Best Practices (Abstraction):**
> - Program to interfaces, not implementations — depend on `List`, not `ArrayList`.
> - Use abstract classes to share state and partial implementation; use interfaces to define contracts.
> - Keep interfaces focused and narrow (Interface Segregation Principle) — many specific interfaces are better than one general one.
> - Name interfaces after what they do/are, not how they are implemented (`Runnable`, `Comparable`, not `AbstractRunnableBase`).

### 5.3 Inheritance
- What is inheritance? What does `extends` give you?
- What is the root of the Java class hierarchy (`java.lang.Object`)?
- What is single inheritance? Why does Java restrict classes to single inheritance?
- What is inherited by a subclass: fields, methods, nested classes — but NOT constructors?
- What is the difference between `is-a` and `has-a` relationships?
- When should you use inheritance vs composition?
- What is the fragile base class problem?
- What is the Liskov Substitution Principle (LSP)? Give an example of violating it.
- What does `super` refer to? How do you call a superclass method or constructor?
- What are the rules for `super()` constructor call (must be first statement)?
- What is constructor chaining? What happens if you don't call `super()` explicitly?
- What is `final` class? Why is `String` final?
- What is `final` method? Can it be overridden?
- Can a subclass access `private` fields of its superclass? How?
- What is the difference between method hiding (static) and method overriding (instance)?
- What is the difference between `extends` for classes and `extends` for interfaces?
- What is multi-level inheritance? What are the risks?
- Can a class inherit from multiple classes? What is the diamond problem?
- How does Java solve the diamond problem using interfaces?

> **Best Practices (Inheritance):**
> - Favor composition over inheritance — only inherit when there is a genuine, stable `is-a` relationship.
> - Design for inheritance or prohibit it (`final` class) — do not leave classes in an ambiguous state.
> - Always call `super()` explicitly in constructors when the superclass has meaningful initialization.
> - Do not call overridable methods from constructors — subclass override runs before the subclass constructor, leaving state uninitialized.
> - Keep inheritance hierarchies shallow (≤ 2–3 levels) to avoid the fragile base class problem.
> - Always annotate overriding methods with `@Override` to catch typos at compile time.
> - Make classes `final` unless you explicitly design them to be subclassed.

### 5.4 Object Relationships — Association, Aggregation & Composition

#### Association
- What is association? What does it mean for two classes to be associated?
- What is the difference between association, aggregation, and composition?
- What is unidirectional vs bidirectional association?
- How is association represented in code (one class holds a reference to another)?
- What is multiplicity in association (one-to-one, one-to-many, many-to-many)?
- What is a dependency (uses-a) vs association (has-a)?

#### Aggregation
- What is aggregation? What is the "whole-part" relationship?
- What is the key characteristic of aggregation: the part can exist independently of the whole?
- Give a real-world example of aggregation (e.g., `Department` has `Employees` — employees exist without the department)?
- How is aggregation represented in Java (field reference, usually passed in via constructor or setter)?
- What is the lifecycle implication of aggregation (destroying the whole does NOT destroy the parts)?
- What is the difference between aggregation and a simple association?
- How is aggregation shown in UML (hollow diamond)?

#### Composition
- What is composition? How is it stronger than aggregation?
- What is the key characteristic of composition: the part cannot exist independently of the whole?
- Give a real-world example of composition (e.g., `House` contains `Rooms` — rooms don't exist without the house)?
- How is composition represented in Java (part object created inside the owner, not shared)?
- What is the lifecycle implication of composition (destroying the whole destroys the parts)?
- What is the difference between composition and aggregation in terms of ownership and lifecycle?
- How is composition shown in UML (filled diamond)?
- How do you enforce composition in code (create the part internally, no external references)?

#### Inheritance vs Composition
- What is the "favor composition over inheritance" principle?
- What are the problems with deep inheritance hierarchies (tight coupling, fragile base class)?
- When is inheritance appropriate (`is-a` is genuinely true and stable)?
- When is composition more appropriate (behavior changes at runtime, avoids coupling)?
- What is the Strategy pattern as an example of composition over inheritance?
- What is the difference between `is-a` (inheritance), `has-a` (composition/aggregation), and `uses-a` (dependency)?
- What is the Open/Closed Principle and how does composition help achieve it?
- What is delegation? How does it relate to composition?
- Can you achieve code reuse through composition? How (delegating method calls to the composed object)?

> **Best Practices (Object Relationships):**
> - Model ownership carefully: use composition when the part's lifecycle is tied to the owner; use aggregation when parts are shared or independently managed.
> - Prefer composition for code reuse — swap behavior at runtime without altering the class hierarchy.
> - Avoid circular associations — they create tight coupling and serialization issues.
> - Use constructor injection to make dependencies explicit and testable.
> - For many-to-many relationships, introduce a dedicated relationship/join class rather than holding two lists.

### 5.5 Polymorphism
- What is polymorphism? What are its two types in Java?
- What is compile-time (static) polymorphism? How is it achieved (method overloading)?
- What is runtime (dynamic) polymorphism? How is it achieved (method overriding + upcasting)?
- What is dynamic dispatch (late binding)?
- What is a virtual method table (vtable)?
- What is static type vs dynamic type of a reference variable?
- What is upcasting? Is it implicit or explicit?
- What is downcasting? When can it fail (`ClassCastException`)?
- What is `instanceof`? Why should you check before downcasting?
- What is pattern matching `instanceof` (Java 16+)? How does it eliminate the cast?
- Can overloading and overriding be called at the same time?
- What is the difference between overloading resolution (compile time) and overriding resolution (runtime)?
- Can a static method be polymorphic? Why not?
- Can constructors be polymorphic?
- What is covariant return type in method overriding?

> **Best Practices (Polymorphism):**
> - Always use the most general type for reference variables (`List<String> list = new ArrayList<>()`) to stay flexible.
> - Check with `instanceof` before downcasting; prefer pattern matching `instanceof` (Java 16+) to eliminate the explicit cast.
> - Avoid type-checking chains (`if instanceof A … else if instanceof B`) — replace with polymorphism (overridden methods).
> - Never rely on field access being polymorphic — always access state through methods.

---

## 6. Classes & Objects (Deep Dive)

### 6.1 Class Structure
- What is the difference between a class and an object?
- What is a class member vs an instance member?
- What are the components of a class: fields, methods, constructors, initializer blocks, nested types?
- What is the difference between instance variables and class (static) variables?
- What is a static method? Can it access instance fields?
- Can a static method be overridden? What happens when you define the same static method in a subclass (hiding)?
- What does `final` mean on a method?
- What does `final` mean on a field? Must it be assigned in the constructor?
- What is a `static final` constant?
- What is the difference between a method and a function in the context of Java OOP?

### 6.2 Constructors
- What is a constructor? How does it differ from a method?
- Can a constructor have a return type?
- What is a default constructor? When does the compiler generate it?
- What is constructor overloading?
- What is constructor chaining with `this()`? What is the rule about its position?
- What is constructor chaining with `super()`? What is the rule about its position?
- Can both `this()` and `super()` appear in the same constructor?
- What happens if no `super()` call is written and the superclass has no no-arg constructor?
- Can a constructor be `private`? What is the use case (Singleton, factory, utility class)?
- Can a constructor be `protected`?
- Can a constructor throw an exception?
- Can a constructor call an overridable method? Why is this dangerous?
- What is a copy constructor? Does Java provide one automatically?

### 6.3 Initializer Blocks & Object Lifecycle
- What is a static initializer block? When does it run?
- What is an instance initializer block? When does it run?
- What is the full order of execution when a subclass object is created?
  - Static initializers of superclass → Static initializers of subclass → Instance initializers + constructors (super first, then sub)
- What is object creation? How many ways can you create an object in Java (new, reflection, clone, deserialization, factory)?
- What is object reachability and when does an object become eligible for GC?
- What is the difference between object creation and object initialization?
- What is a blank final field? When must it be assigned?

### 6.4 this & super
- What does `this` refer to?
- What are the uses of `this`: refer to current instance, call another constructor, pass current object as argument?
- What does `super` refer to?
- What are the uses of `super`: call superclass constructor, call hidden/overridden method, access hidden field?
- Can `this` or `super` be used in a static context?

### 6.5 Nested Classes
- What is a static nested class? What can it access from the enclosing class?
- What is an inner (non-static) class? What hidden reference does it hold?
- Why can an inner class cause a memory leak?
- What is a local class? Where is it defined? What can it capture?
- What is an anonymous class? How does it differ from a lambda?
- What are the restrictions on inner classes (no static members except compile-time constants)?
- When would you use a static nested class vs an inner class?

> **Best Practices (Classes & Objects):**
> - Prefer static nested classes over inner classes — inner classes hold a hidden reference to the outer instance and can cause memory leaks.
> - Keep constructors simple — move complex initialization to factory methods or builder pattern.
> - Never call overridable methods from constructors.
> - Make utility classes `final` with a `private` constructor so they cannot be instantiated or subclassed.
> - Prefer factory methods (`of()`, `from()`, `create()`) over constructors when the creation logic is non-trivial or when return type flexibility is needed.
> - Replace anonymous classes with lambdas wherever the interface is functional.
> - Use `Objects.requireNonNull()` at the top of constructors and methods to validate inputs eagerly.

---

## 7. Interfaces (Deep Dive)

- What is an interface? What is its purpose?
- What can an interface contain: abstract methods, constants (`public static final`), default methods, static methods, private methods, nested types?
- What are the implicit modifiers on interface methods and fields?
- What are default methods (Java 8+)? Why were they introduced (backward compatibility)?
- What are static methods in interfaces (Java 8+)? Can they be inherited?
- What are private methods in interfaces (Java 9+)? What is their purpose?
- What is `@FunctionalInterface`? What is the rule (exactly one abstract method)?
- Can an interface have state (instance fields)? Why not?
- Can an interface extend another interface? Can it extend multiple interfaces?
- Can a class implement multiple interfaces? How are conflicting default methods resolved?
- What is the rule when a class inherits a default method and a method from a superclass with the same signature (class wins)?
- What is the rule when two interfaces provide conflicting default methods (compile error, must override)?
- What is the difference between an abstract class and an interface?
- What is a marker interface? Give examples (`Serializable`, `Cloneable`, `Remote`)?
- What is the difference between a marker interface and an annotation?
- What is `Iterable<T>`? What method must it implement?
- What is `Comparable<T>`? What is the contract of `compareTo()`?
- What is `Comparator<T>`? How does it differ from `Comparable`?
- What is `Cloneable`? Why is it considered a poorly designed interface?
- Can an interface be `final`? Can an interface be instantiated?
- Can an interface have a constructor?
- What is an interface reference? What methods can be called through it?

> **Best Practices (Interfaces):**
> - Program to interfaces — declare variables as `List`, `Map`, `Executor`, not `ArrayList`, `HashMap`, `ThreadPoolExecutor`.
> - Keep interfaces small and focused (ISP) — a `Flyable` and `Swimmable` are better than a single `Animal` interface with both.
> - Use `default` methods only for backward-compatible additions, not as a primary design mechanism.
> - Avoid putting state (mutable fields) in interfaces — use abstract classes when shared state is needed.
> - Prefer `@FunctionalInterface` annotation on single-abstract-method interfaces to prevent accidental addition of methods.
> - Use `Comparator.comparing()` instead of implementing `Comparator` as an anonymous class.

---

## 8. Inheritance — Overriding Rules (Complete)

- What are the five rules for a valid method override?
  1. Same method name
  2. Same parameter list (not overloading)
  3. Return type must be same or covariant (subtype)
  4. Access modifier must be same or more permissive (cannot reduce visibility)
  5. Cannot throw new or broader checked exceptions
- What is covariant return type? Give an example.
- Can an overriding method throw fewer checked exceptions? More?
- Can an overriding method throw unchecked exceptions freely?
- What is the `@Override` annotation? Why should you always use it?
- What happens if `@Override` is used but the method does not actually override anything?
- Can you override a `private` method? (No — it is hidden, not overridden; subclass creates a new method)
- Can you override a `static` method? (No — static methods are hidden, not overridden)
- Can you override a `final` method?
- Can you override a method and make it `final`?
- Can you override a method and make it `abstract`?
- What is the difference between method overriding and method hiding?
- What is the difference between method overriding and method overloading?
- Can you override a method with a synchronized or non-synchronized version?
- What is a bridge method? Why does the compiler generate it for generics + overriding?

> **Best Practices (Overriding):**
> - Always use `@Override` — catches signature mismatches at compile time.
> - Follow Liskov Substitution: an overriding method must honour the contract of the method it replaces.
> - Never weaken preconditions (throw more checked exceptions) or strengthen postconditions in an override.
> - If a class is not designed for subclassing, mark it `final`; document it clearly if it is.
> - Keep overriding method access the same or wider — narrowing access is a compile error.

---

## 9. Polymorphism — Method Resolution (Complete)

- What is the rule for selecting which overloaded method to call at compile time?
- What happens when autoboxing, varargs, or widening is needed for overload resolution?
- What is the order of preference: widening → boxing → varargs?
- What is the rule for selecting which overridden method to call at runtime (dynamic dispatch)?
- Can field access be polymorphic? (No — field access is resolved at compile time based on reference type)
- Can static method calls be polymorphic? (No — resolved at compile time based on reference type)
- What is the output of calling an overridden method from a superclass constructor?
- What is casting and what are its compile-time vs runtime checks?
- What is a `ClassCastException`? Give an example.
- What is `instanceof` pattern matching? `if (obj instanceof String s) { ... }`
- What is the difference between `getClass()` and `instanceof` for type checking?

---

## 10. Enums (Deep Dive)

- What is an `enum`? How does the compiler implement it (extends `java.lang.Enum`)?
- Can an enum extend a class? Why not?
- Can an enum implement an interface?
- What are implicitly provided methods: `values()`, `valueOf(String)`, `name()`, `ordinal()`, `toString()`?
- What is the difference between `name()` and `toString()`?
- Can an enum have instance fields?
- Can an enum have a constructor? What access modifier must it be?
- Can an enum have abstract methods? What must each constant then do?
- Can an enum have concrete methods?
- Can an enum constant have a body (constant-specific class body)?
- Can you compare enum constants with `==`? Why is `==` safe for enums?
- What is `Enum.compareTo()`? What ordering does it use (`ordinal`)?
- How are enums serialized (enum constants use name, not ordinal)?
- What is `EnumSet`? Why is it implemented as a bit vector?
- What is `EnumMap`? Why is it faster than `HashMap` for enum keys?
- How does enum guarantee a singleton per constant?
- What is the preferred Singleton implementation using enum?

> **Best Practices (Enums):**
> - Use enums instead of `int` or `String` constants — type-safe and self-documenting.
> - Use `EnumSet` and `EnumMap` instead of `HashSet`/`HashMap` when keys/elements are enum values — significantly faster.
> - Never rely on `ordinal()` for persistence or logic — ordinals change when constants are reordered. Use a named field.
> - Use enums for Singletons — thread-safe, serialization-safe, concise.
> - Add a `fromString()` or `fromCode()` factory method when enums map to external values (DB codes, API strings).
> - Override `toString()` only when a human-readable representation differs from `name()`.

---

## 11. Sealed Classes & Records (Java 16–17+)

### Sealed Classes
- What is a sealed class? What keyword is used?
- What does `permits` do? What happens if `permits` is omitted (implicitly permits classes in the same file)?
- What are the three modifiers a permitted subclass must have: `final`, `sealed`, or `non-sealed`?
- What is `non-sealed`? Why was it introduced?
- What problem do sealed classes solve (restricted type hierarchy, exhaustive pattern matching)?
- Can a sealed class be abstract?
- Can an interface be sealed?
- What is the relationship between sealed classes and pattern matching for switch?

### Records
- What is a record? What is it designed for (immutable data carriers)?
- What does the compiler auto-generate for a record: canonical constructor, accessors, `equals()`, `hashCode()`, `toString()`?
- Can a record extend a class? (No — implicitly extends `java.lang.Record`)
- Can a record implement an interface?
- Can a record be abstract?
- Can a record be subclassed?
- What is a compact constructor? What is it used for (validation, normalization)?
- Can you add custom constructors to a record?
- Can you add instance methods to a record?
- Can a record have mutable fields? (No — all components are `final`)
- Can a record have static fields and methods?
- What is a local record?
- How are records serialized?

> **Best Practices (Sealed Classes & Records):**
> - Use records for simple, immutable data carriers (DTOs, value objects) — eliminates boilerplate.
> - Use sealed classes to model closed domain hierarchies (e.g., `Result`, `Shape`, `Event`) where all subtypes are known.
> - Combine sealed classes with `switch` pattern matching for exhaustive, compiler-verified dispatch.
> - Use compact constructors in records to validate and normalize input, not to add optional behavior.
> - Prefer records over Lombok `@Value` for pure data carriers in modern Java (16+).

---

## 11. Generics

- What is the purpose of generics?
- What is type erasure? What information is erased at runtime?
- What is a generic class? A generic method?
- What is an upper bounded wildcard `<? extends T>`?
- What is a lower bounded wildcard `<? super T>`?
- What is an unbounded wildcard `<?>`?
- What is the PECS principle (Producer Extends, Consumer Super)?
- Why can't you create a generic array (`new T[]`)?
- What is heap pollution?
- What is a raw type? Why is it dangerous?
- What are reifiable vs non-reifiable types?
- What is recursive type bound? Example: `<T extends Comparable<T>>`
- What are bridge methods? Why does the compiler generate them?
- Can you use `instanceof` with a generic type?
- What is type inference? What is the diamond operator `<>`?
- What are generic type bounds at method level vs class level?

> **Best Practices (Generics):**
> - Never use raw types — always parameterize generic types (`List<String>`, not `List`).
> - Use `<? extends T>` when you only read from a structure; use `<? super T>` when you only write to it (PECS).
> - Use the diamond operator `<>` to avoid redundant type declarations.
> - Prefer generic methods over casting to `Object` when writing reusable utility methods.
> - Suppress `@SuppressWarnings("unchecked")` only at the narrowest possible scope and always add a comment explaining why it is safe.
> - Avoid overloading methods that differ only in generic type — type erasure makes them identical at runtime.

---

## 12. Strings

- Why is `String` immutable in Java?
- What is the String pool (String intern pool)?
- Where is the String pool stored (Heap since Java 7)?
- What does `intern()` do?
- What is the difference between `new String("abc")` and `"abc"`?
- What is `StringBuilder` and how does it differ from `String`?
- What is `StringBuffer`? How does it differ from `StringBuilder`?
- How does string concatenation with `+` work internally? What does the compiler do?
- What are text blocks (Java 15+)? What is incidental vs essential whitespace?
- What is `String.formatted()` (Java 15+)?
- What are common String methods: `length()`, `charAt()`, `substring()`, `indexOf()`, `contains()`, `startsWith()`, `endsWith()`, `replace()`, `replaceAll()`, `split()`, `trim()`, `strip()`, `isBlank()`, `lines()`, `repeat()`?
- What is the difference between `trim()` and `strip()`?
- What is the difference between `equals()` and `equalsIgnoreCase()`?
- What is `String.valueOf()` vs `toString()`?
- How is `String` stored internally (compact strings Java 9+: `byte[]` with encoding flag)?
- What is `CharSequence`?

> **Best Practices (Strings):**
> - Use `StringBuilder` for string construction in loops — `String +` in a loop creates O(n²) intermediate objects.
> - Use text blocks (Java 15+) for multiline strings (JSON, SQL, HTML) instead of `\n` concatenation.
> - Use `String.formatted()` or `String.format()` for parameterized messages, not `+` concatenation.
> - Use `strip()` instead of `trim()` — `strip()` is Unicode-aware.
> - Never call `intern()` manually unless profiling shows measurable String pool benefit.
> - Use `isEmpty()` or `isBlank()` instead of `.length() == 0` or `.trim().isEmpty()`.
> - Prefer `contains()`, `startsWith()`, `endsWith()` over `indexOf()` checks for readability.
> - Use `equalsIgnoreCase()` or normalize case before comparing user input.

---

## 13. Object Class Methods

- What methods does `java.lang.Object` provide?
- What is the `equals()` contract (reflexive, symmetric, transitive, consistent, null)?
- What is the `hashCode()` contract? Why must `equals()` and `hashCode()` be consistent?
- What happens if you override `equals()` but not `hashCode()`?
- What is `toString()`? What is the default output?
- What is `clone()`? What is shallow clone vs deep clone?
- What is the `Cloneable` marker interface?
- What is `finalize()`? Why is it deprecated?
- What are `wait()`, `notify()`, and `notifyAll()`? What lock is required to call them?
- What is `getClass()`?

> **Best Practices (Object Class Methods):**
> - Always override both `equals()` and `hashCode()` together — never one without the other.
> - Use `Objects.equals(a, b)` and `Objects.hash(f1, f2, ...)` to handle nulls safely in `equals`/`hashCode`.
> - Override `toString()` to include all meaningful fields — invaluable for debugging and logging.
> - Avoid `clone()` — it is broken by design (shallow, relies on marker interface). Use a copy constructor or factory method instead.
> - Never use `finalize()` — use `try-with-resources` and `Cleaner` for resource cleanup.
> - Use `notifyAll()` instead of `notify()` unless you have a specific reason to wake exactly one thread.

---

## 14. Exception Handling

- What is the exception hierarchy: `Throwable` → `Error` / `Exception` → `RuntimeException`?
- What is a checked exception? An unchecked exception?
- What is the difference between `Error` and `Exception`?
- What is `finally`? Is it always executed?
- Can `finally` override a `return` value?
- What is try-with-resources? What interface must a resource implement?
- What is a suppressed exception in try-with-resources?
- What is multi-catch (`catch (A | B e)`)?
- What is exception chaining (`initCause()`, constructor with `Throwable`)?
- How do you create a custom exception?
- When should you use checked vs unchecked exceptions?
- What is rethrowing an exception?
- Can you throw inside a `finally` block?
- What is `StackOverflowError`? What causes it?
- What is `OutOfMemoryError`? What are common causes?
- What is `NullPointerException`? What helpful NPE messages were added in Java 14?
- What is `ClassCastException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException`, `IllegalStateException`?
- What does `throws` in a method signature mean?

> **Best Practices (Exception Handling):**
> - Always use try-with-resources for any `AutoCloseable` — streams, connections, readers, writers.
> - Never swallow exceptions with an empty `catch` block — at minimum log them.
> - Catch the most specific exception type possible, not `Exception` or `Throwable`.
> - Throw `IllegalArgumentException` for bad input, `IllegalStateException` for invalid state, `NullPointerException` via `Objects.requireNonNull()` for null guards.
> - Prefer unchecked exceptions for programming errors; use checked exceptions only for recoverable conditions callers must handle.
> - Never throw exceptions in `finally` blocks — they mask the original exception.
> - Always include a meaningful message in exceptions — include the offending value.
> - Use exception chaining (`new RuntimeException("context", cause)`) to preserve the root cause.
> - Do not catch `Error` or `Throwable` unless you are writing a framework boundary that must log-and-rethrow.

---

## 15. Collections Framework — Core Interfaces

- What is the Collections Framework hierarchy?
- What is the difference between `Collection` and `Collections`?
- What is `Iterable` → `Collection` → `List`/`Set`/`Queue`?
- What is `Map`? Why doesn't `Map` extend `Collection`?
- What is the difference between `List`, `Set`, and `Queue`?
- What is `SortedSet` and `NavigableSet`?
- What is `SortedMap` and `NavigableMap`?
- What is `Deque`? How does it extend `Queue`?

---

## 16. Collections Framework — List

- What is `ArrayList`? How does it grow dynamically?
- What is the initial capacity and growth factor of `ArrayList`?
- What is the time complexity of `get()`, `add()`, `add(index)`, `remove()` in `ArrayList`?
- What is `LinkedList`? How is it implemented (doubly linked)?
- What is the time complexity of `get()`, `add()`, `remove()` in `LinkedList`?
- When would you prefer `LinkedList` over `ArrayList`?
- What is `CopyOnWriteArrayList`? When is it used? What is its cost?
- What is `Collections.unmodifiableList()` vs `List.of()` (Java 9)?
- What does `subList()` return? What is structural modification in context of subList?
- What is `Arrays.asList()`? Can you add to it?
- What is `RandomAccess` marker interface?
- How does `ListIterator` differ from `Iterator`?

> **Best Practices (List):**
> - Pre-size `ArrayList` with an expected capacity when known: `new ArrayList<>(1000)` avoids repeated resizing.
> - Prefer `ArrayList` over `LinkedList` for nearly all use cases — better cache locality and lower memory overhead.
> - Use `List.of()` (Java 9+) for small, fixed, immutable lists — faster and memory-efficient.
> - Never add or remove from a list while iterating with a for-each — use `Iterator.remove()` or `removeIf()`.
> - Use `Collections.unmodifiableList()` to expose internal lists defensively; prefer `List.copyOf()` for snapshots.
> - Be careful with `subList()` — it is a view; structural changes to the original list invalidate it.

---

## 17. Collections Framework — Set

- What is `HashSet`? How is it implemented internally (backed by `HashMap`)?
- How does `HashSet` determine uniqueness?
- What is `LinkedHashSet`? What ordering does it maintain?
- What is `TreeSet`? What ordering does it use? What interface must elements implement?
- What is the time complexity of `add()`, `contains()`, `remove()` in `HashSet` vs `TreeSet`?
- What is `EnumSet`? Why is it more efficient?

> **Best Practices (Set):**
> - Use `HashSet` for O(1) uniqueness checks; use `TreeSet` only when sorted order is actually required.
> - Use `LinkedHashSet` when insertion-order iteration matters (e.g., deduplication preserving order).
> - Use `EnumSet` whenever the element type is an enum — it is backed by a bit vector and orders of magnitude faster.
> - Always ensure `equals()` and `hashCode()` are correctly overridden for elements stored in a `HashSet`.

---

## 18. Collections Framework — Map

- What is `HashMap`? How does it work internally?
- What is hashing? What is the role of `hashCode()` and `equals()` in `HashMap`?
- What is the load factor and initial capacity of `HashMap`?
- What is rehashing? When does it occur?
- What happens when two keys have the same hash code (collision)?
- What changed in `HashMap` in Java 8 (treeification of bins at threshold 8)?
- What is `LinkedHashMap`? What ordering modes does it support?
- What is `TreeMap`? What ordering does it maintain?
- What is `WeakHashMap`? When would you use it?
- What is `IdentityHashMap`? When would you use it?
- What is `EnumMap`?
- What is the difference between `HashMap` and `Hashtable`?
- What is `ConcurrentHashMap`? How does it achieve thread safety?
- What is `Map.Entry`?
- What are `Map` methods: `put()`, `get()`, `remove()`, `containsKey()`, `containsValue()`, `putIfAbsent()`, `computeIfAbsent()`, `computeIfPresent()`, `compute()`, `merge()`, `getOrDefault()`, `forEach()`, `replaceAll()`?
- How do you iterate over a `Map`?

> **Best Practices (Map):**
> - Always use immutable, value-based objects as `HashMap` keys — `String`, `Integer`, `UUID`, records.
> - Pre-size `HashMap` when the number of entries is known: `new HashMap<>(expectedSize / 0.75 + 1)`.
> - Use `computeIfAbsent()` for lazy initialization in maps (e.g., `map.computeIfAbsent(k, k -> new ArrayList<>()).add(v)`).
> - Use `getOrDefault()` to avoid null checks on missing keys.
> - Use `Map.of()` / `Map.copyOf()` for small, fixed, immutable maps (Java 9+).
> - Prefer `ConcurrentHashMap` over `Hashtable` or synchronized `HashMap` for concurrent access.
> - Never use `containsKey()` followed by `get()` — use `getOrDefault()` or `computeIfAbsent()` instead.
> - Iterate `entrySet()` when you need both key and value — faster than `keySet()` + `get()`.

### 18.1 Immutable Objects as HashMap Keys

- Why must a good `HashMap` key be immutable?
- What happens when you mutate an object that is already used as a `HashMap` key?
  - Its `hashCode()` changes → it lands in a different bucket → `get()` can no longer find it → effectively lost entry (memory leak)
- What is the contract between `hashCode()` and `equals()` that immutability protects?
- Why is `String` the most commonly used `HashMap` key (immutable, `hashCode` cached internally)?
- What is `String`'s `hashCode` caching (`private int hash`)? How does it avoid recomputation?
- How do you make a custom class a safe `HashMap` key?
  1. Make the class `final` (or effectively immutable)
  2. Make all fields `private final`
  3. Do not provide setters
  4. Override `hashCode()` based only on immutable fields
  5. Override `equals()` consistently with `hashCode()`
  6. If fields are mutable objects themselves, use defensive copies
- What is the consequence of using a mutable key in `HashMap` in a concurrent environment?
- What is the difference between a mutable key causing a logical bug vs a structural bug in `HashMap`?
- Why do wrapper types (`Integer`, `Long`, `Double`) make safe keys?
- Why do `enum` constants make safe keys (and `EnumMap` is preferred when keys are enums)?
- What makes `LocalDate`, `LocalDateTime` safe as keys?
- What is a degenerate `hashCode()` (e.g., always returning a constant)? What is its impact on `HashMap` performance (O(1) degrades to O(n))?
- What is the relationship between a good hash function and uniform bucket distribution?
- What is `Objects.hash(field1, field2, ...)` for implementing `hashCode()`?
- What is the 31-multiplier pattern used in `String.hashCode()`? Why 31 (prime, JIT-friendly)?
- What is the `hashCode` / `equals` symmetry rule: if `a.equals(b)` then `a.hashCode() == b.hashCode()` (but not vice versa)?
- Can two unequal objects have the same `hashCode`? (Yes — hash collision is allowed)
- Can two equal objects have different `hashCode`? (No — this breaks the contract)
- What is the impact of a poor `hashCode()` on `HashMap`, `HashSet`, `Hashtable`, `ConcurrentHashMap`?
- What is a record as a `HashMap` key? (Records auto-generate `equals` and `hashCode` based on all components — safe if components are immutable)
- What is an immutable key wrapper pattern (wrapping a mutable object in an immutable snapshot for use as a key)?
- What is defensive copying for keys before inserting into a map?
- What is the risk of using arrays as `HashMap` keys? (arrays use identity `hashCode` and `equals`, not content)
- How do you use an array as a logical key safely? (wrap in a `List` or use `Arrays.asList()`)

---

## 19. Collections Framework — Queue & Deque

- What is `Queue`? What are its methods (`offer()`, `poll()`, `peek()` vs `add()`, `remove()`, `element()`)?
- What is `Deque`? What is `ArrayDeque`?
- Why is `ArrayDeque` preferred over `Stack` and `LinkedList`?
- What is `PriorityQueue`? What ordering does it use?
- How do you create a max-heap with `PriorityQueue`?
- What is `BlockingQueue`? Name implementations.
- What is `ArrayBlockingQueue` vs `LinkedBlockingQueue`?
- What is `SynchronousQueue`?
- What is `DelayQueue`?
- What is `PriorityBlockingQueue`?

> **Best Practices (Queue & Deque):**
> - Use `ArrayDeque` as a stack or queue — faster and uses less memory than `LinkedList` and `Stack`.
> - Use `LinkedBlockingQueue` or `ArrayBlockingQueue` for producer-consumer scenarios in concurrent code.
> - Use `PriorityQueue` with an explicit `Comparator` to avoid relying on natural ordering for complex types.
> - Size `ArrayBlockingQueue` carefully — an unbounded queue can cause `OutOfMemoryError` under burst load.
> - Prefer `offer()` / `poll()` over `add()` / `remove()` — the former return `false`/`null` on failure instead of throwing.

---

## 20. Iterators & For-Each

- What is `Iterator`? What methods does it have?
- What is `ListIterator`? What extra capabilities does it have?
- What is `ConcurrentModificationException`? What is fail-fast behavior?
- What is fail-safe iterator? Which collections use it?
- What does the enhanced for-each loop compile to?
- What is `Spliterator`? How does it support parallel streams?
- What are `Spliterator` characteristics (SIZED, SORTED, DISTINCT, etc.)?

> **Best Practices (Iterators):**
> - Use `Iterator.remove()` to safely remove elements during iteration instead of `list.remove()`.
> - Use `Collection.removeIf(predicate)` for bulk removal — cleaner and avoids `ConcurrentModificationException`.
> - Prefer for-each over explicit `Iterator` usage unless you need `remove()` during iteration.
> - Never modify a collection's structure in a for-each loop — add to a separate list and merge after.

---

## 21. Comparable & Comparator

- What is the `Comparable<T>` interface? What method does it define?
- What is the contract of `compareTo()`?
- What is the `Comparator<T>` interface? What method does it define?
- When would you use `Comparable` vs `Comparator`?
- What is `Comparator.comparing()`, `thenComparing()`, `reversed()`?
- What is a natural ordering?
- What is the relationship between `compareTo()` and `equals()`?
- How does `TreeSet`/`TreeMap` use comparators?

> **Best Practices (Comparable & Comparator):**
> - Implement `Comparable` for the natural, primary ordering of a class; use `Comparator` for alternative orderings.
> - Ensure `compareTo()` is consistent with `equals()` — inconsistency causes surprising behavior in sorted collections.
> - Use `Comparator.comparing()` chained with `thenComparing()` instead of writing manual comparison logic.
> - Use `Comparator.naturalOrder()` / `reverseOrder()` instead of writing trivial comparators.
> - Avoid subtraction tricks for integer comparison (`a - b`) — they overflow; use `Integer.compare(a, b)`.

---

## 22. Lambda Expressions & Functional Interfaces

- What is a lambda expression? What problem does it solve?
- What is a functional interface?
- What does `@FunctionalInterface` annotation do?
- What is an effectively final variable? Why must lambdas capture effectively final variables?
- What is the difference between a lambda and an anonymous class?
- What is a method reference? What are the 4 types?
- What is `Function<T,R>`? `BiFunction<T,U,R>`?
- What is `Predicate<T>`? `BiPredicate<T,U>`?
- What is `Consumer<T>`? `BiConsumer<T,U>`?
- What is `Supplier<T>`?
- What is `UnaryOperator<T>`? `BinaryOperator<T>`?
- What are primitive specializations (e.g., `IntFunction`, `ToIntFunction`, `IntConsumer`)?
- What is `Function.compose()` vs `Function.andThen()`?
- What is `Predicate.and()`, `or()`, `negate()`?

> **Best Practices (Lambdas & Functional Interfaces):**
> - Prefer method references over lambdas when the lambda body is just a single method call — more readable.
> - Keep lambda bodies short (one expression) — extract long lambdas to named private methods.
> - Use primitive specializations (`IntFunction`, `ToLongFunction`) to avoid boxing overhead in hot paths.
> - Do not mutate external (captured) state inside lambdas — leads to subtle concurrency bugs.
> - Annotate custom functional interfaces with `@FunctionalInterface` to prevent accidental addition of abstract methods.
> - Compose predicates using `.and()`, `.or()`, `.negate()` rather than nested `if` blocks.

---

## 23. Streams API

- What is a Stream? How does it differ from a Collection?
- What is the difference between intermediate and terminal operations?
- What is lazy evaluation in streams?
- What is a short-circuit operation?
- What are stateless vs stateful intermediate operations?
- What is `filter()`, `map()`, `flatMap()`, `mapToInt()`, `mapToObj()`?
- What is the difference between `map()` and `flatMap()`?
- What is `peek()`? When would you use it?
- What is `distinct()`, `sorted()`, `limit()`, `skip()`?
- What is `forEach()`, `forEachOrdered()`?
- What is `collect()`? What is a `Collector`?
- What is `Collectors.toList()`, `toSet()`, `toMap()`, `toUnmodifiableList()`?
- What is `Collectors.groupingBy()`? `partitioningBy()`? `joining()`? `counting()`? `summarizingInt()`?
- What is `reduce()`? What is `identity`?
- What is `Optional<T>`? How does it avoid NullPointerException?
- What are `Optional` methods: `of()`, `ofNullable()`, `empty()`, `isPresent()`, `isEmpty()`, `get()`, `orElse()`, `orElseGet()`, `orElseThrow()`, `map()`, `flatMap()`, `filter()`, `ifPresent()`, `ifPresentOrElse()`?
- What is `count()`, `min()`, `max()`, `findFirst()`, `findAny()`?
- What is `anyMatch()`, `allMatch()`, `noneMatch()`?
- What are `IntStream`, `LongStream`, `DoubleStream`? What is `range()` vs `rangeClosed()`?
- What is `Stream.of()`, `Stream.iterate()`, `Stream.generate()`, `Stream.concat()`?
- What is a parallel stream? How does it work?
- What are the risks of parallel streams (ordering, shared mutable state, thread safety)?
- When should you NOT use parallel streams?
- What is `toArray()` on a stream?
- Can you reuse a stream?
- What is `stream()` vs `parallelStream()`?

> **Best Practices (Streams):**
> - Prefer streams for transformations and aggregations; use loops for simple iteration with side effects.
> - Always put `filter()` before `map()` to reduce the number of elements processed.
> - Never use `peek()` for business logic — it is for debugging only and may not execute with short-circuit operations.
> - Use `Collectors.toUnmodifiableList()` / `List.copyOf()` when the result must be immutable.
> - Avoid stateful lambdas in streams (shared mutable variables) — they break parallelism and introduce race conditions.
> - Only use parallel streams for CPU-bound work on large datasets; measure before using — overhead often outweighs gains.
> - Use primitive streams (`IntStream`, `LongStream`) for numeric pipelines to avoid boxing.
> - Never `collect()` into a collection just to immediately stream it again — chain operations instead.
> - Use `findFirst()` for ordered streams; `findAny()` for parallel streams where order doesn't matter.

---

## 24. Optional

- What is `Optional`? Why was it introduced?
- What is the difference between `orElse()` and `orElseGet()`?
- When should you NOT use `Optional` (method parameters, fields, collections)?
- What is `Optional.stream()` (Java 9+)?

> **Best Practices (Optional):**
> - Use `Optional` only as a return type to signal that a value may be absent — not as a field or parameter.
> - Prefer `orElseGet(supplier)` over `orElse(value)` when the default value is expensive to create.
> - Use `orElseThrow()` (Java 10+, no argument) for "should always be present" cases — clearer than `get()`.
> - Chain `map()`, `filter()`, `flatMap()` on `Optional` instead of unwrapping with `isPresent()` + `get()`.
> - Never call `Optional.get()` without an `isPresent()` check — use `orElseThrow()` or `ifPresent()` instead.
> - Never wrap `null` with `Optional.of()` — use `Optional.ofNullable()`.

---

## 25. Concurrency — Thread Basics

- What is a thread? What is the difference between a process and a thread?
- How do you create a thread: extending `Thread` vs implementing `Runnable` vs `Callable`?
- What is the thread lifecycle: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED?
- What is `Thread.start()` vs `Thread.run()`?
- What is `Thread.sleep()`? Does it release the lock?
- What is `Thread.join()`?
- What is `Thread.interrupt()`? What is `InterruptedException`?
- What is `Thread.yield()`?
- What is a daemon thread? How is it different from a user thread?
- What is thread priority? Is it guaranteed?
- What is `ThreadLocal<T>`? How does it work?
- What are common `ThreadLocal` memory leak scenarios?
- What is `InheritableThreadLocal`?

> **Best Practices (Threads):**
> - Prefer `Runnable`/`Callable` with an `ExecutorService` over subclassing `Thread` directly.
> - Always name threads (or use a `ThreadFactory`) to make thread dumps readable.
> - Never call `Thread.stop()`, `Thread.suspend()`, `Thread.resume()` — they are deprecated and unsafe.
> - Always handle `InterruptedException` properly — either re-interrupt the thread or propagate it; never swallow it silently.
> - Always call `ThreadLocal.remove()` in a `finally` block when using thread pools — prevents memory leaks.
> - Use daemon threads only for background housekeeping that can be abandoned when the JVM exits.

---

## 26. Concurrency — Synchronization

- What is a race condition?
- What is the `synchronized` keyword?
- What is the difference between synchronized instance method and synchronized static method?
- What is an intrinsic lock (monitor)?
- What is a synchronized block? Why is it preferred over synchronized method?
- What is `volatile`? What does it guarantee (visibility, not atomicity)?
- What is the happens-before relationship?
- What is the Java Memory Model (JMM)?
- What is memory visibility? What is CPU cache flushing?
- What is a deadlock? How do you detect and prevent it?
- What is a livelock?
- What is starvation?
- What is `wait()` and `notify()`? What lock must be held?
- What is the producer-consumer pattern using `wait()`/`notify()`?
- What is double-checked locking? How do you fix it with `volatile`?

> **Best Practices (Synchronization):**
> - Prefer `java.util.concurrent` utilities over raw `synchronized` / `wait()` / `notify()`.
> - Synchronize on a private, final dedicated lock object — never on `this` or a public reference that callers can also lock.
> - Keep synchronized blocks as small as possible — only protect the critical section.
> - Always acquire multiple locks in the same fixed order across all threads to prevent deadlock.
> - Use `volatile` only for single-variable visibility (flags, references); use `AtomicXxx` for compound read-modify-write operations.
> - Use `notifyAll()` over `notify()` unless only one consumer can ever act on the condition.
> - Use `while` (not `if`) loops when checking a condition with `wait()` — guards against spurious wakeups.

---

## 27. Concurrency — java.util.concurrent

- What is `ExecutorService`? What methods does it have?
- What is `ThreadPoolExecutor`? What are its core parameters (corePoolSize, maxPoolSize, keepAlive, queue)?
- What is `Executors.newFixedThreadPool()`, `newCachedThreadPool()`, `newSingleThreadExecutor()`, `newWorkStealingPool()`?
- What is `Future<T>`? What is `get()`, `cancel()`, `isDone()`?
- What is `Callable<T>` vs `Runnable`?
- What is `CompletableFuture`? How does it differ from `Future`?
- What are `CompletableFuture` methods: `supplyAsync()`, `runAsync()`, `thenApply()`, `thenAccept()`, `thenRun()`, `thenCompose()`, `thenCombine()`, `exceptionally()`, `handle()`, `allOf()`, `anyOf()`?
- What is `CountDownLatch`? Use case?
- What is `CyclicBarrier`? How does it differ from `CountDownLatch`?
- What is `Semaphore`?
- What is `Exchanger`?
- What is `Phaser`?
- What is `ReentrantLock`? How does it differ from `synchronized`?
- What is `tryLock()`?
- What is `ReadWriteLock`? What is `ReentrantReadWriteLock`?
- What is `StampedLock`? What is optimistic reading?
- What is `AtomicInteger`, `AtomicLong`, `AtomicReference`, `AtomicBoolean`?
- What is Compare-And-Swap (CAS)?
- What is `LongAdder` vs `AtomicLong`?
- What is `ForkJoinPool`? `RecursiveTask` vs `RecursiveAction`?
- What is work stealing?

> **Best Practices (java.util.concurrent):**
> - Always shut down `ExecutorService` explicitly: `shutdown()` then `awaitTermination()` then `shutdownNow()` if needed.
> - Never use `Executors.newCachedThreadPool()` in production for unbounded request handling — it can spawn unlimited threads.
> - Always handle `CompletableFuture` exceptions with `.exceptionally()` or `.handle()` — unhandled exceptions are silently swallowed.
> - Use `ReentrantLock` with `tryLock(timeout)` to avoid indefinite blocking; always unlock in a `finally` block.
> - Use `LongAdder` instead of `AtomicLong` for high-contention counters (e.g., metrics) — much lower contention.
> - Prefer `ReadWriteLock` over exclusive `synchronized` for data structures read far more often than written.
> - Use `StampedLock` optimistic reads only when you can tolerate occasional retry on conflict.

---

## 28. Virtual Threads (Java 21+)

- What are virtual threads?
- How do virtual threads differ from platform (OS) threads?
- What is a carrier thread?
- What is thread pinning and when does it occur?
- How do you create a virtual thread?
- What is `Thread.ofVirtual()`, `Executors.newVirtualThreadPerTaskExecutor()`?
- What use cases benefit most from virtual threads?
- What is structured concurrency (Java 21+ preview)?

> **Best Practices (Virtual Threads):**
> - Use virtual threads for I/O-bound workloads (HTTP, JDBC, file) — one virtual thread per request scales extremely well.
> - Do not pool virtual threads — create a new one per task; pooling defeats the purpose.
> - Avoid `synchronized` blocks holding I/O in virtual threads — causes thread pinning; use `ReentrantLock` instead.
> - Do not use `ThreadLocal` for large objects in virtual threads at high scale — millions of threads × large ThreadLocal = heap pressure.
> - Use `Executors.newVirtualThreadPerTaskExecutor()` as a drop-in replacement for thread pools in I/O-bound services.

---

## 29. Memory Management & Garbage Collection

- What is the heap structure: Eden, Survivor spaces (S0, S1), Old Gen?
- What is a Minor GC? A Major GC? A Full GC?
- What is the GC root?
- What is mark-and-sweep?
- What is mark-compact?
- What is copying collection?
- What is the Serial GC?
- What is the Parallel GC?
- What is G1GC? What are G1 regions? What is a mixed collection?
- What is ZGC? What makes it low-latency?
- What is Shenandoah GC?
- What is `XX:+UseG1GC`, `+UseZGC`, `-Xmx`, `-Xms`, `-Xmn`?
- What is a Strong reference? Soft reference? Weak reference? Phantom reference?
- When is each type of reference collected?
- What is `ReferenceQueue`?
- What is a memory leak in Java? Give common examples.
- What is Metaspace? What causes `OutOfMemoryError: Metaspace`?
- What is object finalization? Why is it deprecated?
- What is `java.lang.ref.Cleaner`?

> **Best Practices (Memory & GC):**
> - Never rely on `finalize()` or `System.gc()` for deterministic resource release — use try-with-resources.
> - Avoid keeping large object graphs in static collections — the GC root never lets them be collected (classic memory leak).
> - Remove listeners/callbacks when no longer needed — registered observers prevent GC of the subject.
> - Use `WeakReference` for caches and `SoftReference` for memory-sensitive caches; never `WeakHashMap` as a general-purpose cache.
> - Close `ThreadLocal` values in thread pool threads with `remove()` — prevents cross-request data leakage and memory leaks.
> - Monitor GC with `-Xlog:gc*` and use `-XX:+HeapDumpOnOutOfMemoryError` in all environments.
> - For short-lived services (lambdas, containers), prefer ZGC or Shenandoah to minimize GC pauses.

---

## 30. Java I/O (java.io)

- What is the difference between byte streams and character streams?
- What is `InputStream` and `OutputStream`?
- What is `FileInputStream`, `FileOutputStream`?
- What is `BufferedInputStream`, `BufferedOutputStream`? Why are they used?
- What is `Reader` and `Writer`?
- What is `FileReader`, `FileWriter`?
- What is `BufferedReader`, `BufferedWriter`?
- What is `InputStreamReader`? How does it bridge bytes to characters?
- What is `PrintStream` and `PrintWriter`?
- What is `DataInputStream`, `DataOutputStream`?
- What is `ObjectInputStream`, `ObjectOutputStream`?
- What is serialization? What is `Serializable`?
- What is `serialVersionUID`? What happens if it doesn't match?
- What is `transient`? How does it affect serialization?
- What is `Externalizable`? How does it differ from `Serializable`?
- What is `readObject()` and `writeObject()` custom serialization?
- What are the security risks of Java serialization?
- What is `RandomAccessFile`?

> **Best Practices (Java I/O):**
> - Always wrap `FileInputStream`/`FileOutputStream` with `BufferedInputStream`/`BufferedOutputStream` — drastically reduces system calls.
> - Always use try-with-resources for all I/O streams — prevents resource leaks even on exceptions.
> - Always specify charset explicitly (`StandardCharsets.UTF_8`) in `InputStreamReader`/`OutputStreamWriter` — never rely on the platform default.
> - Use `Files.readAllBytes()` / `Files.readString()` for small files; use streaming for large files.
> - Avoid Java serialization for new code — it is insecure and fragile; use JSON, Protocol Buffers, or other formats.
> - Always declare `serialVersionUID` explicitly on `Serializable` classes to control version compatibility.
> - Mark non-persistent fields `transient` to exclude them from serialization.

---

## 31. Java NIO (java.nio)

- What is the difference between java.io and java.nio?
- What is a `Buffer`? What are `position`, `limit`, `capacity`, `mark`?
- What is `flip()`, `clear()`, `compact()`, `rewind()` on a Buffer?
- What is a `Channel`? How is it different from a Stream?
- What is `FileChannel`?
- What is `MappedByteBuffer`? What is memory-mapped I/O?
- What is `SocketChannel`, `ServerSocketChannel`?
- What is a `Selector`? How does non-blocking I/O work?
- What is `SelectionKey`? What are `OP_READ`, `OP_WRITE`, `OP_CONNECT`, `OP_ACCEPT`?
- What is a direct buffer vs heap buffer? When would you use a direct buffer?

> **Best Practices (NIO):**
> - Use direct `ByteBuffer` for large I/O-bound operations (network, file) — avoids copying between JVM and OS.
> - Always call `flip()` after writing to a buffer before reading from it.
> - Use `FileChannel.transferTo()`/`transferFrom()` for large file copies — uses OS zero-copy optimization.
> - Use `MappedByteBuffer` for random-access reads of large files; be aware it holds native memory until unmapped.
> - In non-blocking NIO server code, always cancel `SelectionKey` and close the channel on error.

---

## 32. Java NIO.2 (java.nio.file)

- What is `Path`? How does it differ from `File`?
- What is `Paths.get()` vs `Path.of()`?
- What is `Files` utility class? Name key methods.
- What is `Files.copy()`, `move()`, `delete()`, `readAllLines()`, `readAllBytes()`, `write()`, `walkFileTree()`?
- What is `WatchService`? How do you watch a directory for changes?
- What is `FileVisitor`? What are `visitFile()`, `preVisitDirectory()`, `postVisitDirectory()`, `visitFileFailed()`?
- What is `FileAttribute`, `BasicFileAttributes`, `PosixFileAttributes`?
- What is `StandardOpenOption`?

> **Best Practices (NIO.2):**
> - Prefer `Path`/`Files` API over `java.io.File` for all new code — richer, throws checked exceptions, supports symlinks.
> - Use `Files.walk()` with `try-with-resources` to avoid resource leaks on directory traversal.
> - Always close `WatchService` — it holds a native handle.
> - Use `Files.copy(src, dest, StandardCopyOption.REPLACE_EXISTING, StandardCopyOption.COPY_ATTRIBUTES)` for safe file copies.
> - Use `Files.createTempFile()` for temp files — safer than manual path construction.

---

## 33. Reflection

- What is reflection in Java?
- How do you get a `Class` object (`.class`, `getClass()`, `Class.forName()`)?
- How do you inspect fields, methods, and constructors using reflection?
- What is `getDeclaredFields()` vs `getFields()`?
- What is `setAccessible(true)`? What are its security implications?
- How do you invoke a method dynamically using reflection?
- How do you create an instance using reflection?
- What is a dynamic proxy (`java.lang.reflect.Proxy`)?
- What is `InvocationHandler`?
- What is the performance cost of reflection?

> **Best Practices (Reflection):**
> - Avoid reflection in business logic — it bypasses compile-time safety, breaks refactoring tools, and is slow.
> - Cache `Method`, `Field`, `Constructor` objects instead of looking them up on every call.
> - Use reflection only at framework/library boundaries (DI containers, ORMs, serializers) — not in application code.
> - Prefer interfaces and `ServiceLoader` over reflection-based service discovery where possible.
> - Always handle `IllegalAccessException`, `InvocationTargetException` (unwrap the cause) correctly.
> - In Java 9+ modules, `opens` packages to named modules rather than blanket `--add-opens` flags.

---

## 34. Annotations

- What is an annotation?
- What are built-in annotations: `@Override`, `@Deprecated`, `@SuppressWarnings`, `@SafeVarargs`, `@FunctionalInterface`?
- What are meta-annotations: `@Retention`, `@Target`, `@Inherited`, `@Documented`, `@Repeatable`?
- What are the `RetentionPolicy` values: `SOURCE`, `CLASS`, `RUNTIME`?
- What are `ElementType` targets?
- How do you create a custom annotation?
- How do you read annotations at runtime using reflection?
- What is annotation processing (APT) at compile time?
- What is a repeatable annotation?

> **Best Practices (Annotations):**
> - Use `@Override`, `@SuppressWarnings`, `@Deprecated` (with `since` and `forRemoval`) consistently.
> - Always set `@Retention(RUNTIME)` for annotations you need to read via reflection; use `SOURCE` for annotations consumed only by the compiler or APT.
> - Use narrow `@Target` on custom annotations — restrict to only the element types that make sense.
> - Document the contract of custom annotations clearly — they are part of your API.
> - Prefer compile-time annotation processing over runtime reflection for code generation (better performance and early error detection).

---

## 35. Java Module System (JPMS — Java 9+)

- What problem does the module system solve?
- What is `module-info.java`?
- What is `requires`, `exports`, `opens`, `uses`, `provides`?
- What is the difference between `exports` and `opens`?
- What is a named module vs automatic module vs unnamed module?
- What is the module path vs classpath?
- What is `--add-opens` and `--add-exports` JVM flags?
- What is `ServiceLoader`? How does it relate to `uses` and `provides`?
- What is strong encapsulation in JPMS?

> **Best Practices (JPMS):**
> - `exports` only packages that are part of the public API — keep implementation packages unexported.
> - Use `opens ... to` (qualified opens) instead of unqualified `opens` to limit reflective access to specific modules.
> - Use `ServiceLoader` with `uses`/`provides` for plugin-style extensibility without hard compile-time dependencies.
> - Avoid `--add-opens` and `--add-exports` as permanent command-line flags — they signal a design problem; fix the module boundary instead.
> - Migrate libraries to named modules (`Automatic-Module-Name` in `MANIFEST.MF`) before publishing to avoid unnamed-module hell.

---

## 36. Date and Time API (java.time)

- Why was the new Date/Time API introduced? What was wrong with `java.util.Date` and `Calendar`?
- What is `LocalDate`, `LocalTime`, `LocalDateTime`?
- What is `ZonedDateTime`? What is `ZoneId`?
- What is `Instant`? How does it differ from `LocalDateTime`?
- What is `Duration` vs `Period`?
- What is `DateTimeFormatter`?
- What is `OffsetDateTime`? What is `ZoneOffset`?
- How do you parse and format dates?
- What is `Clock`? How is it useful in testing?
- What is temporal arithmetic: `plus()`, `minus()`, `with()`?
- What is `ChronoUnit`?
- How do you convert between legacy `Date`/`Calendar` and `java.time`?

> **Best Practices (Date/Time API):**
> - Never use `java.util.Date`, `Calendar`, or `SimpleDateFormat` in new code — all are error-prone and not thread-safe.
> - Use `LocalDate` for dates without time, `LocalDateTime` for local date-time, `ZonedDateTime` for timezone-aware timestamps, `Instant` for machine timestamps.
> - Always use `DateTimeFormatter` with an explicit locale and pattern — never rely on system defaults.
> - Use `DateTimeFormatter` as a constant (thread-safe, immutable) — do not create new instances per call.
> - Use `Instant` for storing timestamps in databases and APIs — it is timezone-neutral.
> - Use `ZoneId.of("UTC")` / `ZoneOffset.UTC` explicitly rather than defaulting to system timezone.
> - Use `ChronoUnit.between()` for time difference calculations instead of manual millisecond arithmetic.

---

## 37. Concurrency — Memory Visibility Deep Dive

- What is instruction reordering?
- What is the happens-before guarantee?
- How does `volatile` establish happens-before?
- How does `synchronized` establish happens-before?
- How does `Thread.start()` establish happens-before?
- How does `Thread.join()` establish happens-before?
- What is a `final` field's visibility guarantee?
- What is safe publication of objects?

---

## 38. java.lang Package — Core Classes

- What is `Math` class? Key methods: `abs()`, `ceil()`, `floor()`, `round()`, `pow()`, `sqrt()`, `min()`, `max()`, `random()`?
- What is `System` class? `currentTimeMillis()`, `nanoTime()`, `arraycopy()`, `gc()`, `exit()`, `getenv()`, `getProperty()`?
- What is `Runtime`? `availableProcessors()`, `freeMemory()`, `totalMemory()`, `maxMemory()`?
- What is `ClassLoader` chain?
- What is `ProcessBuilder`?
- What is `Cloneable`?
- What is `Comparable`?
- What is `Number` abstract class?
- What is `Void` class?
- What is `ref` package classes?

---

## 39. Miscellaneous Language Features

- What is a varargs method? How does it work internally (array)?
- What are the rules for varargs overloading?
- What is a static import?
- What is covariant return type?
- What is a bridge method in compiled bytecode?
- What is a strictfp modifier (deprecated Java 17)?
- What is `assert`? How do you enable assertions (`-ea`)?
- What is try-with-multiple-resources?
- What is the difference between `i++` and `++i`?
- What is integer promotion in expressions?
- What is string switch case compilation (uses `hashCode()` + `equals()`)?
- What are the rules for widening, narrowing, and unboxing in method invocation?

---

## 40. Immutability

- What makes a class truly immutable?
- How do you make a class immutable (final class, final fields, no setters, defensive copies)?
- What is a defensive copy? When is it needed?
- What is the difference between shallow immutability and deep immutability?
- Why is immutability valuable for thread safety?
- What are examples of immutable classes in Java: `String`, `Integer`, `LocalDate`?

> **Best Practices (Immutability):**
> - Default to immutable — only make classes mutable when there is a clear performance or API reason.
> - Always make defensive copies of mutable inputs in constructors and defensive copies of mutable outputs in getters.
> - Use `Collections.unmodifiableList()` / `List.copyOf()` to return collection fields safely.
> - Use `record` for immutable data carriers (Java 16+) — the compiler enforces immutability automatically.
> - Declare all fields `final` in an immutable class — the compiler enforces assignment in the constructor.
> - Make the class `final` to prevent subclasses from adding mutable state.

---

## 41. Nested Classes Deep Dive

- What is a static nested class? What can it access?
- What is an inner (non-static nested) class? What is the hidden outer reference?
- What is a local class?
- What is an anonymous class?
- What are the restrictions on each type?
- What is a memory leak caused by inner class holding outer class reference?

> **Best Practices (Nested Classes):**
> - Default to static nested classes — they do not hold a reference to the enclosing instance and cannot cause memory leaks.
> - Use inner classes only when the nested class genuinely needs access to the outer instance's fields.
> - Replace anonymous classes implementing a functional interface with a lambda.
> - Replace local classes with lambdas or method-extracted helpers when they are only used once.

---

## 42. ClassLoader & Class Loading

- What is the ClassLoader hierarchy: Bootstrap → Platform → Application?
- What is the parent delegation model?
- What is `Class.forName()` vs `ClassLoader.loadClass()`?
- What is the difference between class loading and class initialization?
- What triggers class initialization?
- How do you implement a custom ClassLoader?
- What are classloader isolation and classloader leaks?

---

## 43. Concurrency — Advanced Patterns

- What is the producer-consumer pattern?
- What is the reader-writer lock pattern?
- What is the thread pool pattern?
- What is the future/promise pattern?
- What is lock-free programming using CAS?
- What is the actor model? (conceptual)
- What is `ThreadPoolExecutor` rejection policies: `AbortPolicy`, `CallerRunsPolicy`, `DiscardPolicy`, `DiscardOldestPolicy`?
- What is work stealing in `ForkJoinPool`?
- What is the difference between `submit()` and `execute()` on `ExecutorService`?
- What is `invokeAll()` vs `invokeAny()`?

---

## 44. GC Tuning & JVM Monitoring

- How do you print GC logs? (`-Xlog:gc`)
- What is GC pause time and throughput trade-off?
- What JVM tools exist: `jps`, `jstack`, `jmap`, `jstat`, `jcmd`, `jconsole`, `jvisualvm`?
- What is a heap dump? How do you capture one?
- What is a thread dump? How do you read one?
- What is `-XX:+HeapDumpOnOutOfMemoryError`?
- What is the `jstat -gcutil` output?
- How do you tune G1GC pause targets (`-XX:MaxGCPauseMillis`)?
- What are common memory leak patterns and how do you detect them?

---

## 45. Sealed, Records, Pattern Matching Summary (Java 17–21)

- What is pattern matching for `switch` (Java 21)?
- What is a guarded pattern (`case String s when s.length() > 5`)?
- What is a record pattern (`case Point(int x, int y)`)?
- How does sealed classes + pattern matching enable exhaustive switch?
- What is `when` guard in switch patterns?
- What is `null` case in switch (Java 21)?

---

## 46. JDBC — Architecture & Drivers

- What is JDBC? What does it stand for?
- What is the purpose of JDBC (uniform API to interact with any relational database)?
- What are the layers in JDBC architecture: application, JDBC API, JDBC Driver Manager, JDBC Driver, database?
- What is a JDBC driver? What are the four types of JDBC drivers?
  - Type 1: JDBC-ODBC Bridge (deprecated, removed Java 8)
  - Type 2: Native-API driver (partially Java)
  - Type 3: Network Protocol driver (middleware)
  - Type 4: Thin driver / Pure Java driver (most common today)
- What is `java.sql` package vs `javax.sql` package?
- What is `DriverManager`? What does `DriverManager.getConnection()` do?
- What is the JDBC URL format? Give examples for MySQL, PostgreSQL, Oracle.
- How does `DriverManager` find the right driver (service provider mechanism, `Class.forName()` legacy)?
- What is `Class.forName("com.mysql.cj.jdbc.Driver")`? Is it still needed in modern JDBC (Java 6+)?
- What is `Driver` interface? What is `Driver.connect()`?
- What is `DriverManager.registerDriver()`?
- What is `DataSource`? How does it differ from `DriverManager`?
- What are the advantages of `DataSource` over `DriverManager` (connection pooling, JNDI lookup)?
- What is `javax.sql.DataSource`?
- What is `javax.sql.ConnectionPoolDataSource`?
- What is `javax.sql.XADataSource` (distributed transactions)?

---

## 47. JDBC — Connection

- What is a `Connection` object? What does it represent?
- What is the JDBC connection URL? What are common parameters (host, port, dbname, charset, SSL)?
- How do you obtain a connection using `DriverManager.getConnection(url, user, password)`?
- What is connection autocommit? What is the default value (true)?
- What does `setAutoCommit(false)` do?
- What is `Connection.close()`? Why must it always be called?
- What happens if you don't close a connection?
- What is try-with-resources with JDBC connections?
- What is `Connection.isClosed()`? `isValid(timeout)`?
- What is `Connection.setReadOnly(true)`?
- What is `Connection.setCatalog()` and `setSchema()`?
- What is `Connection.setNetworkTimeout()`?
- What connection properties can be passed via `Properties` object?
- What is `Connection.getMetaData()`? What is `DatabaseMetaData`?
- What information does `DatabaseMetaData` provide (database version, supported features, tables, columns)?

---

## 48. JDBC — Statements

### Statement
- What is `Statement`? When should you use it?
- What is `Statement.executeQuery(sql)`? What does it return?
- What is `Statement.executeUpdate(sql)`? What does it return (update count)?
- What is `Statement.execute(sql)`? When is it used (unknown result type)?
- What is `Statement.getResultSet()` and `getUpdateCount()` after `execute()`?
- What is `Statement.getGeneratedKeys()`? How do you retrieve auto-generated keys?
- What is `Statement.RETURN_GENERATED_KEYS`?
- What is statement fetch size (`setFetchSize()`)? How does it affect memory?
- What is `Statement.setMaxRows()`?
- What is `Statement.setQueryTimeout()`?
- What is `Statement.cancel()`?
- What is `Statement.close()`?

### PreparedStatement
- What is `PreparedStatement`? How does it differ from `Statement`?
- What are the advantages of `PreparedStatement` over `Statement`?
  - Precompilation and execution plan caching
  - Prevents SQL injection
  - Better performance for repeated execution
  - Type-safe parameter binding
- How do you create a `PreparedStatement`?
- What are parameter placeholders (`?`) in a `PreparedStatement`?
- What are the setter methods: `setInt()`, `setString()`, `setDouble()`, `setDate()`, `setTimestamp()`, `setNull()`, `setObject()`, `setBlob()`, `setClob()`?
- What is `setNull(index, Types.INTEGER)`?
- What is `clearParameters()`?
- What is `PreparedStatement.executeQuery()` vs `executeUpdate()` vs `execute()`?
- How does `PreparedStatement` prevent SQL injection?
- Can you use `PreparedStatement` for DDL statements?
- What is a parameterized query?

> **Best Practices (Statements):**
> - Always use `PreparedStatement` for any query with parameters — prevents SQL injection and improves performance via plan caching.
> - Never concatenate user input into SQL strings.
> - Set `setQueryTimeout()` on statements that may run long to prevent hung queries.
> - Use `setFetchSize()` on queries returning large result sets to avoid loading all rows into memory at once.
> - Close `Statement` objects explicitly (or via try-with-resources) — open statements consume server-side cursors.
> - Use `getGeneratedKeys()` with `RETURN_GENERATED_KEYS` to retrieve auto-increment primary keys after insert.

### CallableStatement
- What is `CallableStatement`? When is it used?
- How do you call a stored procedure with `CallableStatement`?
- What is `registerOutParameter()`? How do you retrieve OUT parameters?
- What is the difference between IN, OUT, and INOUT parameters in stored procedures?
- What is `CallableStatement.execute()` vs `executeQuery()` vs `executeUpdate()`?

---

## 49. JDBC — ResultSet

- What is `ResultSet`? What does it represent?
- What is the cursor in a `ResultSet`? Where does it start?
- What is `ResultSet.next()`? What does it return?
- What are getter methods: `getInt()`, `getString()`, `getDouble()`, `getDate()`, `getTimestamp()`, `getBoolean()`, `getObject()`, `getBlob()`, `getClob()`?
- What is the difference between accessing columns by index vs by name?
- What is `ResultSet.wasNull()`? When do you need it?
- What are the three `ResultSet` types?
  - `TYPE_FORWARD_ONLY`: cursor moves forward only (default)
  - `TYPE_SCROLL_INSENSITIVE`: scrollable, not sensitive to changes
  - `TYPE_SCROLL_SENSITIVE`: scrollable, sensitive to changes
- What are the `ResultSet` concurrency modes?
  - `CONCUR_READ_ONLY` (default)
  - `CONCUR_UPDATABLE`
- How do you create a scrollable `ResultSet`?
- What are navigation methods on a scrollable ResultSet: `previous()`, `first()`, `last()`, `absolute()`, `relative()`, `beforeFirst()`, `afterLast()`?
- What is an updatable `ResultSet`? What are `updateInt()`, `updateString()`, `updateRow()`, `insertRow()`, `deleteRow()`?
- What is `ResultSet.getMetaData()`? What is `ResultSetMetaData`?
- What information does `ResultSetMetaData` provide (column count, column name, column type, table name)?
- What is `ResultSet.close()`? What happens when the `Statement` is closed?
- What is a `ResultSet` holdability: `HOLD_CURSORS_OVER_COMMIT` vs `CLOSE_CURSORS_AT_COMMIT`?
- What is a streaming `ResultSet` (large result sets with `setFetchSize(Integer.MIN_VALUE)` in MySQL)?

> **Best Practices (ResultSet):**
> - Always close `ResultSet` explicitly in try-with-resources — open cursors consume server resources.
> - Use column names (not indices) in getters for readability and resilience to column reordering.
> - Call `wasNull()` after reading a column that may be `NULL` from a nullable primitive column.
> - Use `setFetchSize()` to stream large results row by row rather than buffering everything in memory.
> - Prefer `TYPE_FORWARD_ONLY` / `CONCUR_READ_ONLY` (defaults) unless scrollability or updatability is genuinely needed — they are faster.
> - Do not keep a `ResultSet` open across transaction boundaries — holdability causes resource leaks.

---

## 50. JDBC — Transactions

- What is a database transaction?
- What are the ACID properties (Atomicity, Consistency, Isolation, Durability)?
- What is `Connection.setAutoCommit(false)` for manual transaction control?
- What is `Connection.commit()`?
- What is `Connection.rollback()`?
- What is `Connection.rollback(Savepoint)`?
- What is a `Savepoint`? How do you create one with `Connection.setSavepoint()`?
- How do you release a savepoint (`Connection.releaseSavepoint()`)?
- What is the correct try-catch-finally pattern for JDBC transactions?
- What are database transaction isolation levels?
  - `TRANSACTION_NONE` — transactions not supported
  - `TRANSACTION_READ_UNCOMMITTED` — dirty reads possible
  - `TRANSACTION_READ_COMMITTED` — no dirty reads; non-repeatable reads possible
  - `TRANSACTION_REPEATABLE_READ` — no dirty/non-repeatable reads; phantom reads possible
  - `TRANSACTION_SERIALIZABLE` — fully isolated
- What is a dirty read?
- What is a non-repeatable read?
- What is a phantom read?
- How do you set isolation level with `Connection.setTransactionIsolation()`?
- What is `Connection.getTransactionIsolation()`?
- What is optimistic locking vs pessimistic locking?
- What is a deadlock in database transactions?

> **Best Practices (JDBC Transactions):**
> - Always call `setAutoCommit(false)` before a multi-step transaction; commit only on full success.
> - Always roll back in a `catch` block and in a `finally` if commit did not complete.
> - Use `READ_COMMITTED` as the default isolation level — it balances correctness and concurrency for most apps.
> - Use `SERIALIZABLE` only when absolute correctness is required and accept the throughput cost.
> - Prefer optimistic locking (version columns) over pessimistic locking for high-concurrency reads.
> - Keep transactions short — do not hold a transaction open while waiting for user input or calling external services.
> - Use savepoints for nested logical units within a transaction that may need partial rollback.

---

## 51. JDBC — Batch Processing

- What is batch processing in JDBC? Why is it used?
- How do you add statements to a batch with `Statement.addBatch(sql)`?
- How do you execute a batch with `Statement.executeBatch()`? What does it return (`int[]`)?
- How do you clear a batch with `Statement.clearBatch()`?
- How do you use `PreparedStatement` for batching?
- What is the performance advantage of batching over individual inserts?
- What is `rewriteBatchedStatements=true` (MySQL JDBC property)?
- What happens if one statement in a batch fails?
- What is `BatchUpdateException`? How do you get per-statement update counts from it?
- What is `Statement.executeLargeBatch()`? What does it return (`long[]`)?
- What is `EXECUTE_FAILED` constant in `Statement`?

> **Best Practices (Batch Processing):**
> - Use `PreparedStatement` batching, not `Statement` batching — avoids re-parsing SQL on each batch item.
> - Batch in chunks (e.g., 500–1000 rows per batch) rather than one massive batch — avoids excessive memory and lock contention.
> - Wrap the batch loop in a transaction for atomicity and speed.
> - Check individual update counts from `BatchUpdateException.getUpdateCounts()` to identify which statements failed.
> - Enable driver-level batch rewriting where available (e.g., `rewriteBatchedStatements=true` for MySQL).

---

## 52. JDBC — Connection Pooling

- What is connection pooling? Why is it necessary?
- What is the cost of creating a new database connection each time?
- What does a connection pool manage (a cache of reusable connections)?
- What happens when you call `connection.close()` with a pooled connection (returns to pool, not actually closed)?
- What are key connection pool configuration parameters?
  - Minimum pool size (min idle connections)
  - Maximum pool size (max total connections)
  - Connection timeout (wait time to get connection from pool)
  - Idle timeout (how long idle connection stays in pool)
  - Max lifetime (maximum age of a connection)
  - Keepalive / validation query
- What is HikariCP? Why is it considered the fastest connection pool?
- What is `HikariConfig` and `HikariDataSource`?
- What is Apache DBCP2?
- What is c3p0?
- What is connection validation (test-on-borrow, test-while-idle)?
- What is a stale connection? How does the pool detect it?
- What is a connection leak? How do you detect one?
- What is `leakDetectionThreshold` in HikariCP?
- What is `DataSource.getConnection()` vs `DriverManager.getConnection()`?

> **Best Practices (Connection Pooling):**
> - Always use a connection pool in production — never `DriverManager.getConnection()` directly.
> - Use HikariCP as the default pool — lowest overhead, best throughput.
> - Set `maximumPoolSize` based on the database's max connections and the number of application instances.
> - Set `connectionTimeout` to a sensible value (e.g., 30s) to fail fast rather than queue indefinitely.
> - Set `maxLifetime` (e.g., 30 min) shorter than the database's `wait_timeout` to avoid stale connections.
> - Enable `leakDetectionThreshold` in development to catch connections not returned to the pool.
> - Always close connections in a `finally` block or use try-with-resources — pooled connections must be returned.

---

## 53. JDBC — Advanced Features

### Large Objects (LOB)
- What is a `Blob` (Binary Large Object)?
- What is a `Clob` (Character Large Object)?
- How do you read a `Blob` from a `ResultSet`?
- How do you write a `Blob` into a `PreparedStatement`?
- What is `NClob` (national character set Clob)?
- What is streaming LOBs vs materializing LOBs?

### Array & Struct
- What is `java.sql.Array`? How do you use it with `PreparedStatement`?
- What is `java.sql.Struct`?
- What is `Connection.createArrayOf()`?

### RowSet
- What is `RowSet`? How does it differ from `ResultSet`?
- What is `JdbcRowSet`? What is its advantage (scrollable, updatable by default)?
- What is `CachedRowSet`? What is its advantage (disconnected, serializable)?
- What is `WebRowSet`?
- What is `FilteredRowSet`?
- What is `JoinRowSet`?
- What is `javax.sql.rowset.RowSetFactory`?

### Stored Procedures & Functions
- How do you call a stored procedure that returns a `ResultSet`?
- How do you call a stored function that returns a value?
- What is the `{call procedure_name(?,?)}` escape syntax?
- What is the `{? = call function_name(?)}` escape syntax?
- What are JDBC escape sequences?

### Metadata
- What is `DatabaseMetaData`? How do you get it?
- What does `DatabaseMetaData.getTables()` return?
- What does `DatabaseMetaData.getColumns()` return?
- What does `DatabaseMetaData.getPrimaryKeys()` return?
- What does `DatabaseMetaData.supportsTransactions()` return?
- What is `ResultSetMetaData.getColumnCount()`, `getColumnName()`, `getColumnType()`, `getColumnClassName()`?

---

## 54. JDBC — Type Mappings

- What is the mapping between Java types and SQL types?
  - `int` / `Integer` → `INTEGER`
  - `long` / `Long` → `BIGINT`
  - `double` / `Double` → `DOUBLE`
  - `float` / `Float` → `FLOAT` / `REAL`
  - `boolean` / `Boolean` → `BIT` / `BOOLEAN`
  - `String` → `VARCHAR`, `CHAR`, `TEXT`
  - `byte[]` → `BINARY`, `VARBINARY`, `BLOB`
  - `java.sql.Date` → `DATE`
  - `java.sql.Time` → `TIME`
  - `java.sql.Timestamp` → `TIMESTAMP`
  - `java.math.BigDecimal` → `NUMERIC`, `DECIMAL`
  - `java.sql.Blob` → `BLOB`
  - `java.sql.Clob` → `CLOB`
- What is `java.sql.Types`? What are the SQL type constants?
- What is the difference between `java.sql.Date`, `java.sql.Time`, `java.sql.Timestamp` and `java.util.Date`?
- How do you map `java.time.LocalDate` and `java.time.LocalDateTime` to JDBC (Java 8+ drivers support directly)?
- What is `setObject(index, value, Types.XXX)`?
- What is `getObject(index, Class<T>)` (JDBC 4.1)?

---

## 55. JDBC — Exception Handling

- What is `SQLException`? What information does it carry (message, SQLState, vendor error code)?
- What is `SQLState`? What are common SQLState codes?
- What is the `SQLException` chain (multiple exceptions linked via `getNextException()`)?
- What are the subclasses of `SQLException`?
  - `SQLTransientException` (retry may succeed)
  - `SQLNonTransientException` (retry won't help)
  - `SQLRecoverableException` (reconnect and retry)
  - `BatchUpdateException`
  - `SQLWarning`
  - `DataTruncation`
- What is `SQLWarning`? How do you retrieve warnings from `Connection`, `Statement`, `ResultSet`?
- What is `DataTruncation`? When is it thrown?
- What is `Connection.getWarnings()` and `clearWarnings()`?
- What is the correct way to close JDBC resources to avoid connection leaks (try-with-resources)?
- What is the proper order to close JDBC resources: `ResultSet` → `Statement` → `Connection`?

---

## 56. JDBC — SQL Injection & Security

- What is SQL injection? Give an example.
- How does `PreparedStatement` prevent SQL injection?
- Why is string concatenation to build SQL queries dangerous?
- What is input validation as a secondary defense?
- What is the principle of least privilege for database users?
- What is `allowMultiQueries=false` (MySQL JDBC property to prevent stacked queries)?
- What are stored procedures as a defense against SQL injection?
- How do you securely pass credentials to JDBC (environment variables, config files, secrets manager)?

---

## 57. JDBC — Best Practices

- Always use `PreparedStatement` instead of `Statement` for parameterized queries.
- Always close `ResultSet`, `Statement`, and `Connection` in a `finally` block or use try-with-resources.
- Use connection pooling in production (never `DriverManager` directly).
- Set `setFetchSize()` for large result sets to avoid loading everything into memory.
- Use batch inserts for bulk data operations.
- Use `setAutoCommit(false)` for multi-statement transactions and always `rollback()` on exception.
- Use `setQueryTimeout()` to prevent hung queries.
- Use `DatabaseMetaData` and `ResultSetMetaData` for dynamic schema handling.
- Avoid `SELECT *` — always name required columns explicitly.
- Log SQL errors with `SQLState` and vendor error code for diagnostics.
- Use `setNull()` explicitly when inserting null values.
- Prefer `java.sql.Timestamp` or `java.time` types over `java.util.Date` for date/time columns.

---

## 58. Java Version History & Key Features per Release

### Java 1.0 – 1.4 (Foundations)
- What was introduced in Java 1.0? (Classes, interfaces, threads, basic collections, AWT)
- What was added in Java 1.1? (Inner classes, JDBC 1.0, RMI, Reflection, JavaBeans)
- What was added in Java 1.2 (Java 2)? (Collections Framework, Swing, JIT compiler, strictfp)
- What was added in Java 1.3? (HotSpot JVM default, JNDI, Java Sound)
- What was added in Java 1.4? (Assertions, NIO, Logging, Regular expressions, Chained exceptions, XML)

### Java 5 (Generics Era — 2004)
- What was the biggest release in early Java? (Java 5 — transformative)
- What were the major features of Java 5?
  - Generics
  - Enhanced for-each loop
  - Autoboxing/unboxing
  - Enums
  - Varargs
  - Static imports
  - Annotations (`@Override`, `@Deprecated`, `@SuppressWarnings`)
  - `java.util.concurrent` (`ExecutorService`, `Future`, `Lock`, `Atomic*`)
  - `StringBuilder` (replacing `StringBuffer`)
  - Formatted output (`printf`, `Formatter`)
- What are type-safe enums? Why were they introduced to replace `int` constants?

### Java 6 (2006)
- Scripting API (`javax.script`), Compiler API (`javax.tools`), JDBC 4.0 (automatic driver loading), JAX-WS 2.0, improvements to collections performance

### Java 7 (2011)
- What were the major features of Java 7 (Project Coin)?
  - Diamond operator `<>`
  - try-with-resources (`AutoCloseable`)
  - Multi-catch (`catch (A | B e)`)
  - `switch` on `String`
  - Binary integer literals (`0b1010`)
  - Underscores in numeric literals (`1_000_000`)
  - NIO.2 (`java.nio.file` — Path, Files, WatchService)
  - `ForkJoinPool`
  - `Objects` utility class
- What is `AutoCloseable`? Why is it important?
- What changed with String pool in Java 7? (Moved from PermGen to Heap)

### Java 8 (2014 — Functional Era)
- What were the major features of Java 8? (Largest single release since Java 5)
  - Lambda expressions
  - Functional interfaces (`@FunctionalInterface`, `java.util.function`)
  - Streams API (`java.util.stream`)
  - `Optional<T>`
  - Default and static methods in interfaces
  - Method references
  - `java.time` package (replacing `Date`/`Calendar`)
  - `CompletableFuture`
  - Nashorn JavaScript engine (deprecated later)
  - Parallel array sorting (`Arrays.parallelSort`)
  - Base64 encoding (`java.util.Base64`)
  - New `Map` methods (`computeIfAbsent`, `merge`, `getOrDefault`, `forEach`, `replaceAll`)
  - `StringJoiner`
  - PermGen removed → replaced by Metaspace
- What is a default method? Why was it revolutionary in Java 8?
- What is the difference between `Stream.of()` and `Arrays.stream()`?

### Java 9 (2017 — Module Era)
- What were the major features of Java 9?
  - Java Platform Module System (JPMS / Project Jigsaw)
  - `module-info.java`
  - JShell (REPL)
  - Private methods in interfaces
  - Immutable collection factories: `List.of()`, `Set.of()`, `Map.of()`
  - `Stream` enhancements: `takeWhile()`, `dropWhile()`, `iterate()` with predicate, `ofNullable()`
  - `Optional` enhancements: `ifPresentOrElse()`, `stream()`, `or()`
  - `CompletableFuture` enhancements: `orTimeout()`, `completeOnTimeout()`, `failedFuture()`
  - `HttpClient` (incubator)
  - Process API improvements (`ProcessHandle`)
  - Stack-Walking API (`StackWalker`)
  - Compact Strings (internal — `byte[]` instead of `char[]`)
  - G1GC becomes default GC
- What is JShell? When would you use it?

### Java 10 (2018)
- What were the major features of Java 10?
  - Local variable type inference: `var`
  - `List.copyOf()`, `Set.copyOf()`, `Map.copyOf()`
  - `Collectors.toUnmodifiableList/Set/Map()`
  - Application class-data sharing (AppCDS)
  - Thread-local handshakes (JVM internals)
- What are the restrictions on `var`? (local variables only, not fields, parameters, return types)
- What is the difference between `var` and dynamic typing?

### Java 11 (2018 — LTS)
- What were the major features of Java 11?
  - `HttpClient` (standard — `java.net.http`)
  - String methods: `strip()`, `stripLeading()`, `stripTrailing()`, `isBlank()`, `lines()`, `repeat()`
  - `Files.readString()`, `Files.writeString()`
  - `var` in lambda parameters
  - `Predicate.not()` static method
  - Nest-based access control
  - Local-Variable Syntax for Lambda Parameters
  - Oracle JDK → OpenJDK feature parity
  - Removal of Java EE and CORBA modules
- What is `Predicate.not(String::isBlank)`? How does it differ from `.negate()`?

### Java 12–13 (2019)
- Java 12: Switch expressions (preview), `Collectors.teeing()`, `String.indent()`, `String.transform()`
- Java 13: Text blocks (preview), `switch` with `yield` (preview), `FileSystems.newFileSystem()`
- What is `Collectors.teeing()`? Give a use case.

### Java 14 (2020)
- Switch expressions (standard)
- Records (preview)
- Pattern matching for `instanceof` (preview)
- Helpful NullPointerExceptions (JVM flag → default in Java 15)
- `jpackage` tool

### Java 15–16 (2020–2021)
- Java 15: Text blocks (standard), Sealed classes (preview), `String.formatted()`
- Java 16: Records (standard), Pattern matching `instanceof` (standard), `Stream.toList()`, `Stream.mapMulti()`, `Vector API` (incubator)
- What is `Stream.toList()` vs `Collectors.toList()`? (Java 16 `toList()` returns unmodifiable list)
- What is `Stream.mapMulti()`? When is it better than `flatMap()`?

### Java 17 (2021 — LTS)
- Sealed classes (standard)
- Pattern matching for `switch` (preview)
- Removal of RMI Activation, Applet API deprecation
- Strong encapsulation of JDK internals
- Pseudorandom number generators API (`RandomGenerator`)
- Foreign Function & Memory API (incubator)

### Java 18 (2022)
- UTF-8 as default charset
- Simple Web Server (`jwebserver`)
- Code snippets in Javadoc (`@snippet`)
- `Vector` API (incubator, 3rd iteration)

### Java 19–20 (2022–2023)
- Virtual threads (preview → 2nd preview)
- Structured concurrency (incubator)
- Record patterns (preview)
- Pattern matching for `switch` (preview iterations)
- Foreign Function & Memory API (preview)

### Java 21 (2023 — LTS)
- What were the major features of Java 21?
  - Virtual threads (standard — Project Loom)
  - Pattern matching for `switch` (standard)
  - Record patterns (standard)
  - Sequenced Collections (`SequencedCollection`, `SequencedSet`, `SequencedMap`)
  - String Templates (preview)
  - Unnamed classes and instance main methods (preview)
  - Structured concurrency (preview)
  - Unnamed patterns and variables (preview)
  - `Math.clamp()`
- What are Sequenced Collections? What problem do they solve?
- What is `SequencedCollection.getFirst()`, `getLast()`, `addFirst()`, `addLast()`, `reversed()`?

### Java 22–24 (2024–2025)
- Java 22: Unnamed variables `_`, `Stream.gatherers()` (preview), Foreign Function & Memory API (standard), String Templates (2nd preview), Structured Concurrency (2nd preview)
- Java 23: Markdown in Javadoc, Primitive types in patterns (preview), `Module.implAddOpens` removal
- Java 24: Removal of `SecurityManager`, `ahead-of-time class loading`, Generational ZGC (default), `ClassFile` API (standard)
- What is the `ClassFile` API? What does it replace? (ASM-style bytecode manipulation in the JDK)
- What is Generational ZGC? How does it improve upon non-generational ZGC?

### LTS Release Timeline
- What are the LTS (Long-Term Support) versions of Java? (8, 11, 17, 21)
- What is the Java release cadence since Java 9? (6-month release cycle)
- What is the difference between Oracle JDK and OpenJDK licensing?
- What is the support timeline for Java LTS versions?

> **Best Practices (Version Selection):**
> - Target Java 21 LTS for new projects — virtual threads, records, sealed classes, pattern matching all standard.
> - Do not stay on Java 8 for new code — Java 11+ is the minimum viable modern baseline.
> - Use `--enable-preview` only in development; never in production.
> - Check `jdeprscan` before upgrading JDK versions to identify use of deprecated/removed APIs.
> - Pin JDK version in `pom.xml`/`build.gradle` and CI pipelines to ensure reproducible builds.

---

## 59. Tricky Senior-Level OOP Questions

- What is the output when a subclass constructor calls `this.methodName()` and `methodName` is overridden?
- A parent class constructor calls an overridable method. The child overrides it and reads an instance field. What is the output? (Field is `0`/`null` — child fields not yet initialized when parent constructor runs)
- What is the difference between `List<Object>` and `List<?>`?
- Can a `final` method be overloaded?
- What happens when an interface and a superclass both define a method with the same signature? (Class method wins)
- Two interfaces A and B both define `default void foo()`. Class C implements both. What happens? (Compile error — must override)
- Interface A defines `default void foo()`. Interface B extends A and overrides `foo()`. Class C implements both A and B. Which `foo()` runs? (B's — more specific interface wins)
- Can a class implement the same interface twice with different type parameters? (No — `class Foo implements Comparable<String>, Comparable<Integer>` is a compile error)
- What is the difference between `Object.getClass()` and `.class`?
- Is `ArrayList<String>` a subtype of `ArrayList<Object>`? (No — covariant arrays vs invariant generics)
- `String[] sa = new String[1]; Object[] oa = sa; oa[0] = 42;` — what happens? (`ArrayStoreException` at runtime — covariant arrays are unsound)
- Can you catch a generic exception? `catch (T e)` — why not? (Type erasure — JVM doesn't know T at runtime)
- What is the result of `null instanceof String`? (`false` — no exception)
- Can you call a static method on a null reference? (`Foo.staticMethod()` via null ref compiles and runs — resolved by declared type)
- A method declares `throws IOException`. Can you override it to throw `RuntimeException`? (Yes — unchecked exceptions are always allowed)
- A method declares `throws IOException`. Can the override throw no exception? (Yes — you can reduce the exception set)
- What is the difference between `Cloneable` and a copy constructor? Why is copy constructor preferred?
- What is object slicing? Does Java have it? (No — Java uses references, not value semantics like C++)
- Is an inner class instance serializable if the outer class is not? (No — serialization requires all enclosing instances to be serializable too)
- Can an enum constant implement an abstract method differently from another constant? (Yes — constant-specific class body)
- What does `super.super.method()` do? (Compile error — Java does not support skipping levels)
- What is the output of `System.out.println(1 + 2 + "3")`? (`"33"` — left-to-right, `1+2=3` then `3+"3"="33"`)
- What is the output of `System.out.println("1" + 2 + 3)`? (`"123"` — String once encountered, rest concatenated)
- Can you have a constructor in an abstract class? Can you call it? (Yes constructor exists, called via `super()`)
- What is a default method conflict with `Object` methods? (`default boolean equals(Object o)` in interface is NOT allowed — `Object` methods cannot be overridden by default methods)

> **Best Practices:**
> - Know the exact JLS rules for method resolution — interviewers test these edge cases specifically.
> - Always trace object initialization order in questions about constructor calls and overriding.

---

## 60. Tricky Senior-Level Generics & Type System Questions

- What is the difference between `List<? extends Number>` and `List<Number>`?
- Why can't you add to `List<? extends Number>`? (Compiler doesn't know the actual type parameter)
- Why can't you read a typed value from `List<? super Integer>`? (Returns `Object` only)
- What is the difference between `Class<T>` and `Class<?>`?
- Is `List<String>` a subtype of `List<Object>`? (No — generics are invariant)
- Is `String[]` a subtype of `Object[]`? (Yes — arrays are covariant — but this is a design flaw)
- What is the danger of covariant arrays? (Can throw `ArrayStoreException` at runtime)
- What does the compiler do with `Collections.emptyList()` return type? (Type inference infers the target type)
- Can a generic method's return type be inferred when assigned to `var`? (Yes in Java 10+)
- What is a non-denotable type? Can `var` hold one? (Yes — intersection types, captured wildcards)
- Why is `List<List<?>>` different from `List<List<String>>`?
- Can you overload `void foo(List<String> l)` and `void foo(List<Integer> l)`? (No — same erasure)
- What is `@SafeVarargs`? When is it needed?
- What is the difference between `T[]` and `Object[]` in a generic method?
- What is capture conversion? (`List<?>` element captured as `CAP#1 extends Object`)
- Can a generic class have a static field of type `T`? (No — `T` is erased; all instances share the static field)
- What happens to primitive types with generics? Why is `List<int>` not valid? (Type erasure requires reference types; use `List<Integer>`)
- What is a generic singleton factory pattern? (`Collections.emptyList()`, `Optional.empty()`)
- What is recursive generics? Give a real example. (`class Enum<E extends Enum<E>>`)
- What is the difference between `Comparable<T>` and `Comparable<? super T>`?

> **Best Practices:**
> - Use bounded wildcards liberally in API design — they make APIs more flexible for callers.
> - In method parameters, prefer `? super T` for consumer parameters and `? extends T` for producer parameters.
> - Never use raw types in new code — the compiler's unchecked warnings signal real runtime risks.

---

## 61. Tricky Senior-Level Collections Questions

- `HashMap` in Java 7 had a concurrency bug in multi-threaded resize. What was it? (Infinite loop due to circular linked list in bucket after concurrent resize)
- Is `ConcurrentHashMap.size()` exact? (No — it is an approximation; uses `sumCount()`)
- You insert a key with `hashCode() = 0` into a `HashMap`. Where does it go? (Bucket 0)
- What happens if all keys have the same `hashCode()`? (All in one bucket → O(n) operations → becomes a red-black tree at 8 entries in Java 8+)
- What is the difference between `HashMap.remove(key)` and `HashMap.remove(key, value)`?
- `List.of()` vs `Collections.unmodifiableList(list)` — what is the structural difference? (`List.of()` is truly immutable; `unmodifiableList` is a view — the underlying list can still be mutated)
- Is `Collections.synchronizedList` fully thread-safe? (Iteration still requires external synchronization)
- What is the `modCount` field in `ArrayList`? How does it enable fail-fast behavior?
- A `TreeMap` uses a `Comparator` that returns 0 for two unequal objects. What happens? (They are treated as the same key — one overwrites the other)
- Can `null` be a key in `TreeMap`? (No — throws `NullPointerException` since keys are compared)
- Can `null` be a key in `HashMap`? (Yes — stored in bucket 0 with special null-key logic)
- `LinkedHashMap` with `accessOrder=true` — what is the eviction pattern? (Eldest entry evicted — basis for LRU cache)
- How do you implement an LRU cache using standard Java? (`LinkedHashMap` with `accessOrder=true` and overridden `removeEldestEntry()`)
- What is the difference between `ArrayDeque` and `LinkedList` when used as a queue? (`ArrayDeque` is faster, no null elements, not thread-safe; `LinkedList` allows null)
- What happens when you call `iterator.remove()` on a `CopyOnWriteArrayList` iterator? (`UnsupportedOperationException` — COWAL iterators do not support remove)
- What is `PriorityQueue`'s time complexity for `add()`, `poll()`, `peek()`? (add: O(log n), poll: O(log n), peek: O(1))
- What is the space complexity of `HashMap` vs `TreeMap`? (`HashMap` has higher space; `TreeMap` uses tree nodes with 3 pointers each)
- Is `EnumSet` ordered? (Yes — in natural enum declaration order, backed by a bit vector)
- What is `NavigableMap.floorKey(k)` and `ceilingKey(k)`?
- What is `Map.compute(key, remappingFunction)` vs `merge(key, value, mergingFunction)`?

> **Best Practices:**
> - When implementing an LRU cache, use `LinkedHashMap` — simpler and correct.
> - For read-heavy concurrent maps, consider `ConcurrentHashMap` with `computeIfAbsent()` instead of manual locking.
> - Use `Map.entry(k, v)` (Java 9) for creating `Map.Entry` objects in streams.

---

## 62. Tricky Senior-Level Concurrency Questions

- What is the ABA problem in lock-free programming?
- How does `AtomicStampedReference` solve the ABA problem?
- What is false sharing? How does it affect `AtomicLong` in a loop?
- What is a CPU cache line? How does padding help? (`@Contended` annotation / `LongAdder`)
- Can `volatile` guarantee atomicity for `long` and `double`? (Without `volatile`, 64-bit read/write is not atomic on 32-bit JVMs; with `volatile`, it is atomic on all JVMs)
- What is the difference between `synchronized(this)` and `synchronized(MyClass.class)`?
- A thread reads a `volatile` field 1000 times. Is each read a fresh read from main memory? (Yes — every read goes through the memory barrier)
- What is the difference between `Thread.interrupted()` and `Thread.isInterrupted()`? (`interrupted()` clears the flag; `isInterrupted()` does not)
- What happens when a thread that owns a `ReentrantLock` throws an exception without unlocking? (Lock is permanently held — other threads starve)
- What is a spurious wakeup? How do you guard against it? (Use `while` not `if` around `wait()`)
- `ExecutorService.shutdown()` vs `shutdownNow()` — what is the difference? (`shutdown()` waits for running tasks; `shutdownNow()` interrupts them and returns pending task list)
- Is `CompletableFuture.thenApply()` always executed on the same thread? (No — may run on the completing thread or a pool thread)
- What is the difference between `thenApply()` and `thenApplyAsync()`?
- A `CompletableFuture` completes exceptionally. What happens to chained `thenApply()` stages? (They are skipped; only `exceptionally()` or `handle()` stages run)
- What is the difference between `exceptionally()` and `handle()`? (`exceptionally()` handles only exceptions; `handle()` runs always with (result, exception))
- What is a thread pool's task queue full policy? Which `RejectedExecutionHandler` is the default? (`AbortPolicy` — throws `RejectedExecutionException`)
- `newCachedThreadPool()` — what happens if 10,000 tasks are submitted? (10,000 threads created — potential OOM or OS thread exhaustion)
- What is the difference between `ForkJoinPool.commonPool()` and a custom `ForkJoinPool`?
- What is structured concurrency? What problem does it solve?
- What is `ThreadFactory`? Why should you always supply one to `ExecutorService`?
- What is the difference between `CountDownLatch` and `Semaphore`? (Latch is single-use; Semaphore is reusable)
- When does `CyclicBarrier.await()` throw `BrokenBarrierException`?
- What is `Phaser.arriveAndDeregister()`? How does it differ from `await()`?
- What is a thread confinement pattern? Give examples. (ThreadLocal, single-threaded executor, stack confinement)
- What is a publication hazard? What is unsafe publication? (Sharing a partially constructed object — fields may be visible in default state due to JMM reordering)

> **Best Practices:**
> - Always supply a named `ThreadFactory` — unnamed threads make thread dumps unreadable.
> - Never share mutable objects between threads without synchronization or immutability.
> - Test concurrent code with `jcstress` (Java Concurrency Stress tests) for correctness.

---

## 63. Functional Java — Advanced Senior Questions

- What is a monad in Java terms? Is `Optional` a monad? Is `Stream` a monad?
- What is the difference between `map()` and `flatMap()` in monadic terms?
- What is referential transparency? Why do pure functions have it?
- What is a side effect in functional programming? What Java operations are side effects?
- What is a custom `Collector`? What are its four components (`supplier`, `accumulator`, `combiner`, `finisher`)?
- When is the `combiner` of a `Collector` called? (Only in parallel streams)
- What `Characteristics` can a `Collector` declare: `CONCURRENT`, `UNORDERED`, `IDENTITY_FINISH`?
- What is `IDENTITY_FINISH`? What optimization does it enable?
- How do you write a collector that groups elements into fixed-size partitions?
- What is `Collectors.teeing(downstream1, downstream2, merger)`? Give a use case (compute min and max simultaneously).
- What is `Stream.iterate(seed, predicate, next)` (Java 9)? How does it differ from the Java 8 version?
- What is `Stream.takeWhile()` vs `Stream.limit()`? (`takeWhile` stops at first non-matching element; `limit` stops after N elements)
- What is `Stream.dropWhile()`?
- What is `Stream.mapMulti()`? When is it more efficient than `flatMap()`? (Avoids creating intermediate `Stream` per element — uses `BiConsumer`)
- What is `Spliterator`? How do you implement a custom one?
- What is `SUBSIZED` characteristic of a `Spliterator`?
- How does `Stream.parallel()` split work? (Calls `Spliterator.trySplit()` recursively until `LEAF_SIZE` is reached)
- What is lazy evaluation in streams? What is the difference between `limit(5).sorted()` and `sorted().limit(5)`? (`sorted()` must see all elements; order matters for correctness and performance)
- Is `Stream.forEach()` guaranteed to be ordered? (No for parallel streams — use `forEachOrdered()`)
- What is a `Supplier<T>` used for besides lazy initialization? (Factory, retry logic, lazy evaluation, dependency injection)
- What is function memoization? How do you implement it in Java? (`Map.computeIfAbsent(arg, fn)`)
- What is currying? Can you implement it in Java with lambdas? (`Function<A, Function<B, C>>`)
- What is partial application? How does it differ from currying?
- What is the difference between `Function<T,R>` and `UnaryOperator<T>`?
- What is the difference between `Predicate<T>` and `Function<T, Boolean>`?
- What happens if you throw a checked exception inside a lambda? (Must be declared or caught — no checked exception propagation in standard functional interfaces; use a wrapper)
- How do you propagate checked exceptions from within a stream pipeline?
- What is `Optional.flatMap()` vs `Optional.map()`? (When the mapping function already returns `Optional`)

> **Best Practices:**
> - Write pure functions for stream operations — no shared state mutation in lambdas.
> - Separate the `combiner` logic carefully in custom collectors used with parallel streams.
> - Use `mapMulti` for 1-to-few expansions; `flatMap` for 1-to-many with existing collections.

---

## 64. JVM Object Layout & Memory Internals

- What is the object header in HotSpot JVM? What does it contain?
  - Mark word (identity hash, GC age, lock state, monitor pointer)
  - Klass pointer (reference to the class metadata)
  - (Array length for arrays)
- How big is a typical object header? (16 bytes on 64-bit JVM without compressed OOPs; 12 bytes with compressed OOPs)
- What are compressed OOPs (Ordinary Object Pointers)? When are they enabled?
- What is object alignment (padding)? Why are objects aligned to 8 bytes?
- What is the memory layout of a `new Object()`? A `new int[0]`?
- What is a TLAB (Thread-Local Allocation Buffer)? How does it enable fast allocation?
- Why is object allocation in Java "almost free" in most cases?
- What is card table marking in GC? How does it track cross-generation references?
- What is a remembered set in G1GC?
- What is the Safepoint in JVM? Why do GC pauses require all threads to reach a safepoint?
- What is Time-To-Safepoint (TTSP)? How can it be a hidden latency source?
- What is JVM object identity hash code? Where is it stored? (In the mark word — stored on first call; constant thereafter)
- What is biased locking? (Deprecated in Java 15, removed in Java 21 — lightweight lock optimization for single-threaded access)
- What is thin lock vs fat lock (inflated monitor)?
- What is escape analysis? (JVM detects objects that don't "escape" a method → allocates on stack or eliminates entirely)
- What is scalar replacement? (JVM replaces short-lived objects with their fields directly on the stack)
- What is the difference between on-heap and off-heap memory? When would you use off-heap?

> **Best Practices:**
> - Use `-XX:+PrintCompilation` and `-XX:+PrintInlining` to diagnose JIT behavior in hot paths.
> - Avoid creating many small short-lived objects in hot loops — GC pressure adds up even with TLAB.
> - Use `jol-core` (Java Object Layout) tool to inspect actual object sizes and field offsets.

---

## 65. Java Memory Model — Safe Publication & Visibility Traps

- What is safe publication? Why is `new MyObject()` NOT always safe to share across threads?
- What are the four ways to safely publish an object?
  1. Initialize in a static field (`static final`)
  2. Store in a `volatile` field
  3. Store in an `AtomicReference`
  4. Store in a field guarded by a lock (and read under the same lock)
- What is the problem with publishing via a non-final field without synchronization? (Other threads may see partially constructed state due to JMM reordering)
- What is the `final` field visibility guarantee? (After constructor completes, any thread that reads the `final` field sees the correctly initialized value — even without synchronization)
- What is the double-checked locking idiom? Why must the field be `volatile`?
- What is the initialization-on-demand holder (Holder) idiom? Why is it preferred over double-checked locking?
- What is a data race? Give an example.
- What is a benign data race? Is it safe in Java? (JMM prohibits relying on benign races — JIT may break them)
- What is the reordering of stores? What does a store-load barrier do?
- What is `sun.misc.Unsafe.putOrderedObject()` / `VarHandle.setOpaque()`?
- What are `VarHandle` access modes: `plain`, `opaque`, `acquire/release`, `volatile`?
- What is `VarHandle`? (Java 9+ — safer, more performant replacement for `Unsafe` field access)
- What is the difference between `volatile` write/read and `AtomicInteger` CAS in terms of memory barriers?
- What is a publication through a data race? (Sharing an object reference without any synchronization — other threads may see stale reference or stale object state)

> **Best Practices:**
> - Use the holder pattern for lazy singletons — simpler and correct without `volatile`.
> - Never share mutable objects between threads through non-volatile, non-synchronized fields.
> - Use `VarHandle` in framework code needing fine-grained memory ordering instead of `Unsafe`.

---

## 66. Bytecode & Compiler Behavior

- What is Java bytecode? What tool do you use to inspect it? (`javap -c -verbose`)
- What is the constant pool in a `.class` file?
- What is the difference between `invokevirtual`, `invokespecial`, `invokestatic`, `invokeinterface`, `invokedynamic`?
- When is `invokespecial` used? (Constructors, `super.method()`, private methods)
- What is `invokedynamic`? What is it used for in Java? (Lambdas, method references, string concatenation in Java 9+)
- What is a bootstrap method for `invokedynamic`?
- How are lambdas compiled? (Using `invokedynamic` + `LambdaMetafactory` — not anonymous inner classes since Java 8)
- How are string `+` operations compiled in Java 8 vs Java 9+? (Java 8: `StringBuilder`; Java 9+: `invokedynamic` + `StringConcatFactory`)
- What is a synthetic method? When does the compiler generate one? (For inner class access to private outer members)
- What is a bridge method at bytecode level?
- What is the difference between `checkcast` and `instanceof` bytecodes?
- What is the effect of `final` on local variables in terms of bytecode? (None — `final` is a compiler hint only; same bytecode)
- What is the `ConstantValue` attribute in bytecode? (For `static final` primitive fields — inlined at compile time)
- What is dead code elimination by the compiler and JIT?
- What is `ClassFile` API (Java 22+)? How does it replace ASM for bytecode generation?

> **Best Practices:**
> - Use `javap -c ClassName` to verify what the compiler actually emits when debugging puzzling behavior.
> - Understand `invokedynamic` to explain why lambdas are faster than anonymous inner classes in many JVMs.

---

## 67. Java Security Fundamentals

- What is the Java Security Manager? (Deprecated Java 17, removed Java 24)
- What replaced SecurityManager for sandboxing? (Native sandboxing, OS-level process isolation)
- What is the Java Cryptography Architecture (JCA)?
- What is a `MessageDigest`? How do you compute SHA-256 of a string?
- What is `Cipher`? What are ECB vs CBC vs GCM modes? Why is ECB insecure?
- What is `KeyStore`? What is it used for?
- What is `SecureRandom` vs `Random`? Why should you never use `Random` for security?
- What is `KeyPairGenerator`? `KeyAgreement`? `Signature`?
- What is `SSLContext`? What is a `TrustManager`?
- What is certificate pinning?
- What is HMAC? How does it differ from a plain hash?
- What is a salt in password hashing? Why is it needed?
- What is `PBKDF2`, `bcrypt`, `scrypt`, `Argon2`? Why should you use them instead of SHA-256 for passwords?
- What is timing attack? How does `MessageDigest.isEqual()` prevent it? (Constant-time comparison)
- What is deserialization attack? How do you defend against it?
- What is `ObjectInputFilter`? (Java 9+ — whitelist/blacklist of classes allowed during deserialization)
- What are OWASP Top 10 vulnerabilities relevant to Java?
  - Injection (SQL, LDAP, command injection)
  - Broken Authentication
  - Sensitive Data Exposure
  - XXE (XML External Entity)
  - Broken Access Control
  - Security Misconfiguration
  - XSS (in web outputs)
  - Insecure Deserialization
  - Using Components with Known Vulnerabilities
  - Insufficient Logging & Monitoring

> **Best Practices:**
> - Always use `SecureRandom` for tokens, nonces, session IDs — never `Math.random()` or `new Random()`.
> - Store passwords with a slow adaptive hash (BCrypt/Argon2) — never MD5, SHA-1, or plain SHA-256.
> - Validate and sanitize all external input — even from internal services.
> - Use `ObjectInputFilter` to whitelist safe classes before deserializing from untrusted sources.

---

## 68. String Pool & Interning — Deep Dive

- What exactly is the String pool? Where is it stored? (Heap since Java 7; PermGen in Java 6 and earlier)
- How does the JVM implement the String pool? (A `ConcurrentHashMap`-like structure of weak references)
- What is `String.intern()`? What does it return?
- If you call `intern()` on two equal strings, are they `==`? (Yes — both return the canonical pooled reference)
- When are string literals automatically interned? (At class loading — all string literals in bytecode are interned)
- `String s1 = "abc"; String s2 = new String("abc");` — are they `==`? (No)
- `String s1 = "abc"; String s2 = "ab" + "c";` — are they `==`? (Yes — constant folding at compile time)
- `String s1 = "abc"; String s2 = x + "c";` where `x = "ab"` — are they `==`? (No — `x` is a variable; concatenation creates a new object)
- What is the performance cost of `intern()`? (Hash lookup + potential lock contention on the pool)
- When should you explicitly call `intern()`? (Almost never in application code — only in extreme memory optimization scenarios)
- What is `String.isInterned()` (Java 12 — not yet in mainline)?
- Does `StringBuilder.toString()` return an interned string? (No)
- Does `String.concat()` return an interned string? (No)
- What is `String.hashCode()` caching? How is the cached hash invalidated? (It is never invalidated — but `hashCode() == 0` is special-cased to avoid re-computing)
- What happens to interned strings during GC? (They are subject to GC when no other references exist — the pool uses weak references)

---

## 69. CompletableFuture — Advanced Patterns

- What is a non-blocking asynchronous pipeline with `CompletableFuture`?
- What thread runs `thenApply()` if the future is already complete when `thenApply()` is called? (The calling thread — it runs synchronously)
- What is the difference between `thenCompose()` and `thenCombine()`? (`thenCompose` = sequential chaining (flatMap); `thenCombine` = parallel combination of two futures)
- How do you run two futures in parallel and combine their results?
- How do you wait for all of N futures to complete? (`CompletableFuture.allOf(...)`)
- How do you return the first completed result of N futures? (`CompletableFuture.anyOf(...)`)
- What does `CompletableFuture.join()` do vs `get()`? (`join()` throws `CompletionException` (unchecked); `get()` throws checked `ExecutionException`)
- How do you handle a timeout in `CompletableFuture`? (`orTimeout(duration)` Java 9+)
- What is `completeOnTimeout(defaultValue, duration)`?
- How do you convert a callback-based API to `CompletableFuture`? (`new CompletableFuture<>()` + `complete()`/`completeExceptionally()` in callback)
- What is `CompletableFuture.completedFuture(value)`?
- What is `CompletableFuture.failedFuture(throwable)` (Java 9+)?
- What is `CompletableFuture.delayedExecutor(delay, TimeUnit)` (Java 9+)?
- What happens when a `CompletableFuture` stage throws an exception that is not caught? (Silently swallowed unless you call `get()`/`join()` or attach `exceptionally()`/`handle()`)
- How do you cancel a `CompletableFuture`? Does it interrupt the running thread? (`cancel(true)` sets state to cancelled but does NOT interrupt background threads — unlike `FutureTask`)
- What is the difference between `runAsync()` (returns `CompletableFuture<Void>`) and `supplyAsync()` (returns `CompletableFuture<T>`)?
- What is the default thread pool used by `CompletableFuture.supplyAsync()`? (`ForkJoinPool.commonPool()`)
- Why should you provide a custom `Executor` to `supplyAsync()` in production? (commonPool may be shared; better to size your own pool for the workload)

> **Best Practices:**
> - Always attach `.exceptionally()` or `.handle()` to production `CompletableFuture` pipelines.
> - Always provide a custom `Executor` for `supplyAsync`/`runAsync` — never rely on `commonPool` for blocking I/O.
> - Use `orTimeout()` to bound all async operations — unbounded futures can hang indefinitely.
> - Use `thenCompose()` (not `thenApply()`) when the mapping function itself returns a `CompletableFuture`.

---

## 70. Thread Pool Sizing & ExecutorService Patterns

- What is the formula for optimal thread pool size for CPU-bound tasks? (`N_cpu` threads, where `N_cpu = Runtime.getRuntime().availableProcessors()`)
- What is the formula for optimal thread pool size for I/O-bound tasks? (`N_cpu × (1 + W/C)` where W = wait time, C = compute time)
- What is `ThreadPoolExecutor`'s task acceptance policy when the queue is full and max threads reached? (Calls `RejectedExecutionHandler`)
- What is `CallerRunsPolicy`? When is it appropriate? (Executes task in the calling thread — provides natural back-pressure)
- What is the difference between `LinkedBlockingQueue` (unbounded) and `ArrayBlockingQueue` (bounded) as the executor's work queue?
- Why is `Executors.newFixedThreadPool(n)` with a `LinkedBlockingQueue` dangerous under high load? (Queue grows unbounded → OOM)
- What is `SynchronousQueue` used as an executor queue? (`newCachedThreadPool` — no buffering; task must be immediately handed to a thread)
- What is a `ScheduledThreadPoolExecutor`? How does it differ from `Timer`? (Handles exceptions without killing the scheduler; uses thread pools not a single thread)
- What is the problem with `Timer` in the face of exceptions? (A thrown exception kills the timer thread — all future tasks cancelled)
- What is thread starvation? How can a `ForkJoinPool` help? (Work stealing — idle threads steal tasks from busy threads' queues)
- How do you implement a bulkhead pattern with `ExecutorService`? (Separate pools per subsystem — one slow subsystem doesn't starve others)
- What is a `CompletionService`? (`ExecutorCompletionService` — wraps an executor and provides `take()` to consume results as they complete)
- How do you properly drain a thread pool on application shutdown?
  1. `executor.shutdown()`
  2. `executor.awaitTermination(timeout, unit)`
  3. `executor.shutdownNow()` if not terminated
  4. Re-interrupt: `Thread.currentThread().interrupt()`
- What is the difference between `execute()` and `submit()` on `ExecutorService`? (`execute` takes `Runnable` and swallows exceptions; `submit` returns `Future` and stores exceptions)

> **Best Practices:**
> - Use bounded queues + `CallerRunsPolicy` for back-pressure in production executors.
> - Always name your thread pools via `ThreadFactory` — makes thread dumps actionable.
> - Shut down all executors on application exit — un-shut-down pools prevent JVM exit.
> - Separate I/O-bound and CPU-bound work into different pools.

---

## 71. Java Performance & Micro-Optimization

- What is a micro-benchmark? Why is JMH (Java Microbenchmark Harness) required? (JIT warmup, dead code elimination, constant folding, OSR skew all invalidate naive benchmarks)
- What is dead code elimination by JIT? How does JMH's `Blackhole` prevent it?
- What is the cost of object creation in Java? Why is it near-zero with modern GC? (TLAB bump-pointer allocation)
- What is false sharing? How does `@jdk.internal.vm.annotation.Contended` prevent it?
- What is the size of a cache line? (Typically 64 bytes on x86)
- What is `LongAdder` vs `AtomicLong` under contention? (`LongAdder` splits into cells per CPU — reduces CAS contention)
- What is inlining? When does JIT inline a method? (Method is short, called frequently, not virtual or devirtualized)
- What is megamorphic call site? How does it affect JIT? (3+ receiver types at a virtual call site — JIT cannot inline optimally)
- What is on-stack replacement (OSR)? When does it occur? (Replacing an interpreted method mid-execution with compiled code — in long-running loops)
- What is deoptimization? What triggers it? (Assumption invalidated — e.g., new class loaded that changes type hierarchy)
- What is the difference between `ArrayList.get()` and `LinkedList.get()` cache behavior? (`ArrayList` is contiguous memory — CPU cache friendly; `LinkedList` pointer-chasing kills cache)
- What is the cost of autoboxing in a hot loop? (Object allocation + GC pressure)
- What is String interning overhead? (Hash lookup per `intern()` call — fine for startup, costly in hot paths)
- What is heap fragmentation? How does G1GC handle it? (G1 uses regions — can evacuate fragmented regions independently)
- What is `-XX:+UseStringDeduplication`? (G1GC feature — deduplicates `char[]`/`byte[]` backing equal strings to save heap)
- What is `Arrays.parallelSort()` vs `Arrays.sort()`? (`parallelSort` uses ForkJoinPool — faster for large arrays on multi-core)
- What is the cost of exception creation? (Stack trace capture — `new Throwable()` is expensive; avoid using exceptions for control flow)

> **Best Practices:**
> - Never optimize without profiling first — guess-based optimization is rarely correct.
> - Use `async-profiler` or `JFR` (Java Flight Recorder) for production profiling.
> - Profile heap allocation with `-XX:+FlightRecorder` + JDK Mission Control.
> - Reduce megamorphic call sites in hot paths — try to narrow the set of concrete types.

---

## 72. Java Miscellaneous Senior Tricky Questions

- What is the output of `Math.abs(Integer.MIN_VALUE)`? (Still `Integer.MIN_VALUE` — overflow; no positive representation)
- What is `Integer.MIN_VALUE`? (`-2147483648` — no positive counterpart in `int`)
- What is the result of `0.1 + 0.2 == 0.3` in Java? (`false` — floating point representation error)
- What is the result of `-1 >>> 1`? (`Integer.MAX_VALUE` — unsigned right shift fills with 0)
- What is the result of `Integer.MAX_VALUE + 1`? (`Integer.MIN_VALUE` — integer overflow wraps around)
- What is the output of `new Integer(42).equals(new Long(42))`? (`false` — `Integer.equals()` checks for `Integer` type)
- Is `(int) 3.9` equal to `3` or `4`? (`3` — truncation, not rounding)
- Can a `try` block have no `catch` and no `finally`? (No — must have at least one)
- What is the difference between `throw e` and `throw new RuntimeException(e)`? (Former preserves original stack trace; latter wraps it)
- What is the difference between `e.printStackTrace()` and logging the exception? (Prints to stderr; logging goes to log framework with context)
- What is the difference between `System.exit(0)` and `System.exit(1)`? (`0` = normal termination; non-zero = abnormal; `finally` blocks run; shutdown hooks run)
- Does `System.exit()` run `finally` blocks? (Yes — shutdown hooks and finally blocks run before JVM exits)
- What is a shutdown hook? How do you register one?
- What is the difference between `Comparable.compareTo()` returning `0` and `equals()` returning `true`? (Should be consistent — `TreeSet` and `TreeMap` use `compareTo` for equality, not `equals`)
- Is `"abc" == "abc"` always true? (Yes if both are literals — same pool entry; no if one is `new String("abc")`)
- Can you serialize a lambda? (Not reliably — lambdas are not serializable unless the functional interface extends `Serializable`)
- What is `serialPersistentFields`? (Declares which fields are included in serialization — alternative to `transient`)
- What is a `readResolve()` method in serialization? (Returns the object to use after deserialization — used by Singletons)
- What is `writeReplace()` in serialization?
- What is the difference between `String.format()` and `MessageFormat.format()`? (`MessageFormat` is locale-aware and supports choice patterns)
- Is `StringBuilder` thread-safe? (`StringBuffer` is synchronized; `StringBuilder` is not)
- What is `String.chars()`? What does it return? (`IntStream` of character code points)
- What is `String.codePoints()` vs `String.chars()`? (`chars()` returns char values (UTF-16 code units); `codePoints()` returns full Unicode code points including supplementary characters)
- What is the difference between `==` on `Byte`, `Short`, `Integer` for values in range `-128..127`? (Cached instances — `==` works; outside this range creates new objects)
- What is `Character.isWhitespace()` vs `Character.isSpaceChar()`?
- Can `hashCode()` return a negative number? (Yes — valid; `HashMap` masks with `(n-1)` after spreading the hash)
- Is Java call stack bounded? What is the default stack size? (`-Xss` default is 512K-1M; deep recursion causes `StackOverflowError`)
- What is tail call optimization? Does Java support it? (No — JVM does not optimize tail calls; deep recursion must use iteration or trampolining)
- What is trampolining? (Technique to simulate tail-call optimization using `Supplier<T>` chains)

---

## 73. Java 8 → 21 Feature Comparison Quick Reference

| Feature | Introduced | Standard |
|---|---|---|
| Lambda expressions | Java 8 | Java 8 |
| Streams API | Java 8 | Java 8 |
| `Optional<T>` | Java 8 | Java 8 |
| Default methods in interfaces | Java 8 | Java 8 |
| `java.time` | Java 8 | Java 8 |
| `CompletableFuture` | Java 8 | Java 8 |
| JPMS (modules) | Java 9 | Java 9 |
| `var` (local type inference) | Java 10 | Java 10 |
| `HttpClient` | Java 9 (incubator) | Java 11 |
| `String.strip()`, `isBlank()`, `lines()` | Java 11 | Java 11 |
| Switch expressions | Java 12 (preview) | Java 14 |
| Text blocks | Java 13 (preview) | Java 15 |
| Records | Java 14 (preview) | Java 16 |
| `instanceof` pattern matching | Java 14 (preview) | Java 16 |
| `Stream.toList()` | — | Java 16 |
| Sealed classes | Java 15 (preview) | Java 17 |
| Pattern matching for `switch` | Java 17 (preview) | Java 21 |
| Record patterns | Java 19 (preview) | Java 21 |
| Virtual threads | Java 19 (preview) | Java 21 |
| Sequenced Collections | — | Java 21 |
| Structured concurrency | Java 19 (incubator) | Preview Java 21+ |
| `ClassFile` API | Java 22 (preview) | Java 24 |
| Generational ZGC default | — | Java 24 |

---

<!--

## 74. Senior Interview: "Explain the Output" Quick Questions

These predict outputs — tested in senior interviews to verify deep language understanding.

```
// Q1
String a = "hello";
String b = "hello";
System.out.println(a == b);           // true  (same pool literal)
System.out.println(a == new String("hello")); // false (new heap object)

// Q2
Integer x = 127, y = 127;
System.out.println(x == y);           // true  (Integer cache)
Integer p = 128, q = 128;
System.out.println(p == q);           // false (outside cache range)

// Q3
System.out.println(1 + 2 + "3");      // "33"
System.out.println("1" + 2 + 3);      // "123"

// Q4
int i = 0;
System.out.println(i++ + ++i);        // 0 + 2 = 2 (post then pre)

// Q5 — finally return overrides try return
static int test() {
    try { return 1; }
    finally { return 2; }
}
System.out.println(test());           // 2

// Q6 — null instanceof never throws
Object o = null;
System.out.println(o instanceof String); // false

// Q7 — static method hiding
class Parent { static void hi() { System.out.println("Parent"); } }
class Child extends Parent { static void hi() { System.out.println("Child"); } }
Parent p = new Child();
p.hi();                               // "Parent" (static — resolved by reference type)

// Q8 — field hiding (not overriding)
class A { String name = "A"; }
class B extends A { String name = "B"; }
A obj = new B();
System.out.println(obj.name);         // "A" (fields resolved at compile time by reference type)

// Q9 — char arithmetic
char c = 'A';
System.out.println(c + 1);           // 66 (int promotion)
System.out.println((char)(c + 1));   // 'B'

// Q10 — integer division
System.out.println(7 / 2);           // 3 (integer division)
System.out.println(7 / 2.0);         // 3.5

// Q11 — String switch uses equals() + hashCode()
String s = null;
switch(s) { ... }                    // NullPointerException

// Q12 — autoboxing NPE
Integer val = null;
int x = val;                         // NullPointerException (unboxing null)

// Q13 — overflow
byte b = 127;
b++;
System.out.println(b);               // -128 (overflow wraps)

// Q14 — constructor chaining order
class A {
    A() { System.out.println("A"); }
}
class B extends A {
    B() { System.out.println("B"); }
}
new B();                             // prints "A" then "B"

// Q15 — abstract class constructor
// Called implicitly via super() when subclass instantiated
```

> **Key Rules to Memorise:**
> - `==` on objects: identity; `==` on primitives: value
> - Integer cache: `-128` to `127`
> - Static methods: resolved by reference type (not dynamic dispatch)
> - Fields: resolved by reference type (not dynamic dispatch)
> - `finally` always runs and can override `return`
> - String literals: compile-time constants are interned; variable concatenation is not
> - `null instanceof X` is always `false` — never throws NPE
> - Unboxing `null` throws `NullPointerException`
> - `char + int` promotes to `int` — cast back to `char` explicitly

-->
