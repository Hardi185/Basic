# C# Interview Questions & Answers

## 📑 Table of Contents

- [C# Basics](#c-basics)
- [Memory Management & Garbage Collection](#memory-management--garbage-collection)
- [Collections & LINQ](#collections--linq)
- [Exception Handling](#exception-handling)
- [Async Programming & Threading](#async-programming--threading)
- [Advanced C# Concepts](#advanced-c-concepts)
- [Tricky Questions](#tricky-questions)
- [API Security](#api-security)

---

## C# Basics

### 1. What is C#?
C# is an object-oriented, strongly typed programming language used to build applications on the .NET platform.

### 2. Managed vs Unmanaged code?
- **Managed** → Runs under CLR (automatic memory management)
- **Unmanaged** → Runs outside CLR (manual memory management)

### 3. What is JIT compilation?
JIT converts IL code to machine code at runtime.

### 4. Value type vs Reference type?
- **Value type** → Stores actual value (stack)
- **Reference type** → Stores reference (heap)

### 5. Struct vs Class?
- **Struct** → Value type, lightweight
- **Class** → Reference type, supports inheritance

### 6. Boxing and Unboxing?
- **Boxing** → Value type → Object
- **Unboxing** → Object → Value type

### 7. What is enum?
Enum is a set of named constant values.

### 8. Readonly vs Const?
- **const** → Compile-time, cannot change
- **readonly** → Runtime, set once

### 9. What is namespace?
Namespace organizes code and avoids name conflicts.

---

## Memory Management & Garbage Collection

### 1. What is Garbage Collection (GC)?
GC automatically frees unused managed memory by removing objects that are no longer referenced.

### 2. Heap vs Stack?
- **Stack** → Stores method calls & value types (fast, automatic cleanup)
- **Heap** → Stores reference types (managed by GC)

### 3. What is IDisposable?
IDisposable is used to release unmanaged resources (files, DB connections) using `Dispose()`.

### 4. What is using statement?
Ensures `Dispose()` is automatically called, even if an exception occurs.
```csharp
using(var con = new SqlConnection()) { }
```

### 5. What is Finalize()?
`Finalize()` is called by GC before destroying an object to clean unmanaged resources (non-deterministic).

### 6. How does GC work?
GC:
1. Identifies unused objects
2. Cleans them
3. Compacts memory
4. Uses Generations (0, 1, 2) for optimization

### 7. What causes memory leaks in C#?
- Unreleased unmanaged resources
- Event handlers not unsubscribed
- Static references
- Not calling `Dispose()`

---

## Collections & LINQ

### 1. Array vs List vs Dictionary
- **Array** → Fixed size, fast access
- **List** → Dynamic size, easy add/remove
- **Dictionary** → Key-value pairs, fastest lookup by key

### 2. IEnumerable vs IQueryable?
- **IEnumerable** → In-memory, LINQ runs in app
- **IQueryable** → Database-level, LINQ converts to SQL

### 3. When to use Dictionary?
When you need fast lookup using a unique key.

### 4. What is LINQ?
LINQ allows querying collections using C# syntax.

### 5. Deferred vs Immediate execution?
- **Deferred** → Executes when iterated (`Where`, `Select`)
- **Immediate** → Executes immediately (`ToList`, `Count`)

### 6. Select vs SelectMany?
- **Select** → One-to-one mapping
- **SelectMany** → Flattens nested collections

### 7. First vs FirstOrDefault vs Single?
- **First** → Returns first, throws if empty
- **FirstOrDefault** → Returns default if empty
- **Single** → Exactly one, throws if none or many

### 8. Any vs All?
- **Any** → Checks if at least one matches
- **All** → Checks if all match

### 9. What is GroupBy?
Groups elements based on a key.

### 10. What is ToList()?
Converts result to a List and forces execution.

---

## Exception Handling

### 1. What is an exception?
An exception is a runtime error that disrupts normal program execution.

### 2. try-catch-finally usage?
- **try** → Code that may fail
- **catch** → Handle the exception
- **finally** → Always executes (cleanup)

### 3. throw vs throw ex?
- **throw** → Preserves original stack trace ✅
- **throw ex** → Resets stack trace ❌

### 4. What is a custom exception?
A user-defined exception created by inheriting from `Exception` to handle business-specific errors.

### 5. When should you catch exceptions?
- When you can handle or recover
- At application boundaries (UI/API layer)
- Not where you cannot add value

---

## Async Programming & Threading

### 1. What is async/await?
`async/await` enables non-blocking asynchronous programming, allowing code to run without blocking the main thread.

### 2. Task vs Thread?
- **Thread** → OS-level, heavy, manual management
- **Task** → Lightweight, managed by thread pool, preferred

### 3. What is deadlock?
A deadlock occurs when two threads wait on each other indefinitely.

### 4. What happens if we don't use await?
- Method runs asynchronously
- Caller continues execution immediately
- Result may not be available yet

### 5. ConfigureAwait(false)?
Prevents resuming on the original context, improving performance and avoiding deadlocks (used in libraries).

### 6. Parallel programming vs async?
- **Async** → I/O-bound, non-blocking
- **Parallel** → CPU-bound, multiple cores

---

## Advanced C# Concepts

### 1. What are delegates?
Delegates are type-safe references to methods.

### 2. What is an event?
An event notifies subscribers when something happens, built on delegates.

### 3. Func vs Action?
- **Func** → Returns a value
- **Action** → Returns void

### 4. What is reflection?
Reflection allows inspecting metadata of types at runtime.

### 5. What are attributes?
Attributes add metadata to code (e.g., validation, mapping).

### 6. What is Dependency Injection?
DI provides dependencies from outside, improving testability and loose coupling.

### 7. What are Generics?
Generics allow type-safe, reusable code without boxing.

### 8. What is an extension method?
Adds new methods to existing types without modifying them.

### 9. What is a partial class?
A class split across multiple files.

### 10. What is a record (C# 9+)?
A record is an immutable reference type optimized for data models.

---

## Tricky Questions

### 1. Can a constructor be private?
✅ **Yes** — used in Singleton pattern or static classes.

### 2. Can we override a static method?
❌ **No** — static methods belong to the class, not the instance.

### 3. Can a class be both abstract and sealed?
❌ **No** — abstract requires inheritance, sealed prevents it.

### 4. Why is string immutable?
For performance, security, and thread safety.

### 5. == vs .Equals()?
- **==** → Compares value or reference (overloadable)
- **.Equals()** → Compares content logically

### 6. What happens if exception is not handled?
Application terminates and runtime throws error.

### 7. Default access modifier in C#?
- **Class level** → `internal`
- **Members** → `private`

---

## API Security

### How do you secure APIs?
- Use **Authentication** (JWT / OAuth)
- Apply **Authorization** using `[Authorize]`, roles, and policies
- Enforce **HTTPS**
- Configure **CORS**
- Validate inputs and handle exceptions globally
- Use **rate limiting** and **logging**

---
