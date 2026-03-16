## 1. CPython Internals & Runtime

- What is CPython? How does it differ from PyPy, Jython, IronPython, and MicroPython?
- What is the Python compilation pipeline: source → AST → bytecode → execution?
- What tool inspects Python bytecode? (`dis` module — `dis.dis(func)`)
- What is a `.pyc` file? Where is it stored (`__pycache__`)? When is it regenerated?
- What is the `co_` attributes of a code object (`co_code`, `co_consts`, `co_varnames`, `co_freevars`)?
- What is the Python stack frame (`frame` object)? What does it contain?
- What is the Global Interpreter Lock (GIL)?
- Why does the GIL exist? (CPython memory management uses reference counting — GIL prevents race conditions)
- What are the consequences of the GIL for threading? (Only one thread executes Python bytecode at a time)
- Does the GIL prevent all parallelism? (No — I/O-bound threads release the GIL; C extensions can release it)
- What is `sys.getswitchinterval()`? (Default 5ms — how often the GIL switches threads)
- What is the GIL removed in Python 3.13+ (PEP 703 — free-threaded CPython, experimental)?
- What is `sys.getrefcount(obj)`? Why does it return N+1? (Argument passing adds a temporary reference)
- What is `id(obj)`? What does it represent? (Memory address of the object in CPython)
- What is the `sys` module? Key attributes: `sys.path`, `sys.argv`, `sys.version`, `sys.modules`, `sys.stdin/stdout/stderr`
- What is `sys.intern()`? What is it used for?
- What is the `__builtins__` module?
- What is `PYTHONPATH`? How does it affect module resolution?
- What is `PYTHONDONTWRITEBYTECODE`? `PYTHONOPTIMIZE`?

> **Best Practices:**
> - Use `multiprocessing` (not `threading`) for CPU-bound parallelism — each process has its own GIL.
> - Use `threading` for I/O-bound concurrency — threads release the GIL during I/O.
> - Never rely on `id()` equality across statements — the same address may be reused after an object is GC'd.
> - Use `dis.dis()` to debug surprising behavior in hot paths.

---

## 2. Python Data Model & Object System

- What is the Python Data Model? What are dunder (magic) methods?
- What is `__init__` vs `__new__`? What does each do?
- When would you override `__new__`? (Singleton, immutable subclasses like `int`, metaclasses)
- What is `__del__`? Why is it unreliable? (Not guaranteed to be called — GC timing, circular refs)
- What is `__repr__` vs `__str__`? Which is used by `print()`? By the REPL?
- What is `__bytes__`, `__format__`, `__hash__`?
- What is the `__hash__` / `__eq__` contract? (If `a == b` then `hash(a) == hash(b)`)
- What happens to `__hash__` when you define `__eq__`? (Set to `None` automatically — object becomes unhashable)
- What is `__bool__`? What is the default truthiness of an object?
- What is `__len__`? What does `bool(obj)` check first?
- What is `__getitem__`, `__setitem__`, `__delitem__`, `__contains__`, `__len__`?
- What is `__iter__` and `__next__`?
- What is `__call__`? What makes an object callable?
- What is `__enter__` and `__exit__`? (Context manager protocol)
- What is `__get__`, `__set__`, `__delete__`? (Descriptor protocol)
- What is `__slots__`? What does it do to memory?
- What is `__class_getitem__`? (Generic alias support — `list[int]`)
- What is `__init_subclass__`? When is it called?
- What is `__set_name__`? (Called when a descriptor is assigned to a class attribute)
- What is `__missing__` in `dict` subclasses?
- What are comparison dunders: `__lt__`, `__le__`, `__gt__`, `__ge__`, `__eq__`, `__ne__`?
- What is `functools.total_ordering`?
- What are numeric dunders: `__add__`, `__radd__`, `__iadd__`, `__mul__`, `__truediv__`, `__floordiv__`, `__mod__`, `__pow__`?
- What is the difference between `__add__` and `__radd__`? (Right-side fallback)
- What is `__index__`? When is it needed?

> **Best Practices:**
> - Always define `__repr__` — it makes debugging and logging vastly easier.
> - If you define `__eq__`, always define `__hash__` too (or explicitly set `__hash__ = None`).
> - Use `__slots__` for value objects that are created in large numbers to save memory.
> - Prefer `@functools.total_ordering` over manually implementing all 6 comparison methods.

---

## 3. Variables, Names & Scoping

- What is the difference between a name and a value in Python?
- What is Python's scoping rule? What is LEGB?
  - Local → Enclosing → Global → Built-in
- What is `global` keyword? What does it do?
- What is `nonlocal` keyword? When is it needed?
- What is a free variable? What is a cell variable?
- What is variable shadowing?
- What is `del` statement? Does it delete an object?
- What is the difference between `is` and `==`?
- When does `is` return `True` for integers? (CPython caches small ints: `-5` to `256`)
- What is `None`? Is it a singleton? (Yes — `x is None` is the correct check)
- What are the truthiness rules in Python? What is falsy?
  - `False`, `None`, `0`, `0.0`, `""`, `[]`, `{}`, `set()`, `()`, objects with `__bool__` returning False or `__len__` returning 0
- What is augmented assignment (`+=`)? Does it modify in place or rebind?
- What is the difference between `a = a + [1]` and `a += [1]` for a list?
- What is unpacking? Extended unpacking (`a, *b, c = [1,2,3,4,5]`)?
- What is walrus operator `:=` (Python 3.8+)? Give a use case.

> **Best Practices:**
> - Always use `is` to compare with `None`, `True`, `False` — never `==`.
> - Avoid `global` — it makes code hard to test and reason about; prefer returning values.
> - Use `nonlocal` only when closure state mutation is genuinely needed.

---

## 4. Data Types — Built-ins

### Numbers
- What numeric types does Python have: `int`, `float`, `complex`, `bool`?
- Is Python `int` bounded? (No — arbitrary precision; no overflow)
- What is `float` precision? (`float` is a C `double` — 64-bit IEEE 754)
- What is `0.1 + 0.2 == 0.3` in Python? (`False` — floating point error)
- What is `decimal.Decimal`? When should you use it?
- What is `fractions.Fraction`?
- What is integer division `//` vs `/`?
- What is `divmod(a, b)`?
- What is `math.inf` and `math.nan`? How do you check for NaN?

### Strings
- Are strings mutable in Python? (No — immutable)
- What encoding does Python 3 use for `str`? (Unicode — internally flexible: latin-1, UCS-2, or UCS-4 depending on content)
- What is `bytes` vs `str`? When do you use each?
- What is `bytearray`? How does it differ from `bytes`?
- What is `encode()` and `decode()`?
- What is f-string (Python 3.6+)? What are format spec mini-language options?
- What is `str.format()` vs `%` formatting vs f-strings?
- What are common string methods: `split()`, `join()`, `strip()`, `replace()`, `find()`, `count()`, `startswith()`, `endswith()`, `upper()`, `lower()`, `title()`, `isdigit()`, `isalpha()`, `isspace()`?
- What is `str.translate()` and `str.maketrans()`?
- What is string interning in Python? (`sys.intern()`)

### Lists
- Is `list` mutable? What is its time complexity for `append()`, `insert()`, `pop()`, `__getitem__`?
- What is list comprehension? How does it differ from `map()`/`filter()`?
- What is `list.sort()` vs `sorted()`? What algorithm? (Timsort — O(n log n))
- What is `key=` parameter in `sort()`?
- What is `list.copy()` vs `copy.deepcopy()`?
- What is `list * n`? Is it a deep copy?
- What is `list.__contains__` time complexity? O(n)
- What is slice notation `a[start:stop:step]`? What is `a[::-1]`?
- What is `a[:]`? (Shallow copy)

### Tuples
- How does `tuple` differ from `list`? (Immutable — but can contain mutable objects)
- Is a tuple hashable? When is it not? (Only if all elements are hashable)
- What is a named tuple (`collections.namedtuple`, `typing.NamedTuple`)?
- What is the memory advantage of tuples over lists?
- What is tuple unpacking?
- What is a single-element tuple? (`(1,)` — trailing comma required)

### Dictionaries
- Is `dict` ordered in Python 3.7+? (Yes — insertion order guaranteed)
- What is the time complexity of `dict` operations (`get`, `set`, `delete`, `in`)? O(1) average
- What is a dict comprehension?
- What are `dict.keys()`, `dict.values()`, `dict.items()`? Are they views or copies?
- What is `dict.get(key, default)`?
- What is `dict.setdefault(key, default)`?
- What is `dict.update()`?
- What is `{**d1, **d2}` (dict merge)? What is `|` operator (Python 3.9+)?
- What is `collections.defaultdict`?
- What is `collections.OrderedDict`? Is it still needed in Python 3.7+?
- What is `collections.Counter`?
- What is `collections.ChainMap`?

### Sets
- What is `set`? What is `frozenset`?
- What are set operations: `|`, `&`, `-`, `^`, `<=`, `>=`?
- What is the time complexity of `in` for `set`? O(1)
- What is `set` vs `frozenset` hashability?

### None, bool, bytes
- What is `None`? What type is it?
- What is `bool`? Is it a subclass of `int`? (Yes — `True == 1`, `False == 0`)
- What is `bytes`? `bytearray`? `memoryview`?

> **Best Practices:**
> - Use `dict.get(key)` instead of `dict[key]` when key may be absent.
> - Prefer `collections.defaultdict` over `setdefault` for cleaner grouping code.
> - Use `tuple` for heterogeneous data and `list` for homogeneous sequences.
> - Use `frozenset` as a dict key or set element when set semantics are needed.

---

## 5. Operators & Expressions

- What is operator precedence in Python? What has highest priority?
- What is `//` (floor division) vs `/` (true division)?
- What is `**` (power)? What is `2**31`?
- What is `%` (modulo)? What is `-7 % 3` in Python? (`2` — Python modulo always has the sign of the divisor)
- What is `@` operator (Python 3.5+)? (Matrix multiplication — `__matmul__`)
- What are comparison chaining: `1 < x < 10`? How does Python evaluate it?
- What is `and`, `or`, `not`? What do `and`/`or` return? (The actual object, not necessarily `bool`)
- What is `x or default`? When is it dangerous? (Falsy values like `0`, `""`, `[]` trigger the default)
- What is `x if condition else y` (ternary expression)?
- What is the walrus operator `:=`? What scoping rules apply?
- What are augmented assignment operators? Do they always operate in-place?

---

## 6. Control Flow

- What is `if`/`elif`/`else`?
- What is `match`/`case` (Python 3.10+ structural pattern matching)?
- What are match patterns: literal, capture, wildcard `_`, OR `|`, sequence, mapping, class, guard `if`?
- What is a `for` loop? What does it call under the hood? (`__iter__` then `__next__`)
- What is a `while` loop?
- What is `break`, `continue`, `pass`?
- What is the `else` clause on loops and `try`? (Runs if loop completed without `break`)
- What is `range()`? Is it a list? (No — a lazy sequence object)
- What is `enumerate()`?
- What is `zip()`? Is it lazy? What is `zip_longest()`?
- What does `zip()` do when iterables have different lengths? (Stops at the shortest)
- What is `zip(*matrix)` for transposing?

> **Best Practices:**
> - Use `enumerate()` instead of `range(len(x))` — cleaner and avoids index errors.
> - Use `zip()` over manual index loops when iterating two sequences in parallel.
> - Use `match`/`case` (Python 3.10+) for complex conditional dispatch — more readable than `if`/`elif` chains.
> - The `else` clause on `for`/`while` is rarely used — document its intent clearly.

---

## 7. Functions

- What is a first-class function in Python?
- What are positional arguments, keyword arguments, default arguments?
- What are `*args` and `**kwargs`? What types are they?
- What is keyword-only argument (after `*`)? (`def f(a, *, b): ...`)
- What is positional-only argument (before `/`, Python 3.8+)? (`def f(a, b, /, c): ...`)
- What is the order of parameters? (positional-only, positional-or-keyword, `*args`, keyword-only, `**kwargs`)
- What is a mutable default argument trap? (`def f(x=[])` — the list is shared across all calls)
- What is a closure? What is a free variable in a closure?
- What is a lambda function? What are its limitations?
- What is `functools.partial()`?
- What is `functools.wraps()`? Why is it important for decorators?
- What is `functools.cache()` and `functools.lru_cache()`?
- What is `functools.reduce()`?
- What is function annotation (type hint)? What is `__annotations__`?
- What is `inspect.signature()`?
- What is a generator function? What does `yield` do?
- What is `yield from`?
- What is a coroutine? How does `async def` differ from a generator?

> **Best Practices:**
> - Never use mutable objects as default argument values — use `None` and assign inside the function.
> - Use `functools.wraps(func)` in every decorator to preserve the wrapped function's metadata.
> - Use `functools.lru_cache` for pure functions with expensive recomputation.
> - Prefer keyword arguments for functions with 3+ parameters — improves call-site readability.

---

## 8. Decorators

- What is a decorator? What does `@decorator` syntax desugar to?
- What is a decorator that takes arguments? How is it structured (factory returning decorator)?
- What is `functools.wraps`? What metadata does it preserve?
- What is a class-based decorator?
- What is a decorator that can be used with or without arguments? (Tricky — check if called with a function or arguments)
- What is `@staticmethod`? What does it do?
- What is `@classmethod`? What is `cls`?
- What is `@property`? What is a property setter/deleter?
- What is `@abstractmethod` (`abc` module)?
- What is stacking multiple decorators? In what order are they applied? (Bottom-up application, top-down execution)
- What is `@dataclass` (Python 3.7+)?
- What are `dataclass` parameters: `eq`, `order`, `frozen`, `slots`, `kw_only`?
- What is `@dataclass(frozen=True)` vs a named tuple?
- What is `@singledispatch` (`functools`)? What is it used for?
- What is `@contextmanager` (`contextlib`)?

> **Best Practices:**
> - Always use `@functools.wraps(func)` inside decorator wrappers.
> - Use `@dataclass` for data containers instead of manual `__init__`/`__repr__`/`__eq__`.
> - Use `@dataclass(frozen=True)` for value objects that should be immutable and hashable.
> - Prefer `@property` over direct field access for attributes that require validation or lazy computation.

---

## 9. Classes & OOP

### Class Fundamentals
- What is `self`? Is it special? (Just a convention — the first parameter of instance methods)
- What is `cls`? What is the difference between `self` and `cls`?
- What is instance attribute vs class attribute?
- What is Method Resolution Order (MRO)? What algorithm does Python use? (C3 linearization)
- How do you inspect MRO? (`ClassName.__mro__` or `ClassName.mro()`)
- What is multiple inheritance? What is cooperative multiple inheritance?
- What is `super()`? What does it do? How does it use MRO?
- What is `super()` in Python 3 vs Python 2 (`super(ClassName, self)`)?
- What is diamond inheritance? How does MRO resolve it?
- What is `__init_subclass__`?
- What is `type(obj)`? What does it return?
- What is `isinstance()` vs `type() ==`? Which is preferred?
- What is `issubclass()`?

### Inheritance & Polymorphism
- What is method overriding in Python?
- Can you call an overridden method from the subclass? (`super().method()`)
- What is duck typing? How does it differ from Java's static typing?
- What is EAFP (Easier to Ask Forgiveness than Permission) vs LBYL (Look Before You Leap)?
- What is `__bases__` vs `__mro__`?
- What is a mixin? What is its purpose?

### Abstract Base Classes
- What is `abc.ABC`? What is `abc.ABCMeta`?
- What is `@abstractmethod`? Can an abstract class be instantiated?
- What is virtual subclass registration (`ABC.register()`)?
- What is `__subclasshook__`?
- What are the built-in ABCs in `collections.abc`? (e.g., `Iterable`, `Iterator`, `Sequence`, `Mapping`, `MutableMapping`, `Callable`)

### Slots & Memory
- What is `__slots__`? What memory benefit does it provide?
- What does `__slots__` remove? (The per-instance `__dict__`)
- Can a class with `__slots__` have instance variables not in slots?
- What is the interaction between `__slots__` and inheritance?

### Properties & Descriptors
- What is a descriptor? What methods define the descriptor protocol?
- What is a data descriptor vs non-data descriptor? (Data has `__set__`/`__delete__`; non-data has only `__get__`)
- How does attribute lookup work? (Data descriptors > instance `__dict__` > non-data descriptors)
- What is `property` implemented as? (A data descriptor)
- What is `__get__` called with when accessed from the class vs instance?

> **Best Practices:**
> - Use `super()` (without arguments) always — it's cooperative and respects MRO.
> - Use mixins for reusable behavior — keep each mixin focused on one concern.
> - Prefer duck typing and ABCs over explicit `isinstance` checks for polymorphism.
> - Use `__slots__` for objects created in large numbers (e.g., nodes in a tree).

---

## 10. Metaclasses

- What is a metaclass? What is the metaclass of a class?
- What is the default metaclass? (`type`)
- What is `type(name, bases, namespace)`? How does it create a class?
- How do you define a custom metaclass?
- What is `__new__` vs `__init__` vs `__call__` on a metaclass?
- When is `__prepare__` called on a metaclass? What does it return?
- What is `__init_subclass__`? How is it simpler than a metaclass?
- What are common use cases for metaclasses? (ORMs, API frameworks, plugin registration, singletons)
- What is `ABCMeta`? How does it use metaclass machinery?
- What is the metaclass conflict? When does it occur?
- What is `__class_getitem__` for generic aliases?
- What is `dataclasses` implemented with? (A class decorator, not a metaclass)

> **Best Practices:**
> - Prefer `__init_subclass__` or class decorators over metaclasses for most customization — simpler and clearer.
> - Use metaclasses only when you need to intercept class creation itself.
> - Document metaclass usage thoroughly — it is surprising to readers.

---

## 11. Iterators & Generators

- What is the iterator protocol? (`__iter__` + `__next__`)
- What is `StopIteration`?
- What is the difference between an iterable and an iterator?
- What is a generator function? What does `yield` produce?
- What is generator expression? How does it differ from list comprehension?
- What is `yield from`? What does it do for sub-generators?
- What is `send()` on a generator? What is a send-value generator?
- What is `throw()` on a generator?
- What is `close()` on a generator? What exception does it raise inside? (`GeneratorExit`)
- What is `itertools.islice()`, `chain()`, `product()`, `combinations()`, `permutations()`, `groupby()`, `accumulate()`, `takewhile()`, `dropwhile()`, `cycle()`, `repeat()`, `starmap()`?
- What is `zip_longest()` from `itertools`?
- What is `more-itertools`? (third-party)
- What is a lazy evaluation? Why are generators memory-efficient?
- What is infinite generator? Give an example.
- How do you convert a generator to a list? (`list(gen)` — consumes it)
- Can you reuse a generator? (No — once exhausted, it's done)

> **Best Practices:**
> - Use generators for large or infinite sequences — they don't materialize everything in memory.
> - Prefer generator expressions over list comprehensions when you only iterate once.
> - Use `yield from` to delegate to sub-generators rather than looping manually.
> - Use `itertools` before writing custom iteration code — it's optimized and well-tested.

---

## 12. Comprehensions

- What is list comprehension? `[expr for x in iterable if cond]`
- What is dict comprehension? `{k: v for k, v in iterable}`
- What is set comprehension? `{expr for x in iterable}`
- What is generator expression? `(expr for x in iterable)` — lazy
- What is nested comprehension? When is it too complex?
- What is the scope of the loop variable in a comprehension? (Python 3: scoped to the comprehension)
- What is a walrus operator in a comprehension? (`[y := f(x), y**2, y**3]`)
- When should you prefer a loop over a comprehension?

> **Best Practices:**
> - Keep comprehensions to a single logical operation — split complex ones into loops for readability.
> - Use generator expressions when the result is consumed only once.
> - Avoid comprehensions with side effects — use explicit loops.

---

## 13. Context Managers

- What is a context manager? What protocol does it implement?
- What is `with` statement?
- What are arguments to `__exit__`? (`exc_type`, `exc_val`, `exc_tb`)
- What does returning `True` from `__exit__` do? (Suppresses the exception)
- What is `@contextmanager` from `contextlib`? How does `yield` partition setup and teardown?
- What is `contextlib.ExitStack`? When is it used?
- What is `contextlib.suppress(*exceptions)`?
- What is `contextlib.nullcontext()`?
- What is `contextlib.redirect_stdout()`?
- Can you use multiple context managers in one `with`? (`with A() as a, B() as b:`)

> **Best Practices:**
> - Always use `with` for resources (files, connections, locks) — never rely on `__del__`.
> - Use `@contextmanager` for simple context managers — cleaner than a class.
> - Use `contextlib.ExitStack` when the number of context managers is dynamic.

---

## 14. Exception Handling

- What is the exception hierarchy in Python? (`BaseException` → `Exception` → specific)
- What is `BaseException`? What subclasses are NOT under `Exception`? (`SystemExit`, `KeyboardInterrupt`, `GeneratorExit`)
- What is `Exception`? What is the base for user-defined exceptions?
- What is `try`/`except`/`else`/`finally`? When does `else` run?
- What is multi-except? `except (TypeError, ValueError) as e:`
- What is bare `except:`? Why is it bad?
- What is exception chaining: `raise NewException(...) from original`?
- What is implicit exception chaining (Python 3 — `__context__`)?
- What is `raise` (re-raise current exception)?
- What is `__cause__` vs `__context__` on an exception?
- What is `__traceback__`?
- What is `traceback` module?
- What is `warnings.warn()`? What warning categories exist?
- How do you create a custom exception hierarchy?
- What is `ExceptionGroup` (Python 3.11+)?
- What is `except*` syntax (Python 3.11+)?

> **Best Practices:**
> - Never use bare `except:` — it catches `SystemExit` and `KeyboardInterrupt`.
> - Always catch the most specific exception type.
> - Use `raise ... from original` to preserve exception chain when re-raising.
> - Create a custom exception base class per application domain.
> - Use `logging.exception()` inside `except` blocks — it includes the traceback automatically.

---

## 15. Modules, Packages & Imports

- What is a module? What is a package?
- What is `__init__.py`? Is it required in Python 3? (No — namespace packages work without it)
- What is `__all__`? How does it affect `from module import *`?
- What is `import` vs `from module import`?
- What is relative import? (`from . import foo`, `from .. import bar`)
- What is `sys.modules`? What is module caching?
- What happens when you import a module? (Execute top-level code once, cache in `sys.modules`)
- What is circular import? How do you resolve it?
- What is `importlib`?
- What is `__name__ == "__main__"` guard?
- What is `__file__`? `__spec__`? `__package__`?
- What is `pkgutil`, `importlib.resources`?
- What is a namespace package (PEP 420)?
- What is `__import__()` vs `importlib.import_module()`?

> **Best Practices:**
> - Always add the `if __name__ == "__main__":` guard to scripts used as both scripts and modules.
> - Use `__all__` to define the public API of a module — prevents accidental wildcard pollution.
> - Avoid circular imports — restructure into a shared utility module if needed.
> - Prefer `importlib.import_module()` over `__import__()` for dynamic imports.

---

## 16. Type Hints & Typing Module

- What are type hints? Are they enforced at runtime? (No — only for static analysis tools)
- What is `typing.List`, `typing.Dict`, `typing.Optional`, `typing.Union`?
- What is `list[int]` vs `typing.List[int]`? (Python 3.9+ supports built-in generics)
- What is `Optional[X]`? Is it the same as `X | None`? (Yes — Python 3.10+)
- What is `Union[X, Y]`? What is `X | Y` (Python 3.10+)?
- What is `Any`? When should you use it?
- What is `TypeVar`? What is a bound TypeVar?
- What is `Generic[T]`?
- What is `Protocol` (PEP 544)? How does it enable structural subtyping?
- What is `Callable[[ArgType], ReturnType]`?
- What is `TypedDict`?
- What is `NamedTuple` with type hints?
- What is `Final` (typing)? What does it signal?
- What is `ClassVar`?
- What is `Literal[...]`?
- What is `overload` decorator?
- What is `cast()`?
- What is `TYPE_CHECKING` guard?
- What is `ParamSpec` (Python 3.10+)?
- What is `TypeAlias` (Python 3.10+)?
- What is `Self` type (Python 3.11+)?
- What is `Unpack` and `TypeVarTuple` (variadic generics)?
- What is `mypy`? `pyright`? `pytype`?

> **Best Practices:**
> - Add type hints to all public APIs — enables IDE autocomplete, static analysis, and documentation.
> - Use `Protocol` for duck-typed interfaces instead of ABCs when inheritance is not desired.
> - Use `Optional[X]` / `X | None` explicitly — never leave implicit None returns untyped.
> - Use `TYPE_CHECKING` guard to avoid circular imports from type-only dependencies.
> - Run `mypy --strict` in CI for type-safe codebases.

---

## 17. Functional Programming in Python

- What is `map(func, iterable)`? Is it lazy?
- What is `filter(pred, iterable)`? Is it lazy?
- What is `zip()`? Is it lazy?
- What is `functools.reduce(func, iterable, initializer)`?
- What is `functools.partial(func, *args, **kwargs)`?
- What is `functools.lru_cache(maxsize=128)`? `functools.cache()`?
- What is `functools.cached_property` (Python 3.8+)?
- What is `operator` module? (`operator.itemgetter`, `operator.attrgetter`, `operator.methodcaller`)
- What is a higher-order function?
- What is function composition in Python? (Manual via lambdas or `functools.reduce`)
- What is memoization? How do you implement it?
- What is currying? How do you implement it with `partial`?
- What is `itertools.starmap(func, iterable_of_tuples)`?
- What is a pure function? Why does Python not enforce it?
- What is referential transparency?
- What is `operator.attrgetter` vs `lambda x: x.attr`?

> **Best Practices:**
> - Prefer list comprehensions over `map()`/`filter()` for simple transformations — more Pythonic.
> - Use `operator.itemgetter`/`attrgetter` as key functions — faster than equivalent lambdas.
> - Use `functools.lru_cache` on pure functions with expensive or repeated computations.

---

## 18. Concurrency — Threading

- What is the `threading` module?
- What is `Thread`? How do you create and start one?
- What is `thread.join()`? What does `daemon=True` do?
- What is `threading.Lock`? `RLock`? `Condition`? `Event`? `Semaphore`? `Barrier`?
- What is `Lock.acquire(blocking=True, timeout=-1)`?
- What is a deadlock? How do you avoid it?
- What is `threading.local()`? (Thread-local storage)
- What is `threading.Timer`?
- What is `concurrent.futures.ThreadPoolExecutor`?
- Is Python threading truly parallel? (No for CPU-bound — GIL; yes for I/O-bound)
- What is a race condition? Give a Python example.
- What is `threading.Event`? `wait()` vs `set()` vs `clear()`?
- What is `queue.Queue`? Is it thread-safe? How is it used for producer-consumer?
- What is `queue.SimpleQueue`?
- What is `queue.PriorityQueue`?

> **Best Practices:**
> - Use `ThreadPoolExecutor` instead of raw `Thread` — handles lifecycle, exceptions, and futures.
> - Use `queue.Queue` for inter-thread communication — it is thread-safe.
> - Always use context manager (`with lock:`) instead of `lock.acquire()`/`lock.release()` — avoids forgetting to release.
> - Use `threading.local()` to keep per-thread state without locks.

---

## 19. Concurrency — Multiprocessing

- What is the `multiprocessing` module?
- What is `Process`? How does it differ from `Thread`?
- What is `Pool`? What are `map()`, `starmap()`, `apply()`, `apply_async()`?
- What is `multiprocessing.Queue` vs `queue.Queue`?
- What is `multiprocessing.Pipe()`?
- What is `multiprocessing.Value` and `multiprocessing.Array`? (Shared memory primitives)
- What is `multiprocessing.Manager()`? What types does it proxy?
- What is `multiprocessing.Pool.map()` vs `multiprocessing.Pool.imap()`? (Eager vs lazy)
- What is the `spawn` vs `fork` vs `forkserver` start method?
- Why is `fork` dangerous on macOS/Python 3.8+ with threads? (Can deadlock in forked child)
- What is `concurrent.futures.ProcessPoolExecutor`?
- What is pickling? Why must arguments and return values be picklable?
- What cannot be pickled? (Lambdas, nested functions, file objects, locks)
- What is `multiprocessing.shared_memory` (Python 3.8+)?

> **Best Practices:**
> - Use `ProcessPoolExecutor` instead of raw `Process` — cleaner API, handles futures.
> - On macOS/Windows, explicitly set `multiprocessing.set_start_method("spawn")`.
> - Avoid sharing mutable state between processes — use queues or pipes for communication.
> - Keep task functions at module level — they must be picklable.

---

## 20. Concurrency — asyncio

- What is `asyncio`? What problem does it solve?
- What is an event loop? What does it do?
- What is a coroutine? How do you define one? (`async def`)
- What is `await`? What can you `await`?
- What is `asyncio.run()` (Python 3.7+)?
- What is `asyncio.create_task()`? How does it differ from `await coroutine`?
- What is `asyncio.gather(*coros)`? What is `asyncio.wait(tasks)`?
- What is `asyncio.sleep(n)`?
- What is a `Future`? How does it relate to `Task`?
- What is `asyncio.Queue`, `asyncio.Lock`, `asyncio.Event`, `asyncio.Semaphore`?
- What is `async for`? What is an async iterable?
- What is `async with`? What is an async context manager?
- What is `asyncio.shield()`?
- What is `asyncio.timeout()` (Python 3.11+)?
- What is `asyncio.to_thread()` (Python 3.9+)? When is it used?
- What is the difference between `asyncio.gather()` and `asyncio.TaskGroup` (Python 3.11+)?
- What is `TaskGroup`? What is structured concurrency?
- What is `asyncio.StreamReader`/`StreamWriter`?
- What is the difference between `asyncio`, `trio`, and `anyio`?
- What is `uvloop`? Why is it faster than the default event loop?

> **Best Practices:**
> - Use `asyncio.run()` as the single entry point — never call `loop.run_until_complete()` in new code.
> - Use `asyncio.TaskGroup` (Python 3.11+) for structured concurrency — cancels all tasks on first failure.
> - Use `asyncio.to_thread()` to run blocking I/O or CPU-bound code without blocking the event loop.
> - Never use blocking calls (`time.sleep`, `requests.get`) inside a coroutine — use async equivalents.
> - Use `asyncio.create_task()` to start work concurrently — bare `await` is sequential.

---

## 21. Memory Management & Garbage Collection

- What is Python's primary memory management mechanism? (Reference counting)
- What is `sys.getrefcount(obj)`?
- What happens when an object's reference count drops to 0? (Immediately deallocated)
- What is a reference cycle? Give an example.
- What is the cyclic garbage collector (`gc` module)?
- What is `gc.collect()`? When does automatic collection run?
- What are the three GC generations in CPython?
- What is `gc.disable()`? When would you disable GC? (CPython performance tuning)
- What is `weakref.ref()`? `weakref.WeakValueDictionary`? `weakref.WeakSet`?
- What is `__del__`? Why should it not be relied upon?
- What is a memory leak in Python? Give common patterns.
  - Global caches growing unbounded
  - Circular references with `__del__`
  - Event listeners / callbacks not removed
  - Unclosed file handles / sockets
- What is `tracemalloc`? How do you use it to find memory leaks?
- What is `objgraph`? (Third-party memory profiling)
- What is `sys.getsizeof()`? Does it measure deep size?
- What is `__slots__` memory saving?
- What is CPython small object allocator (`pymalloc`)?
- What is `ctypes.memmove`? When would you use off-heap memory?

> **Best Practices:**
> - Use `weakref` for caches and observer registries to avoid memory leaks.
> - Use `contextlib.suppress` or `try/finally` instead of `__del__` for cleanup.
> - Profile memory with `tracemalloc` before and after suspected leak scenarios.
> - Use `__slots__` in high-volume object classes to reduce per-instance overhead.

---

## 22. File I/O & Path Operations

### File I/O
- What is `open(file, mode, encoding, buffering)`?
- What are file modes: `r`, `w`, `a`, `x`, `b`, `t`, `+`?
- What is `with open(...) as f:`? What does `__exit__` do?
- What is `f.read()`, `f.readline()`, `f.readlines()`, iterating over file?
- What is `f.write()`, `f.writelines()`?
- What is `f.seek()`, `f.tell()`?
- What is `f.flush()`? `f.fileno()`?
- What is text mode vs binary mode?
- What is the default encoding? How to set it explicitly? (`encoding="utf-8"`)
- What is `io.StringIO`? `io.BytesIO`? When are they used?
- What is `BufferedReader`, `BufferedWriter`?

### pathlib
- What is `pathlib.Path`? How does it differ from `os.path`?
- What are `Path` operations: `/` operator, `.name`, `.stem`, `.suffix`, `.parent`, `.parts`?
- What is `Path.read_text()`, `write_text()`, `read_bytes()`, `write_bytes()`?
- What is `Path.exists()`, `is_file()`, `is_dir()`?
- What is `Path.glob()`, `rglob()`?
- What is `Path.mkdir(parents=True, exist_ok=True)`?
- What is `Path.rename()`, `unlink()`, `rmdir()`?
- What is `Path.stat()`?

### os & shutil
- What is `os.getcwd()`, `os.chdir()`, `os.listdir()`, `os.walk()`?
- What is `os.environ`?
- What is `os.path.join()`, `basename()`, `dirname()`, `splitext()`?
- What is `shutil.copy()`, `copytree()`, `rmtree()`, `move()`?
- What is `tempfile.TemporaryFile()`, `NamedTemporaryFile()`, `TemporaryDirectory()`?

> **Best Practices:**
> - Always use `pathlib.Path` for file paths — it's cross-platform and object-oriented.
> - Always open files with an explicit `encoding` — never rely on locale-dependent defaults.
> - Always use `with open(...)` — guarantees the file is closed even on exception.
> - Use `io.StringIO`/`BytesIO` in tests to avoid touching the filesystem.

---

## 23. Collections Module

- What is `collections.namedtuple`? What does it generate?
- What is `collections.deque`? What is its time complexity for `appendleft()`, `popleft()`? O(1)
- What is `deque.maxlen`?
- What is `collections.Counter`? What are `most_common()`, `+`, `-`, `&`, `|`?
- What is `collections.defaultdict`? How is the factory function used?
- What is `collections.OrderedDict`? What is `move_to_end()`?
- What is `collections.ChainMap`? How does lookup work?
- What is `collections.UserDict`, `UserList`, `UserString`? Why subclass these?
- What is `heapq`? `heapq.heappush()`, `heappop()`, `nlargest()`, `nsmallest()`, `heapify()`?
- What is `heapq` heap type? (Min-heap)
- What is `bisect.bisect_left()`, `bisect_right()`, `insort()`?
- What is `array.array`? How does it differ from `list`?
- What is `struct` module?

> **Best Practices:**
> - Use `deque` for queues and stacks — O(1) on both ends; `list.pop(0)` is O(n).
> - Use `Counter` for frequency analysis — far simpler than manual dict accumulation.
> - Use `heapq` for priority queues — it works in-place on a list.
> - Use `defaultdict(list)` for grouping — cleaner than `setdefault`.

---

## 24. Python Data Model — Advanced

- What is attribute lookup order? (Instance `__dict__` → class `__dict__` → base class chain, with descriptor priority)
- What is `__getattr__` vs `__getattribute__`? What is the difference?
- What is `__setattr__`? How do you avoid infinite recursion in it?
- What is `object.__setattr__(self, name, value)` idiom?
- What is `vars(obj)`? What does it return?
- What is `dir(obj)`?
- What is `__dict__` on a class vs an instance?
- What is `__class__`?
- What is `__subclasses__()`?
- What is `__bases__` vs `__mro__`?
- What is `__module__` on a class?
- What is `__qualname__`?
- What is `__doc__`?
- What is `pickle` protocol? What dunders does it use? (`__getstate__`, `__setstate__`, `__reduce__`, `__reduce_ex__`)
- What is `copy.copy()` vs `copy.deepcopy()`? What dunders do they use? (`__copy__`, `__deepcopy__`)

---

## 25. Descriptors — Deep Dive

- What is a descriptor? Give a real-world use case.
- What is a data descriptor? (Has both `__get__` and `__set__`)
- What is a non-data descriptor? (Only `__get__`)
- What is the descriptor protocol invocation order? (Data descriptors override instance `__dict__`; non-data do not)
- How is `property` a data descriptor?
- How are methods descriptors? (Functions implement `__get__` — returns bound method)
- What is a bound method vs an unbound function?
- What is `staticmethod` a descriptor? What does its `__get__` return?
- What is `classmethod` a descriptor? What does its `__get__` return?
- What is `__set_name__(owner, name)` called for? (Python 3.6+)
- How do you implement a validated attribute descriptor?
- What is the `__objclass__` attribute on methods?

---

## 26. Regular Expressions

- What is the `re` module?
- What is `re.match()` vs `re.search()` vs `re.fullmatch()`?
- What is `re.findall()` vs `re.finditer()`?
- What is `re.sub()`, `re.subn()`?
- What is `re.split()`?
- What is `re.compile()`? Why compile patterns used repeatedly?
- What are regex flags: `re.IGNORECASE`, `re.MULTILINE`, `re.DOTALL`, `re.VERBOSE`?
- What is a capture group `(...)` vs non-capture group `(?:...)`?
- What is a named group `(?P<name>...)`? How do you access it?
- What is a lookahead `(?=...)` and lookbehind `(?<=...)`?
- What is `match.group()`, `group(1)`, `groups()`, `groupdict()`?
- What is `match.span()`, `start()`, `end()`?
- What is greedy vs lazy matching? (`.*` vs `.*?`)
- What is a raw string `r"..."` and why use it for regex?

> **Best Practices:**
> - Always use raw strings `r"..."` for regex patterns to avoid double-escaping.
> - Compile patterns with `re.compile()` when used in loops.
> - Use named groups for complex patterns — improves readability over positional groups.

---

## 27. Python Database Access (DB-API 2.0 / sqlite3)

- What is PEP 249 (Python DB-API 2.0)? What does it standardize?
- What is `sqlite3` module?
- What is `sqlite3.connect()`? What does it return?
- What is `connection.cursor()`?
- What is `cursor.execute(sql, params)`? How do you pass parameters safely?
- What is `?` placeholder in sqlite3 vs `%s` in other drivers?
- What is `cursor.executemany(sql, seq_of_params)`?
- What is `cursor.fetchone()`, `fetchall()`, `fetchmany(size)`?
- What is `connection.commit()`, `rollback()`?
- What is `connection.isolation_level`? What is `None` (autocommit)?
- What is `connection.execute()` shortcut?
- What is `sqlite3.Row`? What does it enable (column access by name)?
- What is `connection.row_factory`?
- What are `sqlite3` context manager semantics (`with connection`)?
- What is WAL mode (`PRAGMA journal_mode=WAL`)?
- What is `cursor.description`?
- What is `cursor.lastrowid`?
- What is `cursor.rowcount`?
- What is an in-memory database (`sqlite3.connect(":memory:")`)?
- What is `sqlite3.register_adapter()` and `register_converter()`?
- What popular Python ORMs are there? (`SQLAlchemy`, `Django ORM`, `Peewee`, `Tortoise ORM`)

> **Best Practices:**
> - Always use parameterized queries — never format SQL strings with user input.
> - Always close cursors and connections — use `with` where supported.
> - Use `sqlite3.Row` as `row_factory` for named column access.
> - Use `executemany()` for bulk inserts — far more efficient than looping `execute()`.
> - Commit explicitly after DML — do not rely on autocommit in transactional code.

---

## 28. Python Version History & Key Features

### Python 2.x (Legacy)
- What were the major Python 2.7 features? (Last 2.x release — print statement, `unicode` vs `str`, old-style classes, integer division `/` returns int)
- What is the key difference between Python 2 and Python 3? (Print function, integer division, unicode strings, `range()` vs `xrange()`, `super()`, `__future__` imports)

### Python 3.0–3.5
- Python 3.0 (2008): Print function, `str` is Unicode, integer division, `range()` is lazy, `__future__` removed, `bytes` type
- Python 3.2: `functools.lru_cache`, `concurrent.futures`, `argparse`
- Python 3.3: `yield from`, `venv`, `namespace packages`, `__qualname__`
- Python 3.4: `asyncio` (provisional), `enum`, `pathlib`, `statistics`, `tracemalloc`
- Python 3.5: `async`/`await` syntax (coroutines), `@` matrix multiplication operator, `typing` module, `os.scandir()`

### Python 3.6 (2016)
- f-strings (`f"Hello {name}"`)
- Variable annotations (`x: int = 5`)
- `async` generators and comprehensions
- `secrets` module
- `dict` order preserved (implementation detail, guaranteed in 3.7)
- Underscores in numeric literals (`1_000_000`)
- `__init_subclass__`
- `__set_name__`

### Python 3.7 (2018)
- `dataclasses` module (`@dataclass`)
- `dict` insertion order guaranteed by language spec
- `breakpoint()` built-in
- `contextvars` module (context variables — async-safe thread-local)
- `asyncio.run()` (provisional)
- PEP 563: `from __future__ import annotations` (postponed evaluation)
- `time.perf_counter_ns()`

### Python 3.8 (2019)
- Walrus operator `:=` (assignment expressions)
- Positional-only parameters `def f(a, b, /, c):`
- `f-string` `=` specifier for debugging: `f"{x=}"`
- `functools.cached_property`
- `typing.TypedDict`, `typing.Literal`, `typing.Final`
- `asyncio.run()` (stable)
- `multiprocessing.shared_memory`
- `math.prod()`

### Python 3.9 (2020)
- Built-in generics in type hints: `list[int]`, `dict[str, int]` (no more `typing.List`)
- `dict` merge operators: `|` and `|=`
- `str.removeprefix()`, `str.removesuffix()`
- `zoneinfo` module
- `graphlib` module (`TopologicalSorter`)
- `importlib.resources` improvements

### Python 3.10 (2021)
- Structural pattern matching (`match`/`case`)
- Union types with `|`: `int | str`
- `ParamSpec` and `TypeAlias`
- Better error messages (pinpoints exact location)
- `zip(strict=True)` — raises if lengths differ
- `typing.TypeGuard`

### Python 3.11 (2022 — Major performance)
- CPython 60% faster than 3.10 on average (Faster CPython project)
- Exception groups and `except*`
- `ExceptionGroup`
- `tomllib` module (TOML parsing)
- `asyncio.TaskGroup`
- `asyncio.timeout()`
- `typing.Self`, `typing.Never`, `typing.Unpack`
- Fine-grained error locations in tracebacks

### Python 3.12 (2023)
- Type parameter syntax (PEP 695): `type Vector = list[float]`, `def first[T](l: list[T]) -> T:`
- `@override` decorator in `typing`
- `pathlib.Path` improvements
- `sys.monitoring` (low-overhead instrumentation)
- `f-string` nesting improvements
- `itertools.batched()`
- Removal of deprecated `distutils`

### Python 3.13 (2024)
- Free-threaded CPython (experimental, PEP 703 — no GIL)
- JIT compiler (experimental, PEP 744)
- Improved REPL
- `copy.replace()` (PEP 698)
- `typing.TypeIs`
- Removal of deprecated APIs

### LTS & Release Cadence
- What is the Python release cycle? (Annual major release, 5 years of security support)
- What are currently supported Python versions? (3.9–3.13 as of 2024)
- What is EOL for Python 3.8? (Oct 2024)

> **Best Practices:**
> - Target Python 3.11+ for new projects — significant performance improvements.
> - Use `match`/`case` (3.10+) for complex dispatch logic.
> - Use built-in generics (`list[int]`) instead of `typing.List[int]` in 3.9+.
> - Pin Python version in `pyproject.toml` and CI with `python-requires`.

---

## 29. Tricky Senior-Level Python Questions

- What is the output of `[] == []`? `[] is []`? (`True`, `False` — equal value, different objects)
- What is the output of `(1,) == (1,)`? `(1,) is (1,)`? (`True`; implementation-dependent — small tuples may be cached)
- What is the mutable default argument trap? (`def f(x=[])` — the list persists across calls)
- What is `x = [1,2]; y = x; y += [3]; print(x)`? (`[1,2,3]` — `+=` on list mutates in place)
- What is `x = (1,2); y = x; y += (3,); print(x)`? (`(1,2)` — `+=` on tuple rebinds y)
- What is the output of `True + True + True`? (`3` — `bool` is subclass of `int`)
- What is `0.1 + 0.2 == 0.3`? (`False` — IEEE 754)
- What is `'a' * 0`? (`''`)
- What is `[None] * 3`? `[[]] * 3`? (Three references to the SAME list — shallow copy trap)
- What is `sum([[1],[2],[3]], [])`? (`[1,2,3]` — unpythonic list flattening)
- What is the output of `for i in range(3): pass; print(i)`? (`2` — loop variable persists after loop in Python)
- What is the late binding closure trap? (`[lambda: i for i in range(3)]` — all return `2`)
- How do you fix the late binding closure trap? (`lambda i=i: i` — captures by default arg)
- What is `type(lambda: None)`? (`function`)
- What is the output of `not not ""` ? (`False`)
- What is `"" or "default"`? (`"default"` — empty string is falsy)
- What is `0 or [] or {} or "hello"`? (`"hello"` — first truthy value)
- What is `1 and 2 and 3`? (`3` — last value in truthy chain)
- What is `1 and 0 and 3`? (`0` — first falsy value)
- What is `isinstance(True, int)`? (`True` — bool is subclass of int)
- What is `{} == defaultdict(list)`? (`True` — both are empty dicts)
- What is the output of `d = {1: 'a'}; d[1.0]`? (`'a'` — `1 == 1.0` and `hash(1) == hash(1.0)`)
- Can a `dict` have both `1` and `True` as separate keys? (No — `1 == True` and same hash)
- What is `[i for i in range(5) if i % 2 != 0 if i > 1]`? (`[3]`)
- What is `"hello"[100]`? (`IndexError`) vs `"hello"[100:200]`? (`""` — slices never raise)
- What is the `__class__` of an old-style class instance vs new-style in Python 3? (All Python 3 classes are new-style)

---

## 30. Tricky Generics & Type Annotation Edge Cases

- What is the difference between `list[int]` and `List[int]` at runtime?
- What is `X | Y` vs `Union[X, Y]` in Python 3.10?
- Can you use `isinstance(x, list[int])`? (No — generic aliases are not valid for `isinstance` at runtime)
- What is `get_type_hints(func)`? How is it different from `func.__annotations__`?
- What is `from __future__ import annotations` (PEP 563)? What does it do to `__annotations__`?
- What is PEP 649 (lazy evaluation of annotations, Python 3.14 candidate) vs PEP 563?
- What is `typing.get_origin()`, `typing.get_args()`?
- What is a `TypeVar` with `bound=`? What is a `TypeVar` with `constraints=`?
- What is `typing.overload`? When do you need it?
- What is `ParamSpec` (`P.args`, `P.kwargs`)? How is it used to type decorators?

---

## 31. Functional Java vs Python Comparison (Senior Mental Model)

| Concept | Java | Python |
|---|---|---|
| Lambda | `(x) -> x * 2` | `lambda x: x * 2` |
| Stream | `stream().filter().map().collect()` | Generator expression / `map()`/`filter()` |
| Optional | `Optional<T>` | `Optional` not built-in; use `None` / `typing.Optional` |
| Immutable list | `List.of(...)` | `tuple`, `frozenset` |
| Comparable | `implements Comparable<T>` | `__lt__`, `__eq__`, `@total_ordering` |
| Functional interface | `@FunctionalInterface` | Any callable — duck typed |
| Method reference | `String::length` | `str.upper` (unbound) or `"abc".upper` (bound) |
| `forEach` | `list.forEach(System.out::println)` | `for x in lst: print(x)` |
| Generics | `List<T>` — erased at runtime | `list[T]` — not enforced at runtime |
| Checked exceptions | Yes — enforced at compile time | No — all exceptions unchecked |
| Threads vs async | Threads common for I/O | `asyncio` preferred for I/O |
| GC | Mark-sweep + generational | Reference counting + cyclic GC |
| Overloading | Compile-time multiple dispatch | Not supported; use `*args`/`**kwargs`/`@singledispatch` |

---

## 32. Senior Interview: "Explain the Output" — Python

```python
# Q1 — mutable default argument
def append_to(val, lst=[]):
    lst.append(val)
    return lst

print(append_to(1))  # [1]
print(append_to(2))  # [1, 2]  ← same list reused!

# Q2 — late binding closure
funcs = [lambda: i for i in range(3)]
print([f() for f in funcs])  # [2, 2, 2]  ← all see i=2

# Q3 — is vs ==
a = 256; b = 256
print(a is b)   # True  (CPython caches -5..256)
a = 257; b = 257
print(a is b)   # False in most contexts (not cached)

# Q4 — bool subclass of int
print(True + True + False)  # 2
print(True == 1)             # True
print(True is 1)             # False

# Q5 — list multiplication shallow copy
grid = [[0] * 3] * 3
grid[0][0] = 1
print(grid)  # [[1,0,0],[1,0,0],[1,0,0]] ← same row object!

# Q6 — loop variable scope
for x in range(3):
    pass
print(x)  # 2  ← x persists after loop

# Q7 — list comprehension scope (Python 3)
x = 10
result = [x for x in range(3)]
print(x)  # 10  ← comprehension has its own scope in Python 3

# Q8 — augmented assignment: list vs tuple
a = [1, 2]
b = a
a += [3]
print(b)  # [1, 2, 3]  ← mutates in place

a = (1, 2)
b = a
a += (3,)
print(b)  # (1, 2)  ← a rebound to new tuple; b unchanged

# Q9 — None comparison
x = None
print(x == None)   # True (but bad style)
print(x is None)   # True (correct)

# Q10 — or / and return actual value
print(0 or [] or {} or "hello")   # "hello"
print(1 and 2 and 3)              # 3
print(1 and 0 and 3)              # 0

# Q11 — dict key collision: 1 and True
d = {1: "one", True: "true"}
print(d)         # {1: 'true'}  ← True == 1 overwrites
print(d[True])   # 'true'

# Q12 — slice never raises
s = "hello"
print(s[100:200])   # ""  (no IndexError)
print(s[100])       # IndexError

# Q13 — finally return overrides
def f():
    try:
        return 1
    finally:
        return 2
print(f())  # 2

# Q14 — generator exhaustion
gen = (x for x in range(3))
print(list(gen))  # [0, 1, 2]
print(list(gen))  # []  ← exhausted

# Q15 — class attribute vs instance attribute
class A:
    x = []
a1 = A()
a2 = A()
a1.x.append(1)
print(a2.x)  # [1]  ← shared class attribute

a1.x = [99]  # rebinds a1.x to new list
print(a2.x)  # [1]  ← a2.x still points to class attribute
```

> **Key Rules to Memorise:**
> - Mutable default arguments are shared across all calls — use `None` sentinel.
> - Closures capture the variable reference, not the value — late binding trap.
> - CPython caches small integers (`-5` to `256`) — `is` may be `True` by coincidence.
> - `list *= n` creates shallow copies — nested mutables are shared.
> - `dict` treats `1`, `True`, and `1.0` as the same key.
> - Loop variable persists after the loop; comprehension variable does NOT (Python 3).
> - `+=` on list mutates in place; `+=` on tuple rebinds.
> - Slices never raise `IndexError`; direct indexing does.

---

## 33. Python Performance & Profiling

- What is `cProfile`? How do you profile a function?
- What is `timeit`? When should you use it over `time.time()`?
- What is `line_profiler` (third-party)?
- What is `memory_profiler` (third-party)?
- What is `py-spy`? (Sampling profiler — zero overhead, production-safe)
- What is `guppy3` / `heapy`?
- What is `tracemalloc`?
- What is `__slots__` performance benefit?
- What is the performance of `dict` lookup vs `list` lookup vs `set` lookup?
- Why is `in` O(1) for `set`/`dict` but O(n) for `list`?
- What is `array.array` faster than `list` for?
- What is `numpy` for numerical performance? (C-backed, SIMD, avoids Python loop overhead)
- What is vectorization? What is broadcasting?
- What is the cost of function call overhead in Python?
- What is `numba` (JIT for Python)? When would you use it?
- What is Cython? What does it compile to?
- What is `ctypes`? When would you use C extensions?
- What is `cffi`?
- What is `-O` / `-OO` Python optimization flag?

> **Best Practices:**
> - Profile before optimizing — never guess where the bottleneck is.
> - Use `set`/`dict` for membership tests, never `list` in hot loops.
> - Use `local variable lookup` (assign module functions to local vars in hot loops) — slightly faster.
> - Use numpy for numeric array operations — Python loops over numbers are slow.
> - For CPU-bound work: consider `multiprocessing`, Cython, or C extensions.

---

## 34. Python Packaging & Project Structure

- What is `pip`? What is `PyPI`?
- What is `venv`? How do you create and activate a virtual environment?
- What is `pyproject.toml` (PEP 518/621)?
- What is `setup.py` vs `setup.cfg` vs `pyproject.toml`?
- What is `hatch`, `flit`, `poetry`?
- What is `requirements.txt` vs `pyproject.toml` dependencies?
- What is `pip install -e .` (editable install)?
- What is `__version__` convention?
- What is `MANIFEST.in`?
- What is `wheel` (`.whl`)? What is `sdist`?
- What is `tox`?
- What is `pre-commit`?
- What is semantic versioning (SemVer)?
- What is `importlib.metadata` (Python 3.8+)?

> **Best Practices:**
> - Always use `pyproject.toml` for new projects — it's the modern standard.
> - Pin direct dependencies in `requirements.txt`; use ranges in `pyproject.toml`.
> - Use virtual environments per project — never install packages globally.
> - Use `hatch` or `poetry` for modern dependency management.

---

## 35. Testing in Python

- What is `unittest`? What are `TestCase`, `setUp`, `tearDown`, `setUpClass`, `tearDownClass`?
- What are assert methods: `assertEqual`, `assertRaises`, `assertIn`, `assertTrue`, `assertIsNone`?
- What is `pytest`? How does it differ from `unittest`?
- What is `pytest` fixture? What is `scope` (function, class, module, session)?
- What is `pytest.mark.parametrize`?
- What is `unittest.mock.Mock`? `MagicMock`?
- What is `patch()` and `patch.object()`?
- What is `side_effect` on a Mock?
- What is `assert_called_with()`, `call_count`, `return_value`?
- What is `pytest-mock` (`mocker` fixture)?
- What is `hypothesis` (property-based testing)?
- What is `coverage.py`?
- What is `pytest-cov`?
- What is `conftest.py`?
- What is `doctest`?

> **Best Practices:**
> - Use `pytest` for new projects — simpler syntax, powerful fixtures, rich plugin ecosystem.
> - Use fixtures with appropriate scope — `session` scope for expensive setup (DB connections).
> - Use `unittest.mock.patch` as a context manager or decorator, not manually.
> - Aim for 80%+ coverage on critical business logic; 100% is not always worth it.

---

## 36. Python Security

- What is `pickle` security risk? (Arbitrary code execution on deserialization — never unpickle untrusted data)
- What is `yaml.load()` vs `yaml.safe_load()`? (Unsafe `load()` can execute arbitrary code)
- What is `eval()` security risk? (`eval("__import__('os').system('rm -rf /')")) `)
- What is `exec()` security risk?
- What is `subprocess` shell injection? (`shell=True` with user input)
- What is the safe way to use `subprocess`? (Pass as list, `shell=False`)
- What is `secrets` module? Why use it over `random`?
- What is `hashlib`? What hashes does it support?
- What is `hmac` module?
- What is `ssl` module?
- What is `cryptography` library (third-party)?
- What is SQL injection in Python DB code? How do you prevent it?
- What is input sanitization?
- What is `bandit`? (Static security analysis for Python)

> **Best Practices:**
> - Never `pickle`/`unpickle` data from untrusted sources.
> - Always use `yaml.safe_load()` — never `yaml.load()`.
> - Never use `eval()` or `exec()` with user input.
> - Always pass `subprocess` commands as a list with `shell=False`.
> - Use `secrets.token_urlsafe()` for tokens — never `random`.
