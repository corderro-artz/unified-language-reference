# C#

`multi-paradigm` · `static, strong, inferred, nominal` · `compiled to IL, JIT/AOT` · `2000`

## Contents

- **Identity**
  - [Overview](#overview)
  - [Language Type](#language-type)
  - [Paradigms](#paradigms)
  - [Mental Model](#mental-model)
- **Foundations**
  - [Lexical & Syntax](#lexical--syntax)
  - [Variables & Bindings](#variables--bindings)
  - [Type System](#type-system)
  - [Data Structures](#data-structures)
  - [Operators & Expressions](#operators--expressions)
- **Logic**
  - [Control Flow](#control-flow)
  - [Functions](#functions)
  - [Error Handling](#error-handling)
- **Abstraction**
  - [Object Model](#object-model)
  - [Functional Constructs](#functional-constructs)
  - [Modules & Namespaces](#modules--namespaces)
  - [Metaprogramming & Reflection](#metaprogramming--reflection)
- **Runtime**
  - [Memory Management](#memory-management)
  - [Concurrency & Parallelism](#concurrency--parallelism)
- **Practice**
  - [Standard Library — Start Here](#standard-library--start-here)
  - [Tooling & Ecosystem](#tooling--ecosystem)
  - [Conventions & Style](#conventions--style)
  - [Idioms & Gotchas](#idioms--gotchas)
- **Reference**
  - [Versioning & Editions](#versioning--editions)
  - [Resources](#resources)

---

## Identity

### Overview

C# is a general-purpose, statically typed language built for the .NET
runtime. It runs cross-platform on the modern `dotnet` SDK (Windows,
Linux, macOS) and powers web services (ASP.NET Core), desktop and
mobile apps, cloud back ends, games (Unity), and CLI tools. It is
known for a unified object model, strong tooling, first-class
async/await, and a steady annual cadence of language features.

### Language Type

| Axis      | This language                             |
|-----------|-------------------------------------------|
| Execution | compiled to IL, then JIT or AOT to native |
| Domain    | general-purpose                           |
| Typing    | static, strong, inferred, nominal         |
| Memory    | managed, tracing garbage collector        |

### Paradigms

- **Object-Oriented** — bundles state and behavior into types; objects interact through methods. Here: every value derives from `object`; classes, structs, records, and interfaces.
- **Imperative** — a program is a sequence of statements that change state. Here: the default statement-by-statement flow.
- **Functional** — computation as the evaluation of functions; values over mutation, functions as first-class data. Here: lambdas, LINQ, records, pattern matching, immutable types.
- **Generic** — algorithms and types parameterized over the types they act on. Here: generic classes, methods, and constraints with reified runtime types.
- **Concurrent** — work structured as independently progressing tasks. Here: `async`/`await` over `Task`, plus threads and the TPL.
- **Query** — declaratively retrieve and transform sets of data. Here: LINQ query and method syntax over any `IEnumerable<T>`.
- **Metaprogramming** — programs that read, generate, or transform code. Here: reflection, attributes, and compile-time source generators.

### Mental Model

Think in terms of a unified, managed type system: everything is an
`object`, value types live on the stack or inline while reference
types are heap-allocated and GC-managed, and the boundary between them
matters. Lean on the type system and the compiler — nullable reference
types, generics, and pattern matching catch mistakes early — and let
the runtime own memory so you focus on modeling the domain.

## Foundations

### Lexical & Syntax

- **Comments** — `//` line, `/* ... */` block, `///` XML doc comments.
- **Statements** — terminated with `;`; blocks delimited by `{ }`.
- **Keywords** — lowercase (`class`, `public`, `await`).
- **Entry point** — a `Main` method, or top-level statements in one file
  that the compiler wraps into an implicit entry point.
- **File structure** — code lives in namespaces; `using` directives sit
  at the top. One file may hold many types; the file name need not match.

```csharp
Console.WriteLine("Hello, World!");
```

<details>
<summary>Examples — one per item</summary>

```csharp
int a = 1;   // Comments — line
/* block */  // and /* ... */
/// <summary>XML doc</summary>

int b = 2;                  // Statements — end ;
public static void M() { }  // Keywords — lowercase

Console.WriteLine("Hello"); // Entry point — top-level

// File structure — namespace + usings at top
```

</details>

### Variables & Bindings

- **`var`** — implicitly typed local; the type is inferred and fixed.
- **Explicit type** — `int x = 5;` declares the type manifestly.
- **`const`** — compile-time constant; must be a literal value.
- **`readonly`** — field assignable only in declaration or constructor.
- **Scope** — block-scoped; locals must be assigned before use.
- **Shadowing** — a local may not hide another local in scope, but can
  shadow a field (disambiguate with `this.`).

```csharp
var name = "Ada";
const int Max = 100;
```

<details>
<summary>Examples — one per item</summary>

```csharp
var who = "Ada";         // var — inferred
int count = 5;           // explicit type
const int Max = 100;     // const — compile-time

class Box
{
    readonly int id;     // readonly — set in ctor

    Box(int id)          // param shadows field id
    {
        this.id = id;    // shadowing — this. = field
    }

    void M()
    {
        int local = 1;   // scope — block-scoped
    }
}
```

</details>

### Type System

| Syntax    | Type             | Size    | Range                     | Default | Literal     |
|-----------|------------------|---------|---------------------------|---------|-------------|
| `sbyte`   | signed integer   | 8-bit   | -128 .. 127               | `0`     | `(sbyte)0`  |
| `byte`    | unsigned integer | 8-bit   | 0 .. 255                  | `0`     | `(byte)0`   |
| `short`   | signed integer   | 16-bit  | -32768 .. 32767           | `0`     | `(short)0`  |
| `ushort`  | unsigned integer | 16-bit  | 0 .. 65535                | `0`     | `(ushort)0` |
| `int`     | signed integer   | 32-bit  | -2147483648 .. 2147483647 | `0`     | `0`         |
| `uint`    | unsigned integer | 32-bit  | 0 .. 4294967295           | `0`     | `0U`        |
| `long`    | signed integer   | 64-bit  | -9.22e18 .. 9.22e18       | `0`     | `0L`        |
| `ulong`   | unsigned integer | 64-bit  | 0 .. 1.84e19              | `0`     | `0UL`       |
| `float`   | float            | 32-bit  | ±1.5e-45 .. ±3.4e38       | `0`     | `0f`        |
| `double`  | float            | 64-bit  | ±5.0e-324 .. ±1.7e308     | `0`     | `0d`        |
| `decimal` | decimal          | 128-bit | ±1.0e-28 .. ±7.9228e28    | `0`     | `0m`        |
| `bool`    | boolean          | 1 byte  | true / false              | `false` | `true`      |
| `char`    | UTF-16 unit      | 16-bit  | U+0000 .. U+FFFF          | `'\0'`  | `'a'`       |

<details>
<summary>Native-sized and big integers</summary>

| Syntax  | Type                | Size      | Range              | Default | Literal    |
|---------|---------------------|-----------|--------------------|---------|------------|
| `nint`  | native signed int   | 32/64-bit | platform-dependent | `0`     | `(nint)0`  |
| `nuint` | native unsigned int | 32/64-bit | platform-dependent | `0`     | `(nuint)0` |

`System.Numerics.BigInteger` is an arbitrary-precision integer with
no fixed range.
</details>

- **Inference** — `var` infers a local's type; generic method type
  arguments are usually inferred from the arguments.
- **Conversion** — widening numeric conversions are implicit; narrowing
  needs an explicit cast `(int)x`. No implicit `decimal`↔`double`.
- **Nullability** — reference types are non-nullable by default;
  `string?` opts in to null. Value types use `Nullable<T>` / `int?`.
- **Generics** — types and methods take type parameters with `where`
  constraints (`where T : class`, `: IComparable<T>`, `: new()`).
  Generics are reified: type arguments persist at runtime.

```csharp
var count = 42;        // inferred int
int? maybe = null;     // nullable value type
string? name = null;   // nullable reference
```

<details>
<summary>Examples — one per item</summary>

```csharp
var n = 42;                  // Inference — var
int x = (int)3.9;            // Conversion — cast
string? s = null;            // Nullability — ?
T First<T>(T[] a) => a[0];   // Generics — type param
```

</details>

> [!NOTE]
> Each keyword above is an alias for a `System` type — `int` is
> `System.Int32`, `string` is `System.String`. They are interchangeable.

### Data Structures

| Structure         | Syntax                | Ordered | Mutable | Use                 |
|-------------------|-----------------------|---------|---------|---------------------|
| array             | `new[] { 1, 2, 3 }`   | yes     | yes     | fixed-size sequence |
| `List<T>`         | `[1, 2, 3]`           | yes     | yes     | growable sequence   |
| `Dictionary<K,V>` | `new() { ["a"] = 1 }` | no      | yes     | key lookup          |
| `HashSet<T>`      | `new() { 1, 2 }`      | no      | yes     | unique membership   |
| `Queue<T>`        | `new Queue<int>()`    | yes     | yes     | FIFO                |
| `Stack<T>`        | `new Stack<int>()`    | yes     | yes     | LIFO                |
| tuple             | `(1, "a")`            | yes     | yes     | lightweight group   |
| record            | `new Point(1, 2)`     | n/a     | no      | value-equality data |

> [!NOTE]
> The `[...]` collection-expression literal (C# 12+) builds arrays,
> `List<T>`, `Span<T>`, and other collection types from one syntax.

### Operators & Expressions

| Category   | Operator | Name         | Example      | Note                |
|------------|----------|--------------|--------------|---------------------|
| Arithmetic | `+`      | add          | `a + b`      | also string concat  |
| Arithmetic | `/`      | divide       | `a / b`      | integer if both int |
| Arithmetic | `%`      | modulo       | `a % b`      | remainder           |
| Comparison | `==`     | equal        | `a == b`     | value for structs   |
| Comparison | `<=`     | less-equal   | `a <= b`     |                     |
| Logical    | `&&`     | and          | `a && b`     | short-circuits      |
| Logical    | `\|\|`   | or           | `a \|\| b`   | short-circuits      |
| Bitwise    | `&`      | and          | `a & b`      | non-short-circuit   |
| Bitwise    | `<<`     | left shift   | `a << 2`     |                     |
| Assignment | `=`      | assign       | `a = b`      |                     |
| Assignment | `+=`     | add-assign   | `a += b`     |                     |
| Null       | `??`     | coalesce     | `a ?? b`     | b if a is null      |
| Null       | `?.`     | null-cond    | `a?.B`       | null if a is null   |
| Special    | `=>`     | lambda       | `x => x + 1` | also expr body      |
| Special    | `is`     | pattern test | `o is int n` | type/pattern match  |

- **Overloading** — most operators are user-overloadable via
  `static operator` members. C# 14 adds user-defined compound
  assignment operators (e.g. a custom `+=`).

> [!NOTE]
> C# 14 allows null-conditional operators on the left of an
> assignment: `customer?.Order = GetOrder();` evaluates the right
> side only when `customer` is non-null.

## Logic

### Control Flow

- **`if` / `else`** — boolean conditional branching.
- **`switch` statement** — multi-way branch; supports patterns and
  `when` guards.
- **`switch` expression** — `x switch { 1 => "a", _ => "b" }` returns a
  value.
- **`for` / `while` / `do`** — counted and conditional loops.
- **`foreach`** — iterates any `IEnumerable<T>`.
- **Pattern matching** — type, property, list, and relational patterns
  combine with `is` and `switch`.
- **Jumps** — `break`, `continue`, `return`, `goto`.

```csharp
var label = n switch
{
    < 0 => "negative",
    0 => "zero",
    _ => "positive",
};
```

<details>
<summary>Examples — one per item</summary>

```csharp
if (n > 0) { }              // if / else
else { }

switch (n)                  // switch statement
{
    case 0: break;
    default: break;
}

var s = n switch            // switch expression
{
    < 0 => "neg",
    _ => "pos",
};

for (int i = 0; i < 3; i++) { }   // for
while (n > 0) { n--; }            // while / do

foreach (var c in "ab") { }       // foreach

if (o is int v) { }               // pattern matching

// Jumps — break / continue / return / goto
```

</details>

### Functions

- **Declaration** — methods belong to a type; return type precedes the
  name. Expression-bodied form: `int Sq(int x) => x * x;`.
- **Parameters** — support defaults (`int x = 0`), named arguments,
  `params` arrays/collections, and `ref`/`out`/`in` by-reference.
- **Local functions** — nested, named functions that capture locals.
- **Lambdas** — `(x, y) => x + y`; C# 14 allows modifiers on simple
  lambda parameters without naming the type.
- **First-class** — functions are values via `delegate`, `Func<>`,
  `Action<>`; passed and stored freely.
- **Higher-order** — methods take and return delegates (LINQ).
- **Iterators** — `yield return` produces lazy sequences.

```csharp
Func<int, int> dbl = x => x * 2;
```

<details>
<summary>Examples — one per item</summary>

```csharp
int Sq(int x) => x * x;           // Declaration

void P(int a, int b = 0) { }      // Parameters — default

int Local()                       // Local functions
{
    return 1;
}

var add = (int a, int b) => a + b;   // Lambdas

Func<int, int> f = Sq;            // First-class

int Apply(Func<int, int> g)       // Higher-order
    => g(2);

IEnumerable<int> Seq()            // Iterators
{
    yield return 1;
}
```

</details>

### Error Handling

- **Exceptions** — the primary model: `throw`, `try`/`catch`/`finally`.
  Filter with `catch (E e) when (cond)`.
- **Resource cleanup** — `using` / `using` declarations dispose
  `IDisposable` deterministically at scope exit.
- **Propagation** — uncaught exceptions unwind the stack to the nearest
  matching handler.
- **Assertions** — `Debug.Assert` (debug builds);
  `ArgumentNullException.ThrowIfNull` and guard helpers for arguments.

```csharp
try { Risky(); }
catch (IOException e) { Log(e); }
finally { Cleanup(); }
```

<details>
<summary>Examples — one per item</summary>

```csharp
try                              // Exceptions
{
    Risky();
}
catch (IOException e) when (true)
{
    Log(e);
}
finally { }

using var f =                   // Resource cleanup
    File.OpenText("x");

throw new InvalidOperationException();  // Propagation

ArgumentNullException.ThrowIfNull(f);   // Assertions
Debug.Assert(f is not null);
```

</details>

## Abstraction

### Object Model

- **Types** — `class` (reference), `struct` (value), `record` /
  `record struct` (value-equality), `interface`, and `enum`.
- **Instantiation** — `new T(args)`; object/collection initializers set
  members inline.
- **Members** — fields, properties (`{ get; set; }`, `init`), methods,
  events, indexers. The C# 14 `field` keyword gives a property accessor
  a synthesized backing field without declaring one.
- **Encapsulation** — access modifiers `public`, `private`,
  `protected`, `internal`, and combinations.
- **Inheritance** — single base class; `virtual`/`override`/`abstract`/
  `sealed` control polymorphism.
- **Interfaces** — multiple inheritance of contracts; may carry default
  implementations.
- **Polymorphism** — virtual dispatch plus generics.
- **Composition** — favored via interfaces and member objects.

```csharp
public record Point(int X, int Y);
```

<details>
<summary>Examples — one per item</summary>

```csharp
class Animal { }                  // Types — class
struct Pt { }                     // struct
record R(int X);                  // record
interface IGo { void Go(); }      // interface
enum Color { Red }                // enum

var a = new Animal();             // Instantiation

class Dog : IGo                   // Interfaces
{
    public string Name { get; init; }   // Members
    private int _age;                    // Encapsulation
    public void Go() { }
}

class Base { public virtual void M() { } }   // Inheritance
class Pup : Base
{
    public override void M() { }   // Polymorphism
}

Base b = new Pup();
b.M();                            // dispatch to Pup.M

class Car { Dog _pet = new(); }   // Composition
```

</details>

### Functional Constructs

- **Immutability** — `readonly` fields, `init` setters, and `record`
  types with nondestructive `with` copies.
- **First-class functions** — lambdas, delegates, method groups.
- **Pattern matching** — deconstruct and match records, tuples, and
  lists in `switch` expressions.
- **LINQ** — declarative query/transform over sequences, lazily
  evaluated.
- **Discriminated unions** — approximated today with records and
  sealed hierarchies (native unions are a proposed feature).
- **Tuples** — lightweight multiple returns with deconstruction.

```csharp
var evens = nums.Where(n => n % 2 == 0);
```

<details>
<summary>Examples — one per item</summary>

```csharp
record Point(int X, int Y);       // Immutability
var p = new Point(1, 2);
var p2 = p with { X = 9 };        // copy via with

Func<int, int> dbl = x => x * 2;  // First-class

var where = p switch              // Pattern matching
{
    Point(0, 0) => "origin",
    _ => "other",
};

var evens =                       // LINQ
    nums.Where(n => n % 2 == 0);

abstract record Shape;            // Discriminated unions
record Circle(double R) : Shape;

var (x, y) = (1, 2);              // Tuples
```

</details>

### Modules & Namespaces

- **Namespace** — the logical grouping unit; `namespace App.Data;`
  (file-scoped) or block form. Maps to dotted type names.
- **Assembly** — the deployment/compilation unit (`.dll`/`.exe`); the
  boundary for `internal` visibility.
- **Imports** — `using App.Data;`; `global using` applies project-wide;
  `using static` imports static members.
- **Visibility** — `public` crosses assemblies; `internal` stays within
  one (widen via `InternalsVisibleTo`).
- **Packaging** — NuGet packages (`.nupkg`); the registry is
  nuget.org; restore and publish via the `dotnet` CLI.

```csharp
namespace App.Data;          // file-scoped
using System.Linq;           // import a namespace
global using System.Text;    // project-wide import
```

<details>
<summary>Examples — one per item</summary>

```csharp
namespace App.Data;          // Namespace — file-scoped

// Assembly — App.dll / App.exe (build unit)

using System.Linq;           // Imports
global using System.Text;
using static System.Math;

public class Pub { }         // Visibility — cross-assembly
internal class Internal { }  // one assembly only

// Packaging — dotnet add package <Name>
```

</details>

### Metaprogramming & Reflection

- **Reflection** — inspect types, members, and attributes at runtime via
  `System.Reflection`; instantiate and invoke dynamically.
- **Attributes** — declarative metadata (`[Obsolete]`, custom
  attributes) read by tools and reflection.
- **Source generators** — Roslyn compile-time code generation; no
  runtime cost, AOT-friendly.
- **Expression trees** — represent code as data (`Expression<Func<>>`),
  the basis of LINQ providers.
- **`dynamic`** — opt into late-bound, runtime-resolved member access.

<details>
<summary>Examples — one per item</summary>

```csharp
Type t = typeof(string);         // Reflection
var ms = t.GetMethods();

[Obsolete("use B")]              // Attributes
static void A() { }

// Source generators — Roslyn, compile-time

Expression<Func<int, int>>       // Expression trees
    e = x => x + 1;

dynamic d = 1;                   // dynamic
d = "now a string";
```

</details>

> [!WARNING]
> Reflection is powerful but slow and can break under trimming/AOT.
> Prefer source generators for hot paths and trimmed apps.

## Runtime

### Memory Management

- **Managed heap + GC** — reference types are heap-allocated and freed
  by a generational, tracing garbage collector; no manual `free`.
- **Value vs reference** — `struct` values live inline or on the stack;
  `class` instances are heap references. Choose `struct` for small,
  immutable data.
- **`IDisposable` / `using`** — deterministic cleanup of unmanaged
  resources; `finalizers` (`~T`) are a non-deterministic backstop.
- **`Span<T>` / `stackalloc`** — allocation-free slices over stack,
  array, or unmanaged memory; C# 14 adds first-class span conversions.
- **Pointers / `unsafe`** — raw pointers and `fixed` are available in an
  `unsafe` context for interop and performance.

<details>
<summary>Examples — one per item</summary>

```csharp
var o = new object();            // Managed heap + GC

struct V { public int X; }       // Value vs reference
class Ref { public int X; }

using var f =                    // IDisposable / using
    File.OpenText("data.txt");

Span<int> s = stackalloc int[4]; // Span / stackalloc

unsafe                           // Pointers / unsafe
{
    int n = 1;
    int* p = &n;
}
```

</details>

> [!NOTE]
> Span and ref structs cannot escape to the heap, which keeps
> stack-only memory safe without a GC pause.

### Concurrency & Parallelism

- **`async`/`await`** — the idiomatic model; `await` a `Task` or
  `ValueTask` without blocking the thread.
- **Tasks (TPL)** — `Task.Run`, `Task.WhenAll`, continuations for
  parallel and asynchronous work.
- **Threads** — `System.Threading.Thread` and the thread pool for raw
  parallelism.
- **Synchronization** — `lock`, `Monitor`, `SemaphoreSlim`,
  `Interlocked` atomics, and concurrent collections.
- **Channels & dataflow** — `System.Threading.Channels` for
  producer/consumer pipelines.
- **Parallel loops** — `Parallel.For`/`ForEach` and PLINQ
  (`.AsParallel()`).

```csharp
var data = await client.GetAsync(url);
```

<details>
<summary>Examples — one per item</summary>

```csharp
await Task.Delay(100);           // async / await

var t = Task.Run(() => 1);       // Tasks (TPL)
await Task.WhenAll(t);

new Thread(() => { }).Start();   // Threads

lock (gate) { }                  // Synchronization
Interlocked.Increment(ref n);

var ch =                         // Channels & dataflow
    Channel.CreateUnbounded<int>();

Parallel.For(0, 4, i => { });    // Parallel loops
```

</details>

> [!WARNING]
> Never `.Result` or `.Wait()` on a `Task` in async code — it can
> deadlock. Await it instead.

## Practice

### Standard Library — Start Here

The standard library is the .NET Base Class Library (BCL), shipped
with the SDK/runtime. Reference it with `using` directives; most apps
enable implicit `global using` for common namespaces.

| Module                           | Purpose                           | Import                                  |
|----------------------------------|-----------------------------------|-----------------------------------------|
| `System`                         | core types, `Console`, primitives | `using System;`                         |
| `System.Collections.Generic`     | `List<T>`, `Dictionary<K,V>`      | `using System.Collections.Generic;`     |
| `System.Linq`                    | LINQ query operators              | `using System.Linq;`                    |
| `System.Threading.Tasks`         | `Task`, async primitives          | `using System.Threading.Tasks;`         |
| `System.IO`                      | files, streams, paths             | `using System.IO;`                      |
| `System.Text`                    | `StringBuilder`, encodings        | `using System.Text;`                    |
| `System.Text.Json`               | JSON serialize/deserialize        | `using System.Text.Json;`               |
| `System.Net.Http`                | `HttpClient`, web requests        | `using System.Net.Http;`                |
| `System.Text.RegularExpressions` | regex matching                    | `using System.Text.RegularExpressions;` |
| `System.Numerics`                | `BigInteger`, vectors             | `using System.Numerics;`                |
| `System.Diagnostics`             | tracing, `Stopwatch`              | `using System.Diagnostics;`             |

### Tooling & Ecosystem

| Tool                  | Role                         | Invoke                 |
|-----------------------|------------------------------|------------------------|
| .NET SDK              | runtime + compiler + CLI     | `dotnet --info`        |
| `dotnet new`          | project/template scaffolding | `dotnet new console`   |
| `dotnet build`        | compile to IL assemblies     | `dotnet build`         |
| `dotnet run`          | build and run a project      | `dotnet run`           |
| `dotnet test`         | run the test suite           | `dotnet test`          |
| NuGet                 | package manager              | `dotnet add package X` |
| `dotnet pack`         | build a NuGet package        | `dotnet pack`          |
| `dotnet format`       | code formatter               | `dotnet format`        |
| Roslyn analyzers      | linting / diagnostics        | via build              |
| file-based apps       | run a `.cs` file, no project | `dotnet app.cs`        |
| Visual Studio / Rider | IDE + debugger               | (GUI)                  |
| ASP.NET Core          | dominant web framework       | `dotnet new web`       |

### Conventions & Style

| Identifier                | Convention           | Example        |
|---------------------------|----------------------|----------------|
| types / namespaces        | PascalCase           | `HttpClient`   |
| interfaces                | `I` + PascalCase     | `IDisposable`  |
| methods / properties      | PascalCase           | `ToString`     |
| public members            | PascalCase           | `Count`        |
| local variables / params  | camelCase            | `userId`       |
| private instance fields   | `_camelCase`         | `_workerQueue` |
| private static fields     | `s_camelCase`        | `s_cache`      |
| thread-static fields      | `t_camelCase`        | `t_buffer`     |
| constants                 | PascalCase           | `MaxLength`    |
| type parameters           | `T` + PascalCase     | `TResult`      |
| async methods             | PascalCase + `Async` | `ReadAsync`    |

- **Formatting** — four-space indentation, Allman braces (opening brace
  on its own line). `dotnet format` and `.editorconfig` enforce style.
- **Project layout** — one project per `.csproj`; group projects in a
  solution (`.sln` / `.slnx`); `src/` and `tests/` by convention.
- **Doc comments** — `///` XML comments generate API documentation.

Typical multi-project solution layout — source projects under `src/`,
test projects under a parallel `tests/`:

```text
MyApp.slnx                 solution (or .sln)
├─ src/                    application projects
│  ├─ MyApp/               startup project
│  │  └─ MyApp.csproj
│  └─ MyApp.Core/          class library
│     └─ MyApp.Core.csproj
├─ tests/                  test projects
│  └─ MyApp.Core.Tests/    xUnit / MSTest
│     └─ MyApp.Core.Tests.csproj
├─ Directory.Build.props   shared MSBuild props
├─ .editorconfig           style + analyzer rules
└─ README.md
```

Official style guide:
<https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions>

### Idioms & Gotchas

- **LINQ everywhere** — prefer declarative `Where`/`Select`/`Aggregate`
  over manual loops for clarity.
- **Embrace nullable reference types** — enable `<Nullable>enable` and
  let the compiler track `null`.
- **Records for data** — use `record` for immutable value-style models
  with free equality and `with`.
- **`Async` all the way** — propagate `await`; don't block on tasks.

> [!WARNING]
> Default struct equality uses reflection and is slow — override
> `Equals`/`GetHashCode` or use a `record struct`.

> [!WARNING]
> Captured loop variables: a `foreach` variable is fresh per
> iteration, but beware capturing mutable state in closures.

## Reference

### Versioning & Editions

.NET follows an annual release cadence. Even-numbered releases are
**LTS** (3 years of support); odd-numbered are **STS** (2 years).
**.NET 10 is the current LTS** (released November 2025) and ships
**C# 14**. The language version defaults to the target framework but
can be set with `<LangVersion>` in the project file.

This document targets **modern, cross-platform .NET** (the `dotnet`
SDK), not legacy **.NET Framework 4.x**. The C# language is shared
between them, but .NET Framework is Windows-only, frozen at older
language/BCL surfaces, and tooled differently; new development should
use the `dotnet` SDK.

> [!NOTE]
> C# 14 highlights: extension members (incl. extension properties),
> the `field` keyword, null-conditional assignment, first-class
> `Span<T>`, `nameof` of unbound generics, and partial
> constructors/events.

### Resources

- Official docs: <https://learn.microsoft.com/dotnet/csharp/>
- Language reference: <https://learn.microsoft.com/dotnet/csharp/language-reference/>
- Language specification: <https://learn.microsoft.com/dotnet/csharp/language-reference/language-specification/readme>
- Coding conventions: <https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions>
- NuGet package registry: <https://www.nuget.org/>
- .NET download / SDK: <https://dotnet.microsoft.com/download>
