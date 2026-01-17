---

# Transactions & ACID properties:

## 📑 Table of Contents
1. [What is a Transaction?](#1️⃣-what-is-a-transaction)
2. [Commit & Rollback](#2️⃣-commit--rollback)
3. [ACID Properties](#3️⃣-acid-properties-very-important-)
4. [Transactions in ADO.NET](#4️⃣-how-transactions-are-implemented-in-net)
5. [TransactionScope](#5️⃣-transactionscope-introduced-in-net-framework-20)
6. [Entity Framework Transactions](#6️⃣-entity-framework-ef--ef-core)
7. [Version-wise Summary](#7️⃣-version-wise-summary-interview-gold-)
8. [CommitAsync & RollbackAsync in .NET 6+](#do-we-actually-use-await-transactioncommitasync-and-await-transactionrollbackasync-after-net-6)

---

## 1️⃣ What is a Transaction?

A transaction is a **group of database operations** that are treated as **one single unit of work**.

👉 Either **all operations succeed** or **none of them are applied**.

### Example:

**Transfer ₹100 from A to B**

1. Debit A
2. Credit B

Both must succeed, otherwise data becomes incorrect.


## 2️⃣ Commit & Rollback

### ✅ Commit
- Saves all changes **permanently** to the database
- Called when everything succeeds
```
Transaction successful → COMMIT
```

### ❌ Rollback
- **Undo** all changes
- Called when any error occurs
```
Something failed → ROLLBACK
```

## 3️⃣ ACID Properties (VERY IMPORTANT 🔥)

### A – Atomicity
**All or nothing**

- If one operation fails → entire transaction fails
- No partial data saved

### C – Consistency
**Database remains valid**

- Data must follow rules, constraints, keys, etc.
- Before and after transaction → data is consistent

### I – Isolation
**Transactions don't interfere**

- Multiple transactions can run together
- One transaction doesn't see another's incomplete data

**Isolation levels:**
- Read Uncommitted
- Read Committed
- Repeatable Read
- Serializable

### D – Durability
**Committed data is permanent**

- Even if power failure or crash happens
- Data remains saved

## 4️⃣ How Transactions are Implemented in .NET

### 🟦 ADO.NET (Since very early .NET – Framework 1.0)
```csharp
SqlConnection con = new SqlConnection(connString);
con.Open();

SqlTransaction txn = con.BeginTransaction();

try
{
    SqlCommand cmd1 = new SqlCommand("SQL1", con, txn);
    cmd1.ExecuteNonQuery();

    SqlCommand cmd2 = new SqlCommand("SQL2", con, txn);
    cmd2.ExecuteNonQuery();

    txn.Commit();   // ✅ success
}
catch
{
    txn.Rollback(); // ❌ failure
}
```

👉 **Works in:**
- .NET Framework 2.0, 3.5, 4.0, 4.5
- .NET Core
- .NET 6+

## 5️⃣ TransactionScope (Introduced in .NET Framework 2.0)

Used for **multiple resources** (multiple DBs, services)
```csharp
using (TransactionScope scope = new TransactionScope())
{
    // DB operation 1
    // DB operation 2

    scope.Complete(); // Commit
}
```

**If `Complete()` is NOT called → Rollback automatically**

⚠️ Heavier than ADO.NET transaction

## 6️⃣ Entity Framework (EF / EF Core)

### EF (Framework 4.0+)
```csharp
using (var txn = context.Database.BeginTransaction())
{
    try
    {
        context.SaveChanges();
        txn.Commit();
    }
    catch
    {
        txn.Rollback();
    }
}
```

### EF Core (.NET Core / .NET 6)
```csharp
using var transaction = await context.Database.BeginTransactionAsync();
try
{
    await context.SaveChangesAsync();
    await transaction.CommitAsync();
}
catch
{
    await transaction.RollbackAsync();
}
```

## 7️⃣ Version-wise Summary (Interview Gold 💎)

| Technology | Transaction Support |
|------------|-------------------|
| **.NET Framework 1.0** | ADO.NET transactions |
| **.NET Framework 2.0** | + TransactionScope |
| **.NET Framework 4.5** | ADO.NET, EF, TransactionScope |
| **.NET Core** | ADO.NET, EF Core |
| **.NET 5 / 6 / 7** | Same as Core (async support improved) |


## 8️⃣ One-Line Interview Answer

A transaction ensures **ACID properties**. **Commit** saves all changes permanently, **rollback** undoes everything if any failure occurs. In .NET, transactions are implemented using **ADO.NET (SqlTransaction)**, **TransactionScope**, and **Entity Framework** across .NET Framework and .NET Core/.NET 6+.

---

## Do we actually use `await transaction.CommitAsync()` and `await transaction.RollbackAsync()` after .NET 6?

**Short answer: Yes, we still use them in .NET 6+ — but not always.**


## 1️⃣ Do CommitAsync() and RollbackAsync() exist in .NET 6+?

✅ **YES**

They are part of **EF Core**, not the .NET runtime version.
```csharp
await transaction.CommitAsync();
await transaction.RollbackAsync();
```

They are:
- Valid in .NET Core 3.1
- .NET 5
- .NET 6 / 7 / 8

**Nothing is deprecated here.**

## 2️⃣ Do we always need to call them?

❌ **No**

It depends on how you are using EF Core.

## 3️⃣ Case 1: Explicit transaction (YOU created it) ✅

👉 **You MUST call CommitAsync() or RollbackAsync()**
```csharp
using var transaction = await context.Database.BeginTransactionAsync();
try
{
    await context.SaveChangesAsync();
    await transaction.CommitAsync();   // REQUIRED
}
catch
{
    await transaction.RollbackAsync(); // REQUIRED
}
```

🔹 **Why?**
- EF Core does NOT auto-commit when you start a transaction manually
- If you don't commit → transaction is disposed → auto rollback

✔ **This pattern is still best practice in .NET 6+**


## 4️⃣ Case 2: No explicit transaction (MOST COMMON) ⚠️
```csharp
await context.SaveChangesAsync();
```

👉 **EF Core automatically:**
- Opens a transaction
- Commits if success
- Rolls back if failure

❌ **You do NOT write CommitAsync() or RollbackAsync()**

This is why many devs think transactions are "not used anymore" — they are **implicit**.

## 5️⃣ Case 3: Multiple SaveChanges() in one unit of work

**You must use explicit transaction** 👇
```csharp
using var transaction = await context.Database.BeginTransactionAsync();

try
{
    await context.SaveChangesAsync(); // step 1
    await context.SaveChangesAsync(); // step 2

    await transaction.CommitAsync(); // REQUIRED
}
catch
{
    await transaction.RollbackAsync();
}
```

**Without explicit transaction → first save may commit, second may fail** ❌

## 6️⃣ What happens if you skip Commit in .NET 6+?

**Important interview point** 🔥
```csharp
using var transaction = await context.Database.BeginTransactionAsync();
await context.SaveChangesAsync();
// no CommitAsync
```

👉 **Result:**
- Transaction is disposed
- EF Core rolls back automatically
- **No data is saved**

## 7️⃣ Sync vs Async (Modern Answer)

In .NET 6+, prefer:

✅ `BeginTransactionAsync()`  
✅ `CommitAsync()`  
✅ `RollbackAsync()`

**Especially in:**
- Web APIs
- High-concurrency systems
- Cloud apps (Azure, AWS)

## 8️⃣ Interview One-Liner (Perfect)

In .NET 6+, `CommitAsync()` and `RollbackAsync()` are still used when we **explicitly create a transaction** in EF Core. If we don't create a transaction manually, **EF Core handles commit and rollback automatically** inside `SaveChangesAsync()`.

---
