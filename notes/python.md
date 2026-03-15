
## Python Fundamentals

### Python language overview
- Python is a high-level, interpreted, general-purpose programming language created by Guido van Rossum and first released in 1991.
- It emphasizes code readability through significant whitespace and a clean syntax.
- Python supports multiple programming paradigms: procedural, object-oriented, and functional programming.
- It has a comprehensive standard library and a vast ecosystem of third-party packages.
- Python is dynamically typed and uses garbage collection for memory management.

### Python philosophy (PEP 20)
- PEP 20, also known as "The Zen of Python", lists 19 guiding principles for Python's design.
- Key aphorisms include "Beautiful is better than ugly", "Explicit is better than implicit", and "Simple is better than complex".
- "There should be one-- and preferably only one --obvious way to do it" emphasizes consistency.
- "Readability counts" underpins Python's focus on clean, understandable code.
- The Zen can be accessed by typing `import this` in the Python interpreter.

### Interpreter vs compiler
- An interpreter directly executes source code line by line, while a compiler translates source code into machine code before execution.
- Python is typically implemented as an interpreted language, but it first compiles to bytecode (`.pyc` files) which is then executed by the Python Virtual Machine (PVM).
- Interpreted languages offer greater portability and easier debugging, but can be slower than compiled languages.
- Compiled languages generally produce faster executables but require a separate compilation step.
- Python's hybrid model combines the benefits of interpretation (flexibility) with some performance optimizations via bytecode.

### Python execution model
- Python source code is compiled to bytecode, a low-level platform-independent representation.
- Bytecode is executed by the Python Virtual Machine (PVM), an interpreter loop that runs the bytecode instructions.
- The process involves: source → parser → AST → compiler → bytecode → PVM execution.
- Modules are imported and cached in `sys.modules` to avoid recompilation.
- The global interpreter lock (GIL) limits execution of multiple threads within a single process.

### Variables and dynamic typing
- Variables are references (names) bound to objects; they do not have fixed types.
- Dynamic typing means the type of a variable is determined at runtime based on the object it references.
- Assignment `x = 5` binds the name `x` to an integer object; reassigning `x = "hello"` binds it to a string.
- Type checking occurs during execution, enabling flexible code but requiring careful type handling.
- The `type()` function can be used to inspect the type of an object at runtime.

### Python data types
- Fundamental built-in types include: `int`, `float`, `complex`, `bool`, `str`, `bytes`, `bytearray`, `list`, `tuple`, `set`, `frozenset`, `dict`, `NoneType`.
- Numeric types support arbitrary-precision integers (`int`), floating-point (`float`), and complex numbers.
- Sequences: `str`, `list`, `tuple`; mappings: `dict`; sets: `set`, `frozenset`.
- Every object has a type, identity (returned by `id()`), and value.
- Types can be checked with `isinstance(obj, type)` or `type(obj)`.

### Mutable vs immutable objects
- Mutable objects can be modified after creation (e.g., `list`, `dict`, `set`, custom objects).
- Immutable objects cannot be changed (e.g., `int`, `float`, `str`, `tuple`, `frozenset`).
- Immutability enables hashing (objects can be used as dictionary keys) and safer concurrency.
- Modifying a mutable object affects all references to it; immutability avoids unintended side effects.
- Operations on immutable objects often return new objects, while mutable objects may be updated in-place.

### Numbers and booleans
- Integers (`int`) are arbitrary-precision; floats (`float`) are IEEE 754 double-precision.
- Booleans (`True`, `False`) are a subclass of `int` (True=1, False=0).
- Complex numbers (`complex`) have real and imaginary parts accessed via `.real` and `.imag`.
- Numeric operations include arithmetic (`+`, `-`, `*`, `/`, `//`, `%`, `**`) and bitwise operators.
- Boolean operators: `and`, `or`, `not`; short-circuit evaluation applies.

### Strings
- Strings are immutable sequences of Unicode code points.
- Can be defined with single quotes, double quotes, triple quotes (for multiline).
- String methods: `.split()`, `.join()`, `.strip()`, `.replace()`, `.format()`, f-strings (Python 3.6+).
- Slicing syntax `s[start:stop:step]` extracts substrings.
- Raw strings (`r"..."`) ignore escape sequences, useful for regex and file paths.

### Lists
- Ordered, mutable sequences that can hold heterogeneous elements.
- Created with square brackets `[]` or `list()` constructor.
- Support indexing, slicing, concatenation, and methods like `.append()`, `.extend()`, `.insert()`, `.remove()`, `.pop()`.
- List comprehensions provide concise creation: `[x**2 for x in range(10)]`.
- Lists are implemented as dynamic arrays, offering O(1) amortized append but O(n) insert/delete at arbitrary positions.

### Tuples
- Ordered, immutable sequences; defined with parentheses `()` or `tuple()`.
- Can contain any mix of types; often used for fixed collections like coordinates or database records.
- Immutability makes them hashable (if all elements are hashable), usable as dictionary keys.
- Single-element tuple requires a trailing comma: `(1,)`.
- Named tuples (`collections.namedtuple`) add field names for readability.

### Sets
- Unordered collections of unique, hashable elements.
- Created with `{}` (but empty set must use `set()`) or `set()` constructor.
- Support mathematical set operations: union (`|`), intersection (`&`), difference (`-`), symmetric difference (`^`).
- Fast membership testing (O(1) average) due to hash table implementation.
- Frozen sets (`frozenset`) are immutable and hashable.

### Dictionaries
- Unordered mappings from hashable keys to values.
- Created with `{key: value}` or `dict()` constructor.
- Keys must be immutable and hashable; values can be any object.
- Operations: access via `d[key]`, `d.get(key)`, `d.keys()`, `d.values()`, `d.items()`.
- Dictionary comprehensions: `{x: x**2 for x in range(5)}`.

### Type hints
- Introduced in PEP 484 (Python 3.5) to allow optional static typing.
- Syntax: `variable: type`, `def func(arg: type) -> return_type:`.
- Types can be built-in, custom classes, or from `typing` module (e.g., `List[int]`, `Optional[str]`, `Union`).
- Not enforced at runtime; used by static checkers (mypy), IDEs, and documentation.
- Gradual typing allows mixing typed and untyped code.

### Comments and docstrings
- Comments start with `#` and are ignored by interpreter; used for code explanation.
- Docstrings are string literals immediately after a function/class/module definition, accessible via `obj.__doc__`.
- Docstrings can be single-line or multi-line; follow conventions (PEP 257).
- Tools like Sphinx generate documentation from docstrings.
- Good practice: comments explain "why", docstrings explain "what" and "how".

### Input/output basics
- `input(prompt)` reads a line from stdin, returns as string.
- `print(*objects, sep=' ', end='\n', file=sys.stdout)` writes to stdout.
- File I/O: `open(filename, mode)` returns file object; modes: 'r' (read), 'w' (write), 'a' (append), 'b' (binary).
- Use context managers (`with open(...) as f:`) for automatic resource cleanup.
- `sys.stdin`, `sys.stdout`, `sys.stderr` for standard streams.

### Control flow statements
- Conditional: `if`, `elif`, `else`; no switch statement (but match in Python 3.10+).
- Loops: `for` iterates over iterables; `while` repeats while condition true.
- Loop control: `break` exits loop, `continue` skips to next iteration, `else` clause executes if loop not broken.
- Exception handling: `try/except/else/finally`.
- Context managers (`with`) for resource management.

### Loops and comprehensions
- `for` loops iterate over sequences, iterators, or any iterable.
- List comprehensions: `[expr for item in iterable if condition]`.
- Dictionary comprehensions: `{key_expr: value_expr for item in iterable}`.
- Set comprehensions: `{expr for item in iterable}`.
- Generator expressions: `(expr for item in iterable)` for lazy evaluation.

### Pattern matching
- Introduced in Python 3.10 with PEP 634 (structural pattern matching).
- `match` statement with `case` clauses; similar to switch but more powerful.
- Patterns can match sequences, mappings, classes, and capture variables.
- Guards (`if condition`) add extra constraints.
- Useful for destructuring data and handling complex conditionals elegantly.

## Functions & Functional Concepts
### Function definitions
- Defined with `def` keyword, followed by name, parameters, and colon.
- Body must be indented; can include `return` statement (returns `None` if omitted).
- Functions are first-class objects: can be assigned, passed, returned.
- Scope: variables defined inside function are local unless declared `global` or `nonlocal`.
- Functions can be nested, creating closures.

### Default arguments
- Parameters can have default values: `def func(a, b=5):`.
- Default values are evaluated only once at function definition time (important for mutable defaults).
- Use `None` as default for mutable objects and create new instance inside function.
- Allows optional parameters and reduces overloads.
- Positional arguments must come before default arguments.

### Keyword arguments
- Arguments passed by parameter name: `func(b=3, a=1)`.
- Keyword arguments can be in any order; must follow positional arguments.
- Improve readability and allow skipping defaults.
- Function can require keyword-only arguments by using `*` separator.
- Useful for functions with many optional parameters.

### *args and **kwargs
- `*args` collects extra positional arguments into a tuple.
- `**kwargs` collects extra keyword arguments into a dict.
- Commonly used in decorators, wrappers, and functions that forward arguments.
- Can be used in function calls to unpack sequences (`*list`) and dicts (`**dict`).
- Parameter order: positional, `*args`, keyword-only, `**kwargs`.

### Lambda functions
- Anonymous, single-expression functions defined with `lambda args: expression`.
- Useful for short, throwaway functions (e.g., as key to `sorted()`).
- Cannot contain statements or annotations; limited to expressions.
- Often used with `map()`, `filter()`, `reduce()`.
- Prefer named functions for complex logic to improve readability.

### Closures
- A nested function that remembers variables from the enclosing scope even after that scope exits.
- Created when an inner function references non-local variables from outer function.
- Used in decorators, callback factories, and data hiding.
- The `nonlocal` keyword allows modifying captured variables.
- Closures capture by reference, not by value; late binding can cause issues (e.g., in loops).

### First-class functions
- Functions are objects: can be assigned to variables, stored in data structures, passed as arguments, and returned from other functions.
- Enables higher-order functions and functional programming patterns.
- Functions have attributes (e.g., `__name__`, `__doc__`) and can be introspected.
- Examples: `sorted(iterable, key=len)` passes function `len`.
- Supports dynamic behavior and callbacks.

### Recursion
- A function calling itself; must have base case to avoid infinite recursion.
- Python has a recursion limit (default ~1000) to prevent stack overflow.
- Tail recursion is not optimized; iterative solutions often preferred for deep recursion.
- Useful for tree traversal, divide-and-conquer algorithms.
- Can be elegant but may be inefficient due to function call overhead.

### Higher-order functions
- Functions that take other functions as arguments or return functions.
- Examples: `map()`, `filter()`, `reduce()`, `sorted()`, `partial()`.
- Enable abstraction and code reuse.
- `functools` module provides tools like `partial()`, `reduce()`, `lru_cache`.
- Essential for functional programming in Python.

### Decorators
- A function that takes another function and extends its behavior without modifying it.
- Syntax: `@decorator` above function definition; equivalent to `func = decorator(func)`.
- Can be stacked (`@deco1 @deco2`).
- Often used for logging, timing, access control, memoization.
- Decorators with arguments require an extra nested layer.

### Function annotations
- Optional metadata attached to function parameters and return value.
- Syntax: `def func(a: int, b: str) -> bool:`.
- Not enforced; accessible via `func.__annotations__`.
- Used by type checkers, IDEs, and frameworks (e.g., FastAPI for validation).
- Can be any expression, not just types.

### Partial functions
- `functools.partial` fixes a certain number of arguments of a function, producing a new callable.
- Useful for creating specialized versions of general functions.
- Example: `int_base2 = partial(int, base=2)`.
- Does not evaluate arguments immediately; they are stored and passed when called.
- Similar to currying but more flexible.

### Currying
- Transforming a function that takes multiple arguments into a chain of functions each taking a single argument.
- Not built into Python, but can be implemented manually or with libraries.
- Facilitates partial application and function composition.
- Python's `partial` is often used instead of currying.
- Example: `add = lambda x: lambda y: x + y`; `add(2)(3)` returns 5.

## Object-Oriented Programming
### Classes and objects
- Classes are blueprints for creating objects; defined with `class` keyword.
- Objects are instances of classes; created by calling the class: `obj = MyClass()`.
- Classes define attributes (data) and methods (behavior).
- Attributes can be instance-level (unique per object) or class-level (shared).
- Everything in Python is an object, including classes themselves (they are instances of `type`).

### Instance vs class variables
- Instance variables are defined inside methods (usually `__init__`) and prefixed with `self`.
- Class variables are defined directly in class body and shared across all instances.
- Access via `self` or class name; modifying via instance creates a new instance variable (shadows class var).
- Class variables useful for constants, defaults, and shared state.
- Use `__class__` or `type(self)` to access class variable from instance method.

### Methods
- Instance methods: first parameter `self`, operate on instance.
- Class methods: decorated with `@classmethod`, first parameter `cls`, can access/modify class state.
- Static methods: decorated with `@staticmethod`, no implicit first parameter; like regular functions but namespaced in class.
- Special methods (__dunder__) like `__init__`, `__str__`, `__repr__` customize behavior.
- Methods are just attributes that are callable; bound methods automatically pass instance.

### Constructors
- `__new__` creates a new instance (rarely overridden), `__init__` initializes it.
- `__init__` is called after instance creation; sets up initial state.
- Can have parameters; first parameter `self`.
- If no `__init__` defined, a default no-arg constructor is used.
- `__new__` is useful for immutable objects or singleton patterns.

### Inheritance
- A class can inherit from one or more base classes: `class Child(Parent):`.
- Child class gets all attributes and methods of parent, can override or extend.
- Use `super()` to call parent class methods (e.g., in `__init__`).
- Supports polymorphism: child objects can be used where parent is expected.
- `isinstance(obj, Class)` checks inheritance.

### Multiple inheritance
- A class can inherit from multiple parents: `class C(A, B):`.
- Method resolution order (MRO) determines which parent's method is called first.
- MRO follows C3 linearization algorithm; can be viewed with `ClassName.__mro__`.
- Diamond problem resolved by MRO; parent classes are visited in a consistent order.
- Use with caution to avoid complexity; mixins are common pattern.

### Method resolution order (MRO)
- The order in which base classes are searched when looking for a method.
- Python uses C3 linearization; ensures monotonicity and consistent order.
- Access via `ClassName.__mro__` or `ClassName.mro()`.
- Important for multiple inheritance to avoid ambiguity.
- `super()` follows MRO, not just immediate parent.

### Polymorphism
- Ability of objects of different classes to respond to the same method call.
- In Python, polymorphism is duck-typed: if an object has the required method, it can be used.
- Achieved through inheritance (method overriding) and interfaces (abstract base classes).
- Enables writing generic code that works with many types.
- Example: `len()` works on any object with `__len__` method.

### Encapsulation
- Bundling data and methods that operate on that data within a class.
- Restricting direct access to some components (information hiding).
- Python uses name mangling for "private" attributes: `__attr` becomes `_ClassName__attr`.
- Convention: single leading underscore indicates "protected" (internal use).
- Properties (`@property`) provide controlled access to attributes.

### Abstraction
- Hiding complex implementation details and exposing only essential features.
- Achieved through abstract base classes (ABCs) and interfaces.
- `abc` module: `@abstractmethod` forces subclasses to implement certain methods.
- Promotes separation of concerns and reduces coupling.
- Python's dynamic nature allows duck typing as an alternative to formal interfaces.

### Composition
- Building complex objects by combining simpler objects.
- "Has-a" relationship (e.g., a Car has an Engine).
- Prefer composition over inheritance for code reuse and flexibility.
- Easier to test and modify; changes in component don't affect container as much.
- Can be implemented by storing instances of other classes as attributes.

### Magic methods
- Special methods with double underscores (dunder) that customize object behavior.
- Examples: `__init__`, `__str__`, `__repr__`, `__len__`, `__getitem__`, `__call__`.
- Enable operator overloading and integration with Python language features.
- Define how objects respond to built-in operations (e.g., `+` uses `__add__`).
- Should be implemented following documented protocols.

### Operator overloading
- Giving meaning to operators (+, -, *, etc.) for user-defined classes.
- Implemented via magic methods: `__add__` for `+`, `__sub__` for `-`, etc.
- Can overload comparison operators (`__eq__`, `__lt__`) for sorting.
- Must return appropriate objects; can raise `TypeError` if operation unsupported.
- Used to make custom types behave like built-ins (e.g., vectors, matrices).

### Dataclasses
- Introduced in Python 3.7 (`@dataclass`) to reduce boilerplate for classes that mainly store data.
- Automatically generates `__init__`, `__repr__`, `__eq__`, and optionally other methods.
- Can specify default values, types (with type hints), and field options.
- Supports `frozen=True` for immutability, `order=True` for comparison methods.
- Dataclasses are regular classes; can be extended and customized.

### Slots
- `__slots__` class attribute restricts which attributes an instance can have.
- Prevents dynamic attribute creation, saving memory (no `__dict__` per instance).
- Faster attribute access.
- Subclasses must redeclare `__slots__` if they want to keep the behavior.
- Can cause issues with multiple inheritance; use with care.

### Named tuples
- Factory function `collections.namedtuple` creates tuple subclasses with named fields.
- Provide readability and attribute access while remaining tuple-like (immutable, iterable).
- More memory efficient than dictionaries for simple data containers.
- Example: `Point = namedtuple('Point', ['x', 'y'])`.
- Since Python 3.6, `typing.NamedTuple` allows type hints and docstrings.

## Python Data Structures & Collections
### Built-in collections
- List: ordered, mutable sequence.
- Tuple: ordered, immutable sequence.
- Set: unordered, unique, mutable.
- Dict: unordered key-value mapping.
- String: immutable sequence of characters.

### collections module
- Provides specialized container datatypes.
- Includes `namedtuple`, `deque`, `Counter`, `OrderedDict`, `defaultdict`, `ChainMap`, `UserDict`, `UserList`, `UserString`.
- Extends built-in capabilities for common use cases.
- Often more efficient or convenient than manual implementations.
- Part of standard library; no installation needed.

### defaultdict
- Subclass of `dict` that calls a factory function for missing keys.
- Avoids `KeyError`; automatically creates default value.
- Factory can be `list`, `set`, `int`, or any callable.
- Useful for grouping, counting, or accumulating data.
- Example: `dd = defaultdict(list)` then `dd['key'].append(value)`.

### Counter
- Subclass of `dict` for counting hashable objects.
- Elements stored as keys, counts as values.
- Methods: `.most_common(n)`, `.elements()`, subtraction/union with other Counters.
- Can be created from iterable or mapping.
- Useful for frequency analysis and multiset operations.

### deque
- Double-ended queue (pronounced "deck") from `collections`.
- Optimized for fast appends and pops from both ends (O(1)).
- Supports thread-safe, memory-efficient operations.
- Methods: `append`, `appendleft`, `pop`, `popleft`, `rotate`.
- Can be bounded (maxlen) to act as a fixed-size buffer.

### OrderedDict
- Dictionary subclass that remembers insertion order.
- Since Python 3.7, regular `dict` also preserves insertion order, but `OrderedDict` offers extra methods: `popitem(last=True)`, `move_to_end`.
- Useful when order matters and you need order-sensitive operations.
- Equality tests with regular dicts consider order only if both are OrderedDict.
- Still used for backward compatibility and explicit ordering needs.

### Heap queues
- `heapq` module provides heap queue algorithm (priority queue).
- Heaps are binary trees where parent ≤ children (min-heap).
- Functions: `heappush`, `heappop`, `heapify`, `heapreplace`, `nlargest`, `nsmallest`.
- Useful for scheduling, graph algorithms, merging sorted streams.
- Implemented on lists; O(log n) push/pop.

### Bisect module
- Provides bisection algorithms for sorted lists: `bisect_left`, `bisect_right`, `insort`.
- Uses binary search; O(log n) lookups.
- Maintains list order after insertion.
- Useful for numerical lookups, finding insertion points, and interval queries.
- Lists must be sorted for correct behavior.

### Copying objects
- Assignment (`=`) creates a reference, not a copy.
- Shallow copy: `copy.copy(x)` creates new object but populates with references to original's child objects.
- Deep copy: `copy.deepcopy(x)` recursively copies all objects.
- For built-in collections, can use `list.copy()`, `dict.copy()`, slicing `[:]` for lists.
- Custom objects may need to define `__copy__` and `__deepcopy__` methods.

### Deep vs shallow copy
- Shallow copy: only the top-level container is duplicated, nested objects are shared.
- Changes to nested mutable objects in copy affect original and vice versa.
- Deep copy: creates independent copies of all objects recursively.
- Deep copy may be expensive for large structures; can cause infinite recursion if cycles exist.
- Use shallow copy when structure is simple or you want shared nested objects.

## Error Handling
### Exceptions hierarchy
- Base class: `BaseException`; system-exiting exceptions like `SystemExit`, `KeyboardInterrupt` inherit from it.
- `Exception` is base for all non-system-exiting exceptions; user-defined exceptions should inherit from it.
- Common built-ins: `AttributeError`, `TypeError`, `ValueError`, `KeyError`, `IndexError`, `IOError` (alias `OSError`).
- Hierarchy allows catching groups of exceptions.
- Use `except Exception:` to catch most errors, but avoid bare `except:`.

### try/except/finally
- `try` block contains code that may raise exception.
- `except` block(s) handle specific exceptions; can have multiple.
- `else` block executes if no exception occurred.
- `finally` block always executes, used for cleanup (closing files, releasing resources).
- Exceptions propagate up until caught; uncaught exceptions terminate program.

### Custom exceptions
- Define by subclassing `Exception` (or its subclasses).
- Can add custom attributes and methods for additional context.
- Typically named with "Error" suffix.
- Raise with `raise CustomError("message")`.
- Helps in creating domain-specific error handling.

### Raising exceptions
- Use `raise` statement to trigger an exception.
- Can raise built-in or custom exceptions.
- `raise` without arguments re-raises the last exception in an except block.
- Can chain exceptions with `raise ... from ...` for context.
- `assert` raises `AssertionError` if condition false; used for debugging, not for runtime errors.

### Assertions
- `assert condition, message` raises `AssertionError` if condition false.
- Used for sanity checks during development; can be disabled with `-O` flag.
- Not for validating user input or handling expected errors.
- Helps document assumptions and catch bugs early.
- Overuse can lead to code that fails in production if disabled.

### Exception chaining
- Implicit chaining: when a new exception is raised during handling of another, original exception is attached as `__context__`.
- Explicit chaining: `raise NewException from original_exception` sets `__cause__`.
- `from None` suppresses context.
- Chaining preserves traceback and provides debugging context.
- Useful when translating exceptions between layers (e.g., DB errors to domain errors).

## Modules & Packaging
### Modules and imports
- A module is a `.py` file containing Python code.
- Import with `import module`, `from module import name`, `from module import *` (discouraged).
- `import` searches `sys.path` (list of directories).
- Modules are cached in `sys.modules`; subsequent imports reuse the same module object.
- `__name__` variable: `"__main__"` if run directly, else module name.

### Package structure
- A package is a directory containing an `__init__.py` file (can be empty).
- Subpackages are subdirectories with their own `__init__.py`.
- Allows hierarchical organization of modules.
- Import using dot notation: `import package.subpackage.module`.
- Packages can be distributed and installed.

### __init__.py
- Marks a directory as a Python package.
- Can contain initialization code, set `__all__` for `from package import *`.
- Executed when package is first imported.
- Can be empty; Python 3.3+ supports namespace packages without `__init__.py`.
- Used to expose specific modules or subpackages at package level.

### Virtual environments
- Isolated Python environments with their own interpreter, libraries, and scripts.
- Created with `venv` module: `python -m venv myenv`.
- Activated (`source myenv/bin/activate`) to use; deactivated with `deactivate`.
- Prevents dependency conflicts between projects.
- `pip` installs packages into the active environment.

### pip and dependency management
- `pip` is the standard package installer.
- `pip install package` fetches from PyPI.
- `requirements.txt` lists dependencies with versions: `pip install -r requirements.txt`.
- `pip freeze` outputs current environment's packages.
- Tools like `pipenv`, `poetry` offer more advanced dependency management.

### Packaging standards
- Setuptools and `setup.py` (or `pyproject.toml`) define package metadata.
- Metadata includes name, version, author, dependencies, entry points.
- `sdist` creates source distribution; `bdist_wheel` creates wheel (binary) distribution.
- Wheels are faster to install and platform-specific if needed.
- Upload to PyPI using `twine`.

### Python packaging tools
- `setuptools`: most common for building and distributing.
- `wheel`: built-package format.
- `pip`: installer.
- `twine`: secure upload to PyPI.
- `poetry`, `flit`: modern alternatives simplifying packaging.

### Versioning
- Semantic versioning (major.minor.patch) often used.
- PEP 440 defines version specifiers for Python packages.
- Version constraints in `requirements.txt`: `>=1.2,<2.0`.
- Compatible release (`~=1.2`) means >=1.2, <2.0.
- Important for dependency resolution and avoiding breakage.

### Distribution
- Source distribution (sdist): contains source code, requires compilation on target.
- Wheel (bdist_wheel): pre-built, no compilation needed.
- Platform-specific wheels for C extensions.
- Can distribute via PyPI (public) or private indices.
- Also possible to distribute as standalone executables (PyInstaller, cx_Freeze).

## File Handling & I/O
### File operations
- Open: `open(file, mode='r', encoding=None)`.
- Read: `read()`, `readline()`, `readlines()`.
- Write: `write()`, `writelines()`.
- Close: `close()` (usually via context manager).
- Modes: 'r' (read), 'w' (write, truncate), 'x' (exclusive create), 'a' (append), 'b' (binary), 't' (text default), '+ (update).

### Context managers
- Objects that define `__enter__` and `__exit__` methods.
- Used with `with` statement to manage resources.
- Guarantee cleanup even if exception occurs.
- Common examples: file objects, locks, database connections.
- Can create custom context managers using `contextlib.contextmanager` decorator.

### with statement
- Syntax: `with expression as variable:`.
- Enters context manager, executes block, exits automatically.
- Equivalent to try/finally for resource management.
- Multiple context managers: `with A() as a, B() as b:`.
- Reduces boilerplate and improves readability.

### Reading/writing files
- Text files: specify encoding (UTF-8 default).
- Binary files: mode 'rb'/'wb' for bytes.
- Iterate over file lines: `for line in file:`.
- Use `pathlib` for path manipulations (modern, cross-platform).
- For large files, read in chunks or line by line to avoid memory issues.

### JSON handling
- `json` module: `dump`, `dumps`, `load`, `loads`.
- Serialize Python objects to JSON: `json.dumps(obj)`.
- Deserialize: `json.loads(str)`.
- Supports custom encoders/decoders for non-serializable types.
- JSON is text-based, human-readable, widely used in APIs.

### CSV handling
- `csv` module: reader, writer, DictReader, DictWriter.
- Handles different dialects (Excel, etc.).
- Read: `csv.reader(file)` returns row lists.
- Write: `csv.writer(file).writerow(row)`.
- For large data, consider `pandas` for more features.

### Pickling
- `pickle` module serializes Python objects to byte streams.
- `pickle.dump(obj, file)`, `pickle.load(file)`.
- Can serialize almost any Python object (including custom classes).
- Not secure (can execute arbitrary code); only unpickle trusted data.
- Format is Python-specific; not interoperable with other languages.

### Serialization
- Converting objects to byte streams for storage/transmission.
- Options: JSON, pickle, YAML, XML, MessagePack, Protobuf.
- Choose based on interoperability, performance, security, readability.
- Python's `marshal` is internal, used for bytecode.
- Serialization often used in caching, RPC, data storage.

### Compression formats
- `gzip`, `bz2`, `lzma`, `zipfile`, `tarfile` modules.
- Read/write compressed files transparently.
- Example: `gzip.open('file.gz', 'rt')` for text.
- Zip files: `zipfile.ZipFile` for reading/writing archives.
- Compression reduces storage/transfer size, at cost of CPU.

## Python Internals
### Python object model
- Everything is an object: integers, strings, functions, classes, etc.
- Each object has a header: ob_refcnt (reference count), ob_type (type pointer), and value.
- Objects are allocated on heap; managed by memory manager.
- Types are also objects (instances of `type` or metaclass).
- Objects can be immutable (e.g., int) or mutable (e.g., list).

### Reference counting
- Main memory management mechanism in CPython.
- Each object has an `ob_refcnt` field counting number of references.
- When count drops to 0, object is deallocated immediately.
- `sys.getrefcount(obj)` returns current count (including temporary).
- Cyclic references not handled by reference counting alone (requires GC).

### Garbage collection
- CPython has a cyclic garbage collector to detect and collect reference cycles.
- `gc` module provides interface: `gc.collect()`, `gc.get_threshold()`, `gc.disable()`.
- Collector runs periodically based on allocation count.
- Cycles involving objects with `__del__` are more complex; handled specially.
- Important for container objects like lists, dicts that can form cycles.

### Memory management
- Python uses private heap for objects; memory manager allocates from OS.
- Arenas, pools, blocks: small object allocator for objects ≤ 512 bytes.
- Large objects use system `malloc`.
- Memory is not necessarily freed to OS; may be reused.
- Tools like `tracemalloc` track memory allocations.

### Python stack frames
- Each function call creates a frame object on call stack.
- Frame holds local variables, code object, instruction pointer, etc.
- Frames are linked to previous frame for tracebacks.
- `inspect` module can access frame objects.
- Stack depth limit (default 1000) prevents infinite recursion.

### Bytecode
- Python compiles source to bytecode (`.pyc` files) stored in `__pycache__`.
- Bytecode is a set of instructions for the Python virtual machine.
- `dis` module disassembles bytecode for inspection.
- Platform-independent; same bytecode runs on any CPython.
- Bytecode can be optimized (e.g., `-O` flag removes asserts).

### CPython internals
- Reference implementation of Python written in C.
- Consists of parser, compiler, bytecode interpreter, and runtime.
- Core data structures: PyObject, PyTypeObject, etc.
- GIL, memory management, and standard library in C.
- Understanding CPython helps with debugging extensions and performance.

### GIL (Global Interpreter Lock)
- A mutex that protects access to Python objects, preventing multiple threads from executing Python bytecode simultaneously.
- Simplifies memory management (no need for fine-grained locks on objects).
- Limits CPU-bound multi-threading performance.
- I/O-bound threads release GIL while waiting, allowing concurrency.
- Multiprocessing bypasses GIL by using separate processes.

### Python memory model
- Defines how memory operations behave in multi-threaded programs.
- Python's memory model is simple due to GIL: operations appear atomic for some built-in types.
- However, modifications to shared data still need locks to avoid race conditions.
- The GIL does not guarantee atomicity of operations like `x += 1`.
- `threading` and `queue` provide synchronization tools.

### Interning
- Reusing immutable objects to save memory and speed up comparisons.
- Small integers (-5 to 256) are interned automatically.
- Strings may be interned if they look like identifiers (PEP 237).
- `sys.intern()` can manually intern strings.
- Interning allows fast pointer comparison (`is`) instead of string compare.

### Small object caching
- CPython caches and reuses small immutable objects (e.g., small ints, empty tuple).
- Reduces allocation overhead.
- For ints, a global array holds small integers; others allocated on demand.
- This caching is implementation-specific; not guaranteed in other implementations.
- Affects memory usage and performance of numeric code.

## Iterators & Generators
### Iterator protocol
- Objects that implement `__iter__()` (return self) and `__next__()` methods.
- `__iter__` should return an iterator; `__next__` returns next item or raises `StopIteration`.
- Iterables (e.g., lists) implement `__iter__` returning a new iterator.
- `for` loop calls `iter(iterable)` then repeatedly `next()`.
- Custom iterators can be created by defining these methods.

### Iterable vs iterator
- Iterable: an object that can produce an iterator (has `__iter__()`).
- Iterator: an object that keeps state and produces values one by one (has `__next__()`).
- Iterators are also iterable (they return self in `__iter__`).
- Examples: list is iterable; `iter(list)` returns an iterator.
- Generators are a convenient way to create iterators.

### Generators
- Functions that use `yield` instead of `return`.
- When called, they return a generator object (iterator).
- Each `yield` suspends function state; next call resumes after yield.
- Generators are memory-efficient for large sequences (lazy evaluation).
- Can be used to implement infinite sequences, pipelines, and coroutines.

### yield keyword
- Used in generator functions to produce a value and pause execution.
- Function's local state is saved for next `next()` call.
- `yield from` delegates to another generator (subgenerator).
- Can receive values via `.send(value)` if used in coroutine context.
- `return` in generator signals `StopIteration` with optional value.

### Generator expressions
- Syntax similar to list comprehension but with parentheses: `(x**2 for x in range(10))`.
- Returns a generator object that yields items lazily.
- Memory efficient compared to list comprehension.
- Can be used in function calls (e.g., `sum(x**2 for x in range(10))`).
- Useful for processing streams of data without storing all.

### Coroutines
- Generators extended with `.send()`, `.throw()`, `.close()` methods.
- Can receive values and act as cooperative multitasking units.
- `yield` can be an expression that receives sent values.
- Used in async frameworks (but modern async/await replaced many coroutine use cases).
- Still useful for data pipelines and state machines.

### Lazy evaluation
- Delaying computation until result is needed.
- Generators and iterator protocols enable lazy sequences.
- Saves memory and may improve performance if not all values needed.
- Examples: `range()` in Python 3 is lazy; `itertools` functions produce lazy iterators.
- Can lead to more efficient algorithms for large data.

### itertools module
- Collection of tools for efficient looping and iterator manipulation.
- Infinite iterators: `count`, `cycle`, `repeat`.
- Combinatoric generators: `product`, `permutations`, `combinations`.
- Iterators terminating on shortest input: `chain`, `compress`, `dropwhile`, `groupby`, `islice`, `zip_longest`.
- Functional tools: `accumulate`, `starmap`, `filterfalse`.
- All functions return iterators, composing to build complex pipelines.

## Advanced Language Features
### Metaclasses
- Classes are themselves objects; metaclasses are classes of classes.
- Default metaclass is `type`; custom metaclass inherits from `type`.
- Metaclass `__new__` and `__init__` control class creation.
- Used for class validation, registration, and modifying class behavior.
- Popular use: ORMs (SQLAlchemy), Django models.

### Descriptors
- Objects that define `__get__`, `__set__`, `__delete__` methods.
- Control attribute access in classes.
- Used to implement properties, static methods, class methods.
- Data descriptors (with `__set__` or `__delete__`) override instance dictionaries.
- Non-data descriptors (only `__get__`) are overridden by instance attributes.

### Monkey patching
- Dynamically modifying classes or modules at runtime.
- Can change behavior without altering source code.
- Useful for testing (mocking), hotfixes, and extending libraries.
- Risks: can lead to unpredictable behavior and maintenance issues.
- Should be used sparingly and documented.

### Introspection
- Ability to examine objects at runtime.
- Functions: `type()`, `dir()`, `getattr()`, `hasattr()`, `isinstance()`, `callable()`.
- Inspect module provides more advanced introspection (stack, signatures).
- Useful for debugging, dynamic dispatch, and framework building.
- Introspection is a key feature of Python's dynamic nature.

### Reflection
- Ability to modify program behavior at runtime based on introspection.
- Includes changing attributes, calling methods dynamically, creating classes.
- Examples: `setattr(obj, 'attr', value)`, `getattr(obj, 'method')()`.
- `__import__` to import modules dynamically.
- Enables metaprogramming and flexible designs.

### Dynamic typing internals
- Variables are just names referencing objects; objects carry type information.
- Operations check object's type at runtime (e.g., `+` may call `__add__`).
- Type checking occurs during attribute lookup and method calls.
- Allows duck typing: "if it walks like a duck and quacks like a duck, it's a duck."
- Performance overhead compared to static typing; mitigated by caching and optimizations.

### __slots__
- Class attribute that restricts instance attributes to a fixed set.
- Prevents `__dict__` creation, saving memory (especially for many instances).
- Access to slotted attributes is faster.
- Must be defined in class; subclasses need their own `__slots__` or will have `__dict__`.
- Cannot add new attributes not listed in `__slots__`.

### Weak references
- References that do not increase object's reference count.
- `weakref` module provides `ref`, `WeakKeyDictionary`, `WeakValueDictionary`.
- Useful for caches and avoiding reference cycles.
- Callback can be invoked when object is garbage collected.
- Not all objects can be weakly referenced (e.g., ints, tuples).

### Abstract base classes
- ABCs define interfaces and can enforce method implementation.
- `abc` module: `ABC` class, `@abstractmethod` decorator.
- Can be subclassed; cannot be instantiated if abstract methods not implemented.
- `isinstance(obj, SomeABC)` works for classes that register or inherit.
- Used in collections.abc for container protocols (e.g., `Iterable`, `Sequence`).

### Protocols
- Informal interfaces defined by sets of methods (e.g., iterator protocol).
- Structural subtyping: if a class implements required methods, it satisfies the protocol.
- Python 3.8+ introduces `typing.Protocol` for static structural typing.
- Protocols allow duck typing with static checking.
- Examples: `Iterable`, `Sized`, `Container` from `collections.abc`.

### Structural typing
- Type compatibility based on structure rather than explicit inheritance.
- Python's duck typing is a dynamic form of structural typing.
- With `Protocol`, static type checkers can enforce structural requirements.
- Enables flexible code that works with any object providing needed methods.
- Popular in generic programming and library design.

## Concurrency & Parallelism
### Threads in Python
- Lightweight units of execution within a process; shared memory.
- `threading` module provides thread abstraction.
- Affected by GIL: only one thread executes Python bytecode at a time.
- Useful for I/O-bound tasks where threads release GIL during blocking calls.
- Requires synchronization (locks) to avoid race conditions.

### Multiprocessing
- `multiprocessing` module spawns separate processes, each with its own Python interpreter.
- Bypasses GIL, enabling true parallelism for CPU-bound tasks.
- Processes have separate memory; communication via pipes, queues, shared memory.
- More overhead than threads (process creation, IPC).
- Provides `Pool` for parallel map/reduce operations.

### Threading vs multiprocessing
- Threads: lightweight, shared memory, GIL limits CPU parallelism.
- Multiprocessing: heavier, separate memory, true parallelism, IPC overhead.
- Choose threading for I/O-bound, multiprocessing for CPU-bound.
- Asyncio is another alternative for I/O-bound with single thread.
- Often combined: multiprocessing for CPU, threading for I/O within each process.

### Asyncio basics
- Library for writing concurrent code using async/await syntax.
- Single-threaded, single-process; uses event loop to manage tasks.
- Tasks voluntarily yield control (await) on I/O or other blocking operations.
- Ideal for I/O-bound, high-level structured network code.
- Provides coroutines, futures, tasks, and synchronization primitives.

### Event loop
- Core of asyncio: runs until all tasks are complete.
- Manages scheduling of coroutines, handles I/O events via selectors.
- Can be retrieved with `asyncio.get_running_loop()`.
- Customization: set different event loop policies (e.g., for Windows).
- Event loop runs in main thread; run until stop.

### Async/await
- `async def` defines a coroutine function.
- `await` suspends coroutine until awaitable completes.
- Awaitables: coroutines, futures, tasks, objects with `__await__`.
- Provides structured concurrency; easier to reason than callbacks.
- Used in asyncio, but also in other libraries (e.g., Trio, Curio).

### Futures
- Represent result of asynchronous operation (may not be available yet).
- `concurrent.futures` module: `Future` for thread/process pools.
- Asyncio has its own `Future` class (similar).
- Can attach callbacks, wait for result, check if done.
- Used to bridge callback-based code with async/await.

### Executors
- `concurrent.futures.Executor` abstract class for running callables asynchronously.
- `ThreadPoolExecutor` (threads) and `ProcessPoolExecutor` (processes).
- Submit tasks, get futures; `map()` for parallel iteration.
- Context manager ensures shutdown.
- Useful for mixing threads/processes with asyncio via `loop.run_in_executor()`.

### Thread safety
- Code that works correctly when accessed by multiple threads.
- In Python, GIL ensures atomicity for some operations (e.g., `list.append`) but not all (e.g., `x += 1`).
- Use locks (`threading.Lock`), RLocks, semaphores to protect shared data.
- Queue module provides thread-safe FIFO.
- Avoid shared state where possible; use immutable data or message passing.

### Race conditions
- Unpredictable behavior when multiple threads access shared data without synchronization.
- Example: two threads incrementing a counter may lose updates.
- Can lead to crashes, data corruption, or incorrect results.
- Prevented by locks, atomic operations, or using thread-safe structures.
- Hard to debug; careful design and testing required.

### Locks and synchronization
- `Lock`: mutual exclusion; only one thread can acquire at a time.
- `RLock`: reentrant lock; same thread can acquire multiple times.
- `Semaphore`: limits concurrent access to a resource.
- `Event`: one thread signals others.
- `Condition`: allows threads to wait for a condition.
- Use `with lock:` for automatic release.

### Concurrent programming patterns
- Producer-consumer: use Queue to pass data between threads.
- Thread pool: reuse threads for multiple tasks.
- Map-reduce: parallel processing of data chunks.
- Pipeline: stages connected by queues.
- Future/promise: represent eventual result.

### GIL implications
- CPU-bound threads cannot speed up computation due to GIL.
- I/O-bound threads benefit because GIL is released during blocking syscalls.
- Multiprocessing needed for CPU-bound parallelism.
- C extensions can release GIL during long-running operations.
- GIL is specific to CPython; other implementations (Jython, IronPython) may not have it.

### CPU-bound vs IO-bound workloads
- CPU-bound: tasks that spend most time computing (e.g., numerical calculations).
- IO-bound: tasks that spend time waiting for I/O (network, disk, user input).
- For CPU-bound, use multiprocessing or asyncio with run_in_executor for compute.
- For IO-bound, use asyncio or threading.
- Mixing requires careful design (e.g., using threads for I/O, processes for CPU).

## Performance Optimization
### Profiling Python code
- Measure where time is spent to identify bottlenecks.
- `cProfile` module: deterministic profiling, outputs statistics.
- `profile` module (pure Python) slower but more flexible.
- `pstats` to analyze profile results.
- Visualizers: snakeviz, gprof2dot.

### Memory profiling
- Identify memory leaks and high memory usage.
- `tracemalloc` traces memory allocations; snapshot comparison.
- `memory_profiler` (third-party) line-by-line memory usage.
- `objgraph` for visualizing object references.
- `gc` module can debug cyclic references.

### Time complexity
- Big-O notation describes algorithm efficiency.
- Python operations have known complexities (e.g., list index O(1), dict lookup O(1) average).
- Choose data structures based on time complexity (e.g., set for membership).
- Loops and nested loops can increase complexity.
- Optimize algorithms before low-level tweaks.

### Optimization strategies
- Measure first; don't optimize prematurely.
- Use built-in functions (written in C) where possible.
- Avoid function calls in tight loops; inline or use local variables.
- Use list comprehensions/generators over explicit loops.
- Consider algorithmic improvements (e.g., caching, precomputation).

### C extensions
- Write performance-critical parts in C and wrap with Python.
- Use Python/C API or tools like Cython, CFFI.
- Can release GIL for true parallelism.
- Complex but can yield huge speedups.
- Distributing C extensions requires compilation on target platforms.

### Cython
- Superset of Python that compiles to C.
- Adds static type declarations for speed.
- Can call C functions and release GIL.
- Produces C extensions that can be imported like Python modules.
- Great for numeric code and wrapping C libraries.

### Numba
- Just-in-time (JIT) compiler for Python functions.
- Decorators like `@jit` compile to machine code using LLVM.
- Best for numerical computations (NumPy arrays).
- Can release GIL and parallelize loops.
- No need to write C; works on subset of Python/NumPy.

### Vectorization
- Using array operations (NumPy) to apply operations to entire arrays without explicit loops.
- Leverages optimized C/Fortran libraries (BLAS, LAPACK).
- Faster than Python loops for numerical data.
- Example: `arr + 1` adds 1 to each element.
- Also applies to pandas DataFrame operations.

### Multiprocessing optimization
- Use `Pool.map()` for data parallelism.
- Chunk size tuning to balance overhead.
- Avoid large data transfer; use shared memory (`multiprocessing.Array`, `Value`) where safe.
- Consider `concurrent.futures.ProcessPoolExecutor` for higher-level API.
- Watch for pickling overhead; use `multiprocessing.Manager` for shared objects.

### Async performance
- Asyncio shines for many concurrent I/O connections.
- Avoid blocking calls in coroutines; use `await` and `asyncio.sleep`.
- Use `asyncio.gather()` for concurrent tasks.
- Limit number of concurrent tasks to avoid resource exhaustion.
- Profile with `asyncio` debugging (`PYTHONASYNCIODEBUG=1`).

### Caching strategies
- Store results of expensive computations for reuse.
- `functools.lru_cache` memoizes function calls based on arguments.
- `cachetools` library provides more caching policies (TTL, LFU).
- Use `functools.singledispatch` for generic functions with caching.
- Be mindful of memory usage; set cache size limits.

## Python Standard Library Deep Dive
### os module
- Provides operating system dependent functionality.
- File/directory operations: `os.listdir`, `os.mkdir`, `os.remove`, `os.rename`.
- Path manipulations: `os.path.join`, `os.path.exists` (prefer `pathlib` now).
- Process management: `os.system`, `os.getpid`, `os.environ`.
- Low-level file descriptors: `os.open`, `os.read`, `os.write`.

### sys module
- System-specific parameters and functions.
- `sys.argv`: command-line arguments.
- `sys.path`: module search path.
- `sys.modules`: dict of imported modules.
- `sys.stdout`, `stdin`, `stderr`: standard streams.
- `sys.getrefcount`, `sys.getsizeof`, `sys.setrecursionlimit`.

### pathlib
- Object-oriented filesystem paths (Python 3.4+).
- Path classes: `Path`, `PurePath` (platform-independent).
- Methods: `.read_text()`, `.write_text()`, `.mkdir()`, `.glob()`, `.resolve()`.
- Offers cleaner, cross-platform path handling than `os.path`.
- Path objects can be used with `open()` directly.

### subprocess
- Spawn new processes, connect to pipes, get return codes.
- `subprocess.run()` (recommended) runs command, returns CompletedProcess.
- `Popen` class for advanced control (pipes, asynchronous I/O).
- Capture output with `stdout=subprocess.PIPE`.
- Avoid `os.system`; prefer subprocess for security and flexibility.

### threading module
- High-level threading interface.
- `Thread` class: target function, start, join.
- Synchronization: `Lock`, `RLock`, `Semaphore`, `Event`, `Condition`.
- `local()` for thread-local data.
- `Timer` for delayed execution.

### multiprocessing module
- Spawn processes using similar API to threading.
- `Process` class, `Pool` for worker pools.
- Inter-process communication: `Queue`, `Pipe`, `Value`, `Array`.
- `Manager` for shared objects across processes.
- `Lock`, `Semaphore` etc. for synchronization.

### functools
- Higher-order functions and operations on callables.
- `partial`: fix arguments.
- `lru_cache`: memoization decorator.
- `reduce`: apply function cumulatively.
- `wraps`: preserve metadata when writing decorators.
- `singledispatch`: generic functions based on first argument type.

### itertools
- Efficient looping tools (discussed earlier).
- Infinite iterators, combinatorics, and terminating iterators.
- Chain, cycle, accumulate, groupby, permutations, etc.
- All functions return iterators, enabling lazy composition.
- Essential for functional-style data processing.

### collections
- Container datatypes: `namedtuple`, `deque`, `Counter`, `OrderedDict`, `defaultdict`, `ChainMap`.
- Abstract base classes for containers: `Container`, `Iterable`, `Sequence`, `MutableSet`, etc.
- UserDict, UserList, UserString for easy subclassing.
- `collections.abc` provides ABCs for type checking.

### logging
- Flexible logging system for applications.
- Levels: DEBUG, INFO, WARNING, ERROR, CRITICAL.
- Components: Loggers, Handlers, Filters, Formatters.
- Configure via dictConfig or fileConfig.
- Best practices: use `__name__` as logger, avoid root logger in libraries.

### argparse
- Command-line argument parsing.
- Define arguments, options, subcommands.
- Generates help and usage messages.
- Handles type conversion, choices, defaults.
- Preferred over `getopt` and `optparse`.

## Testing
### Unit testing
- Testing individual units (functions, classes) in isolation.
- Use test frameworks to automate.
- Write test cases covering edge cases and typical usage.
- Arrange-Act-Assert pattern.
- Helps catch regressions and ensures correctness.

### unittest framework
- Built-in xUnit style testing.
- `unittest.TestCase` with assertion methods (`assertEqual`, `assertTrue`, etc.).
- Setup/teardown: `setUp`, `tearDown` methods.
- Test discovery via `unittest.main()` or command line.
- Supports test suites and skipping.

### pytest
- Third-party testing framework, simpler syntax.
- Uses plain `assert` statements.
- Fixtures for setup/teardown, dependency injection.
- Powerful plugins and integrations.
- Highly extensible and widely adopted.

### Mocking
- Replace real objects with mock objects during testing.
- `unittest.mock` module (standard library).
- `Mock` and `MagicMock` classes; `patch` decorator/context manager.
- Verify calls, set return values, simulate exceptions.
- Essential for isolating unit tests from external dependencies.

### Fixtures
- In pytest, fixtures provide a fixed baseline for tests.
- Defined with `@pytest.fixture`, can be scoped (function, class, module, session).
- Automatically injected into test functions.
- Can use `yield` for teardown code.
- Reduces boilerplate and promotes reusability.

### Test coverage
- Measure how much code is executed by tests.
- `coverage.py` tool: `coverage run -m pytest`, `coverage report`.
- Identify untested lines or branches.
- Aim for high coverage, but 100% is not always necessary.
- Integrate with CI to enforce coverage thresholds.

### TDD practices
- Test-Driven Development: write tests before code.
- Red-Green-Refactor cycle.
- Ensures testability and design clarity.
- Encourages small, focused units.
- Facilitates refactoring with confidence.

## Networking & APIs
### HTTP in Python
- Standard library `http.client` for low-level HTTP.
- `urllib.request` for opening URLs.
- More convenient: `requests` library (third-party).
- Handle methods, headers, cookies, sessions.
- Understand status codes, content types, and timeouts.

### Requests library
- Popular third-party library for HTTP requests.
- Simple API: `requests.get(url)`, `post()`, etc.
- Handles parameters, headers, authentication, SSL verification.
- Sessions for persistent connections.
- Returns `Response` object with status, headers, content.

### REST API clients
- Interact with RESTful services using HTTP methods.
- Typically use `requests` to send JSON payloads.
- Handle authentication (API keys, OAuth).
- Parse JSON responses into Python dicts.
- Consider using `httpx` for async HTTP.

### Web scraping basics
- Extract data from websites.
- Tools: `requests` + `BeautifulSoup` (parsing HTML).
- `lxml` for faster parsing.
- Respect `robots.txt` and website terms.
- Handle dynamic content with `selenium` or `playwright`.

### Sockets
- Low-level network communication.
- `socket` module provides BSD socket interface.
- Create TCP/UDP clients and servers.
- Blocking vs non-blocking; select, poll, epoll.
- Typically use higher-level libraries for most applications.

### Async networking
- Asyncio provides `asyncio.open_connection`, `asyncio.start_server`.
- `aiohttp` for async HTTP client/server.
- `httpx` supports async.
- Handles many concurrent connections efficiently.
- Requires async/await pattern.

## Databases
### SQLite usage
- Lightweight, file-based SQL database.
- `sqlite3` module in standard library.
- No separate server; database is a file.
- Supports most SQL features; good for embedded/development.
- Use parameterized queries to avoid SQL injection.

### ORM basics
- Object-Relational Mapping: map Python classes to database tables.
- Abstracts SQL, provides Pythonic API.
- Examples: SQLAlchemy ORM, Django ORM, Peewee.
- Handles relationships, migrations, and query generation.
- Can introduce overhead; use raw SQL when needed.

### SQLAlchemy
- Most popular Python ORM and SQL toolkit.
- Two main components: Core (SQL abstraction) and ORM.
- Provides powerful query API, session management, and connection pooling.
- Supports multiple database backends.
- Advanced features: eager loading, inheritance, events.

### Transactions
- Group multiple operations into atomic unit.
- Commit saves changes; rollback undoes.
- In SQLAlchemy, session manages transactions.
- Use context managers or explicit `begin()`, `commit()`, `rollback()`.
- Ensure ACID properties (Atomicity, Consistency, Isolation, Durability).

### Connection pooling
- Reuse database connections to reduce overhead.
- SQLAlchemy has built-in pooling.
- `QueuePool` (default) with size limits.
- Prevents exhausting database connections under load.
- Monitor and tune based on application needs.

### NoSQL integration
- Python drivers for various NoSQL databases: MongoDB (`pymongo`), Redis (`redis-py`), Cassandra, etc.
- Often document or key-value stores.
- Schema flexibility, horizontal scaling.
- Use appropriate libraries and connection patterns.
- Consider async drivers for high concurrency.

## Web Frameworks (Advanced Python)
### Flask architecture
- Micro-framework with minimal core.
- Built on Werkzeug (WSGI) and Jinja2 (templating).
- Uses decorators for routing.
- Extensions add functionality (e.g., Flask-SQLAlchemy).
- Lightweight, flexible; good for small to medium apps.

### Django architecture
- Full-stack framework with "batteries included".
- MTV pattern (Model-Template-View).
- Includes ORM, admin, authentication, forms, etc.
- Tightly integrated; follows convention over configuration.
- Suitable for large, complex applications.

### FastAPI
- Modern, fast web framework for building APIs.
- Based on Starlette (ASGI) and Pydantic (data validation).
- Automatic OpenAPI docs (Swagger UI).
- Supports async natively.
- Great for high-performance APIs with automatic request validation.

### ASGI vs WSGI
- WSGI (Web Server Gateway Interface) is synchronous, traditional.
- ASGI (Asynchronous Server Gateway Interface) supports async, WebSockets, HTTP/2.
- ASGI apps can handle long-lived connections.
- Frameworks: FastAPI (ASGI), Django (WSGI, with ASGI support).
- Servers: Gunicorn (WSGI), Uvicorn (ASGI), Daphne.

### REST API design
- Use HTTP methods: GET, POST, PUT, DELETE.
- Resource-oriented URLs.
- Stateless; use tokens (JWT) or sessions.
- Versioning: URL path or headers.
- Return appropriate status codes and consistent error formats.

### Middleware
- Components that process requests/responses globally.
- In Flask: `before_request`, `after_request` decorators.
- In Django: middleware classes.
- In FastAPI: ASGI middleware or `@app.middleware`.
- Used for logging, authentication, CORS, compression.

### Authentication
- Common methods: session-based, token-based (JWT), OAuth2.
- Flask: Flask-Login, Flask-JWT.
- Django: built-in auth system, Django REST Framework (DRF) auth.
- FastAPI: OAuth2 with JWT, dependency injection.
- Secure password storage: hashing (bcrypt, argon2).

### ORM integration
- Web frameworks often integrate ORMs.
- Django has own ORM; Flask works with SQLAlchemy.
- FastAPI often uses SQLAlchemy with async support (databases, SQLModel).
- ORM simplifies database interactions but may need tuning.
- Migrations: Alembic (SQLAlchemy), Django migrations.

## Security
### Secure coding practices
- Validate all input; never trust user data.
- Use parameterized queries to avoid SQL injection.
- Escape output to prevent XSS.
- Keep dependencies updated.
- Follow principle of least privilege.

### Encryption basics
- Symmetric encryption (same key): AES.
- Asymmetric encryption (public/private): RSA.
- Python libraries: `cryptography`, `PyCrypto` (legacy), `pyca/cryptography`.
- Use for data at rest or in transit.
- Never roll your own crypto.

### Hashing
- One-way functions for passwords and data integrity.
- Use secure hashing algorithms: SHA-256, SHA-3.
- For passwords: use bcrypt, Argon2, or PBKDF2 with salt.
- Python's `hashlib` provides common hash functions.
- Avoid MD5 and SHA-1 for security.

### Secrets management
- Never hardcode secrets in code.
- Use environment variables, `.env` files, or dedicated secrets managers (HashiCorp Vault, AWS Secrets Manager).
- In production, inject secrets via CI/CD or orchestration.
- Rotate secrets regularly.
- Tools like `python-decouple`, `django-environ` help.

### Input validation
- Validate data type, length, range, format.
- Use libraries like `pydantic` or `marshmallow` for complex validation.
- In web apps, framework validators (WTForms, Django forms).
- Sanitize inputs to prevent injection attacks.
- Always validate on server side (client-side is convenience).

### Authentication patterns
- Session-based: server stores session, client sends cookie.
- Token-based: client sends token (JWT) in header; stateless.
- OAuth2: delegated authorization (e.g., "Login with Google").
- Multi-factor authentication (MFA) adds extra layer.
- Implement rate limiting to prevent brute-force.

## Python Packaging & Deployment
### Virtual environments
- Already covered in Modules & Packaging.
- Essential for isolating dependencies per project.
- Use `venv` or `virtualenv`.
- `pip freeze > requirements.txt` to capture dependencies.
- Consider `pipenv` or `poetry` for more robust dependency management.

### Dependency management
- `requirements.txt` for simple projects.
- `pipenv` combines `pip` and `virtualenv`, uses `Pipfile`.
- `poetry` offers modern dependency resolution, packaging, and publishing.
- Lock files (`Pipfile.lock`, `poetry.lock`) ensure reproducible installs.
- Use constraints files for complex environments.

### Docker with Python
- Containerize Python applications for consistent deployment.
- Write a `Dockerfile` with base image (e.g., `python:3.9-slim`).
- Copy code, install dependencies, set entrypoint.
- Multi-stage builds to reduce image size.
- Use `.dockerignore` to exclude unnecessary files.

### CI/CD pipelines
- Automate testing, building, and deployment.
- Tools: GitHub Actions, GitLab CI, Jenkins, CircleCI.
- Steps: checkout, set up Python, install deps, run tests, build, deploy.
- Integrate code quality checks (linters, type checkers).
- Automate version bumping and package publishing.

### Packaging for production
- Create distributable packages (wheel, sdist) for internal or public use.
- Ensure all dependencies are declared.
- Include entry points for console scripts.
- Test package installation in clean environment.
- Use `twine` to upload to PyPI.

### Distribution strategies
- PyPI for public packages.
- Private PyPI servers (e.g., devpi, Artifactory) for internal.
- Docker images for service deployment.
- Standalone executables (PyInstaller) for end-user tools.
- Consider serverless deployment (AWS Lambda) for functions.

## Data Engineering & ML Ecosystem (Python Advanced)
### NumPy fundamentals
- Core library for numerical computing in Python.
- Provides N-dimensional array object (`ndarray`).
- Vectorized operations, broadcasting.
- Linear algebra, random number generation, Fourier transforms.
- Written in C; performance critical.

### Pandas internals
- Built on NumPy for data manipulation.
- `Series` (1D labeled) and `DataFrame` (2D labeled).
- Indexing, grouping, merging, reshaping.
- Handles missing data, time series.
- Many operations vectorized; careful with large data.

### Data pipelines
- Series of steps to extract, transform, load (ETL) data.
- Tools: Python scripts, Apache Airflow, Luigi, Prefect.
- Can use pandas for in-memory transformations.
- For large scale, use Spark or Dask.
- Ensure idempotency and error handling.

### Spark with Python
- Apache Spark is distributed computing framework.
- PySpark API allows Python usage.
- RDDs, DataFrames, SQL, streaming.
- In-memory processing, fault-tolerant.
- Good for large-scale data processing.

### ETL design
- Extract from sources (databases, APIs, files).
- Transform: clean, enrich, aggregate.
- Load into target (data warehouse, database).
- Consider incremental vs full loads.
- Monitor data quality and lineage.

### Async pipelines
- Use asyncio for I/O-bound pipeline stages.
- Libraries: `aiofiles`, `aiohttp`, `aiosqlite`.
- Can speed up data fetching and writing.
- Be careful with CPU-bound steps (offload to threads/processes).
- Frameworks like `faust` for stream processing.

### ML ecosystem overview
- scikit-learn for classical ML (preprocessing, models, metrics).
- TensorFlow, PyTorch for deep learning.
- XGBoost, LightGBM for gradient boosting.
- Jupyter for interactive exploration.
- MLOps: model serving (MLflow, Kubeflow), monitoring.

## Real-World Scenarios
### Debugging production issues
- Use logging with appropriate levels.
- Structured logging for searchability.
- Collect metrics and traces.
- Reproduce issues in staging if possible.
- Use debuggers (pdb) in development; remote debugging tools (e.g., py-spy) in production.

### Memory leaks
- Objects not released due to lingering references.
- Use `tracemalloc` to compare snapshots.
- Check for circular references with `gc` module.
- Common culprits: caches, global variables, unclosed resources.
- Profile memory usage over time.

### Performance bottlenecks
- Profile CPU with cProfile; memory with tracemalloc.
- Identify slow functions, loops, or I/O.
- Optimize algorithms, use caching, reduce overhead.
- Consider concurrency/parallelism.
- Monitor response times and throughput.

### Scaling Python services
- Horizontal scaling: run multiple instances behind load balancer.
- Use stateless design (session externalized).
- Database connection pooling and read replicas.
- Cache frequently accessed data (Redis, Memcached).
- Asynchronous processing for background tasks (Celery, RQ).

### Concurrency design
- Choose threading, multiprocessing, asyncio based on workload.
- Avoid shared state; prefer message passing.
- Use queues for work distribution.
- Design for idempotency and retries.
- Test under load to uncover race conditions.

### Production monitoring
- Collect metrics (CPU, memory, request rate, error rate).
- Use tools: Prometheus, Grafana, Datadog, New Relic.
- Set up alerts for anomalies.
- Distributed tracing (Jaeger, Zipkin) to debug request flows.
- Log aggregation (ELK stack, Loki).

### Error handling strategies
- Graceful degradation: return fallback values.
- Retry with exponential backoff for transient errors.
- Circuit breaker pattern to avoid cascading failures.
- Centralized error reporting (Sentry).
- Differentiate between expected and unexpected errors.

### Refactoring legacy Python
- Understand existing code (tests, documentation).
- Incremental changes; keep system working.
- Add tests before refactoring.
- Use tools like `pylint`, `mypy` to catch issues.
- Modernize syntax (e.g., f-strings, pathlib) while maintaining compatibility.

## Advanced Python (from second list)
### Object model internals
- CPython objects: `PyObject` header with refcount and type pointer.
- Type objects contain metadata and method tables.
- Instance objects contain `__dict__` (unless `__slots__`).
- Attribute lookup: instance dict, class dict, base classes (MRO).
- All objects are reference-counted; GC handles cycles.

### Classes and metaclasses
- Classes are instances of metaclass (default `type`).
- Metaclasses can intercept class creation (e.g., for validation, registration).
- `__new__` of metaclass creates class; `__init__` initializes it.
- Common use: ORMs (declarative base), singleton pattern.
- Metaclass conflicts in multiple inheritance require careful resolution.

### Multiple inheritance
- Already covered; here deeper: MRO C3 algorithm ensures monotonicity.
- Diamond shape: A -> B, C -> D; MRO: D, B, C, A.
- `super()` uses MRO, not just parent.
- Mixin classes provide reusable functionality.
- Avoid complex hierarchies; prefer composition.

### MRO (method resolution order)
- Detailed: Python uses C3 linearization.
- Properties: local precedence ordering and monotonicity.
- Accessible via `ClassName.__mro__`.
- Important for understanding method lookup in multiple inheritance.
- `super()` follows MRO.

### Descriptors
- Data descriptor: defines `__set__` or `__delete__`; takes precedence over instance dict.
- Non-data descriptor: only `__get__`; instance dict overrides.
- Used for properties, staticmethod, classmethod.
- `__get__` receives instance, owner, returns value.
- Descriptor protocol powers much of Python's attribute magic.

### Decorators
- Deep dive: decorators can be classes implementing `__call__`.
- `functools.wraps` copies metadata.
- Stacked decorators apply from bottom up.
- Common patterns: logging, timing, memoization, authentication.
- Can accept arguments via outer function.

### Closures
- Closure remembers free variables even after outer function exits.
- Implemented via function objects with `__closure__` attribute.
- Useful for factory functions, callbacks, and data hiding.
- Beware of late binding in loops (use default argument to capture current value).
- Closures can cause memory leaks if they hold large objects.

### Generators and coroutines
- Generators as coroutines: `.send()`, `.throw()`, `.close()`.
- `yield from` delegates to subgenerator, allowing bidirectional communication.
- Coroutines can be used for cooperative multitasking.
- `async/await` replaced many use cases, but generators still relevant.
- Generator-based coroutines are the basis of `asyncio`'s old-style coroutines.

### Async/await model
- `async` functions are coroutines; `await` suspends until awaitable completes.
- Awaitables: coroutines, tasks, futures, objects with `__await__`.
- Event loop schedules tasks; cooperative multitasking.
- Concurrency is not parallelism; single thread.
- Async I/O uses non-blocking sockets and callbacks.

### Context managers
- Implemented with `__enter__` and `__exit__`.
- `__exit__` receives exception info; can suppress exception by returning True.
- `contextlib` provides `contextmanager` decorator to turn generator into context manager.
- Also `closing()`, `suppress()`, `ExitStack` for dynamic management.
- Used for resource management, temporary settings, etc.

### Dataclasses
- Automatically generate `__init__`, `__repr__`, `__eq__`, etc.
- Fields defined with type hints; default values possible.
- Options: `frozen=True` (immutable), `order=True` (comparison methods).
- `field()` function for per-field customization (default_factory, exclude from repr).
- Post-init processing with `__post_init__`.

### Slots
- `__slots__` prevents `__dict__` creation, saving memory.
- Subclasses must also define `__slots__` to keep benefits.
- Attribute lookup slightly faster.
- Cannot add new attributes not in slots.
- Use for classes with many instances (e.g., in games, data processing).

### Weak references
- `weakref.ref(obj)` returns a reference that does not prevent garbage collection.
- `weakref.proxy(obj)` acts like object but raises `ReferenceError` if dead.
- Weak dictionaries (`WeakKeyDictionary`, `WeakValueDictionary`) for caches.
- Callbacks can be attached to weakref to be notified on object death.
- Useful in observer patterns and to break reference cycles.

### Introspection
- `dir()`, `type()`, `id()`, `isinstance()`.
- `inspect` module: get source code, signature, stack frames.
- `getattr()`, `setattr()`, `hasattr()` for dynamic attribute access.
- `callable()` checks if object can be called.
- Introspection enables frameworks like Django's admin and serializers.

### Reflection
- More dynamic: modify classes at runtime, create new types.
- `type(name, bases, dict)` creates new class.
- `importlib` to import modules dynamically.
- `exec()` and `eval()` execute code from strings (use with caution).
- Enables metaprogramming and plugin systems.

### Dynamic code execution
- `eval()` evaluates expression.
- `exec()` executes arbitrary code.
- `compile()` compiles source into code object.
- Security risk if input is untrusted; avoid or use restricted execution (e.g., `ast.literal_eval`).
- Used in REPL, debuggers, and some meta-programming.

### Abstract base classes
- Already covered; here add: `abc.ABC` class.
- `@abstractmethod` decorator; can also have concrete methods.
- `register()` method allows classes to be recognized as virtual subclasses.
- `isinstance()` and `issubclass()` work with registered classes.
- `collections.abc` provides many useful ABCs.

### Protocols and typing
- `typing.Protocol` for structural subtyping (Python 3.8+).
- Define expected methods/attributes; classes that match are considered subtypes.
- Static type checkers (mypy) enforce protocol conformance.
- Useful for duck typing with static safety.
- Examples: `typing.Iterable`, `typing.Sized`.

### C extensions
- Write modules in C using Python/C API.
- `PyModuleDef`, `PyMethodDef` define module and functions.
- Handle reference counting, error checking.
- Can release GIL for long operations.
- Use `setuptools` to compile and distribute.

### Python/C API
- C interface to Python internals.
- Functions: `PyObject_GetItem`, `PyList_SetItem`, etc.
- Type definitions: `PyTypeObject`, `PyMethodDef`.
- Must manage reference counts (`Py_INCREF`, `Py_DECREF`).
- Can embed Python in C applications.

### Bytecode fundamentals
- Python source compiled to bytecode, stored in `.pyc` files.
- Bytecode is executed by interpreter loop (ceval.c in CPython).
- `dis` module disassembles functions/code.
- Understanding bytecode helps in optimization and debugging.
- Bytecode is version-specific; not guaranteed stable across releases.

## Python Performance (from second list)
### Performance profiling
- Use `cProfile` to get function call stats.
- `profile` module for pure Python profiling (slower).
- `line_profiler` (third-party) for line-by-line timings.
- `py-spy` for sampling profiler without modifying code.
- Analyze results with `pstats` or visualization tools.

### CPU profiling
- Focuses on time spent in CPU-bound code.
- Identify hot spots using profilers.
- Optimize algorithms, use built-in C functions, vectorize.
- Consider Cython, Numba for numeric code.
- Profile with realistic workloads.

### Memory profiling
- `tracemalloc` tracks allocations with stack traces.
- `memory_profiler` decorator for function-level memory usage.
- `objgraph` shows object graphs and references.
- Identify leaks, excessive allocations, and inefficient data structures.
- Monitor memory usage over time.

### Time complexity analysis
- Understand algorithmic complexity for Python operations.
- Common operations: list index O(1), list append O(1) amortized, dict get O(1) average.
- Nested loops increase complexity.
- Choose appropriate data structures (set for membership, deque for queue).
- Profile to confirm bottlenecks.

### GIL impact
- CPU-bound threads serialized by GIL.
- Use multiprocessing for true parallelism.
- In I/O-bound, GIL is released during blocking syscalls.
- C extensions can release GIL.
- GIL removal is unlikely; work around it.

### Multiprocessing optimization
- Use `Pool` with appropriate chunk size.
- Minimize data transfer; use shared memory when possible.
- Consider `concurrent.futures.ProcessPoolExecutor`.
- Overhead of process creation; reuse pool.
- For large data, consider `multiprocessing.Array` or `Manager`.

### Threading tradeoffs
- Lightweight, shared memory, but GIL limits CPU.
- Good for I/O-bound tasks (network, disk).
- Need locks for shared data.
- Debugging harder due to non-determinism.
- Overhead of context switching.

### Async performance tuning
- Use asyncio for many concurrent I/O connections.
- Avoid blocking calls; use `await` on async libraries.
- Limit concurrency with semaphores.
- Use `asyncio.run()` for main entry.
- Profile with `asyncio` debug mode.

### Vectorization strategies
- Use NumPy to replace loops with array operations.
- Broadcasting and universal functions.
- Pandas operations are vectorized where possible.
- NumExpr for speeding up complex expressions.
- Vectorization leverages optimized C/Fortran libraries.

### Caching techniques
- `functools.lru_cache` for function result caching.
- `cachetools` for more policies (TTL, LFU, etc.).
- Use `functools.singledispatch` with caching.
- For web apps, use Redis/Memcached for external caching.
- Cache invalidation is hard; design carefully.

### Memoization
- Special case of caching for deterministic functions.
- `lru_cache` works for functions with hashable arguments.
- Can be implemented manually with dict.
- Reduces repeated computation.
- Trade-off memory for speed.

### Lazy evaluation
- Delaying computation until needed.
- Generators and `itertools` provide lazy sequences.
- Useful for large data or infinite sequences.
- Can reduce memory footprint.
- May complicate debugging.

### Memory allocation behavior
- CPython's allocator for small objects (arenas, pools).
- Large objects use `malloc`.
- Frequent allocations can cause fragmentation.
- Use `__slots__` to reduce per-instance memory.
- Monitor with `tracemalloc`.

### Garbage collection tuning
- `gc` module thresholds: `gc.get_threshold()`, `gc.set_threshold()`.
- `gc.set_debug()` for logging.
- For long-running apps, manual collection may help.
- Disable GC temporarily for performance-critical sections.
- Understand generational collection (young, old).

### Data structure efficiency
- List: dynamic array, O(1) index, O(n) insert/delete.
- Deque: O(1) append/pop both ends.
- Dict: hash table, O(1) average lookup.
- Set: hash table, O(1) average membership.
- Heapq: priority queue using list, O(log n) push/pop.

### NumPy acceleration
- NumPy arrays store homogeneous data contiguously.
- Operations execute in C, bypassing Python overhead.
- Broadcasting enables element-wise operations without loops.
- Use `np.vectorize` for custom functions (still slower than native).
- Leverage BLAS/LAPACK for linear algebra.

### Cython optimization
- Compile Python to C; add static types for speed.
- Use `cdef` for C variables, `@cython.boundscheck(False)` for performance.
- Can call C functions directly.
- Releases GIL with `with nogil:`.
- Integrates with setuptools.

### Numba optimization
- JIT compile functions with `@jit`.
- Nopython mode for maximum speed (if supported).
- Works best with NumPy.
- Can target CUDA GPUs.
- Easy to use; no separate compile step.

### I/O performance
- Buffered I/O; use large chunks.
- For disk, consider memory-mapped files (`mmap`).
- For network, use async or threading to overlap.
- Avoid many small reads/writes.
- Use `io.BytesIO` for in-memory streams.

### Benchmarking techniques
- Use `timeit` for micro-benchmarks.
- `pytest-benchmark` for integration.
- Run multiple iterations to average noise.
- Compare alternatives; measure under realistic conditions.
- Beware of JIT warm-up, caching effects.

## Python Instrumentation (from second list)
### Runtime instrumentation
- Insert probes to measure performance, behavior.
- Use `sys.settrace` for tracing function calls.
- `sys.setprofile` for profiling.
- Can modify bytecode (e.g., for coverage).
- Tools like `py-spy` use sampling.

### sys.settrace
- Set a trace function for each line executed.
- Used in debuggers, coverage tools.
- Receives frame, event, arg.
- Events: 'call', 'line', 'return', 'exception', etc.
- Significant overhead; use with care.

### Profiling hooks
- `sys.setprofile` similar to trace but for profiler events ('call', 'return').
- `cProfile` uses this internally.
- Can implement custom profilers.
- Overhead high but detailed.

### Debugging hooks
- `sys.settrace` enables step-by-step debugging.
- `pdb` uses it.
- Can also use `breakpoint()` (Python 3.7+).
- Remote debugging with `rpdb` or `web-pdb`.

### Bytecode inspection
- `dis` module disassembles code objects.
- `opcode` module defines bytecode instructions.
- Can analyze or modify bytecode for instrumentation.
- Advanced tools like `bytecode` library.

### Code injection
- Dynamically modify functions or methods.
- Use `types.FunctionType` to create new functions from code.
- Monkey patching (already covered).
- Can replace bytecode (dangerous).
- Used in testing (mock) and hot patching.

### Monkey patching
- Covered earlier; adding: use `unittest.mock.patch` for tests.
- In production, patch with caution; document.
- Can lead to maintenance issues.

### Decorator-based instrumentation
- Use decorators to wrap functions with logging, timing, etc.
- Preserve metadata with `functools.wraps`.
- Can stack decorators.
- Easy to apply selectively.

### AOP-style instrumentation
- Aspect-Oriented Programming: separate cross-cutting concerns.
- In Python, can be done with decorators, metaclasses, or bytecode weaving.
- Libraries like `wrapt` for robust decorators.
- Not as common as in Java.

### Tracing frameworks
- OpenTelemetry, OpenTracing (now merged) provide APIs.
- Instrument code to generate spans, traces.
- Collect and export to backends (Jaeger, Zipkin).
- Automatic instrumentation available for popular libraries.
- Helps in distributed systems.

### Custom profilers
- Build profilers using `sys.setprofile` or sampling.
- Sample profilers (`py-spy`) run in separate process, low overhead.
- Deterministic profilers record every call.
- Can focus on specific parts of code.

### Coverage instrumentation
- `coverage.py` uses trace function to record executed lines.
- Also branch coverage.
- Integrates with pytest.
- Reports show untested code.

### Import hooks
- `sys.meta_path` and `sys.path_hooks` allow custom importers.
- Can modify modules at import time.
- Used for automatic instrumentation, lazy loading, and virtual environments.
- Example: `pytest` import mechanism.

### AST transformations
- Parse code to Abstract Syntax Tree (AST), modify, compile back.
- `ast` module provides tools.
- Can add instrumentation, optimize, or enforce rules.
- Used by `pytest` for assertion rewriting, `mypy` for type checking.
- Complex but powerful.

### Runtime patching
- Replace functions or methods at runtime.
- Can be done with assignment: `obj.method = new_method`.
- Use `unittest.mock.patch` for testing.
- In production, use sparingly.

### Extension instrumentation
- For C extensions, need separate tools.
- Can use `perf`, `valgrind` for profiling C code.
- Python-level profilers see C function calls only if they release GIL.
- Use `py-spy` to see Python stack even in C extensions.

## Python Observability (from second list)
### Observability principles
- Ability to understand internal state from external outputs.
- Three pillars: logs, metrics, traces.
- Helps in debugging, monitoring, and performance analysis.
- Design systems to be observable: structured logs, metric endpoints, tracing IDs.
- Tools collect and correlate data.

### Structured logging
- Log in machine-readable format (JSON).
- Each log entry has keys/values for easy querying.
- Python libraries: `python-json-logger`, `structlog`.
- Integrates with log aggregation systems.
- Enables filtering and alerting.

### Logging frameworks
- Standard library `logging` can be configured for structured output.
- `loguru` offers simpler, more powerful logging.
- `structlog` adds structured logging on top of `logging`.
- Choose based on needs: flexibility, performance, ease.

### Metrics collection
- Numerical measurements over time (counters, gauges, histograms).
- `prometheus_client` for exposing metrics in Prometheus format.
- `statsd` client for sending metrics to aggregator.
- Measure request rates, error rates, latencies.
- Use for alerting and capacity planning.

### Prometheus integration
- Expose metrics HTTP endpoint (`/metrics`).
- Prometheus scrapes periodically.
- Use client library to define metrics.
- Grafana for visualization.
- Alertmanager for alerts.

### OpenTelemetry Python
- OpenTelemetry provides unified APIs for traces and metrics.
- Instrumentation libraries for frameworks (Flask, Django, etc.).
- Export to various backends (Jaeger, Prometheus, etc.).
- Context propagation across services.
- Future of observability standards.

### Distributed tracing
- Track requests across multiple services.
- Each request has trace ID; spans represent units of work.
- Tools: Jaeger, Zipkin, AWS X-Ray.
- Helps identify latency bottlenecks.
- Requires propagation headers (e.g., W3C Trace-Context).

### Log aggregation
- Centralize logs from all services.
- ELK stack (Elasticsearch, Logstash, Kibana) or Loki.
- Search, visualize, and set alerts.
- Use structured logs for efficient querying.
- Correlate with traces via IDs.

### Monitoring pipelines
- Collect metrics, logs, traces.
- Process and store.
- Dashboards for visualization.
- Alerting on thresholds.
- Use tools like Grafana, Prometheus, Thanos.

### Health checks
- Endpoints (e.g., `/health`) to indicate service status.
- Return 200 if healthy, 5xx if not.
- Check dependencies (DB, cache) and internal state.
- Used by load balancers and orchestration (Kubernetes liveness/readiness).
- Lightweight and fast.

### Alerting strategies
- Define SLOs and error budgets.
- Alert on symptoms, not causes.
- Use thresholds with hysteresis to avoid flapping.
- Route alerts to appropriate teams.
- Continuously tune to reduce noise.

### Debugging production issues
- Use logs, metrics, traces.
- Reproduce in staging if possible.
- Canary deployments to test changes.
- Feature flags to toggle problematic code.
- Post-mortems to learn.

### Memory leak detection
- Monitor memory usage over time.
- Take heap snapshots with `tracemalloc`.
- Compare snapshots to identify growing objects.
- Use `objgraph` to find reference chains.
- Check for caches without size limits, unclosed resources.

### Stack trace analysis
- Collect stack traces from running processes.
- `py-spy dump` shows all thread stacks.
- Use `faulthandler` to dump on signals.
- Identify deadlocks or stuck threads.
- Correlate with logs.

### Performance monitoring
- Track response times, throughput, error rates.
- Use APM tools (New Relic, Datadog) or open-source (Prometheus).
- Profile code periodically.
- Set performance budgets.
- Monitor resource usage (CPU, memory, network).

### Application telemetry
- Collect and export telemetry data (metrics, logs, traces).
- Instrument code manually or use auto-instrumentation.
- Ensure low overhead.
- Use to understand behavior and diagnose issues.
- Telemetry data is essential for observability.

---
