# Basics of LINQ (Language-Integrated Query)

## 📑 Table of Contents

- [Introduction](#introduction)
- [Key Concepts](#key-concepts)
- [LINQ Query Syntax](#linq-query-syntax)
  - [1. Query Syntax](#1-query-syntax)
  - [2. Method Syntax](#2-method-syntax)
- [Common LINQ Methods](#common-linq-methods)
- [Example: LINQ with a List](#example-linq-with-a-list)
- [LINQ to SQL](#linq-to-sql)
  - [Example](#example)
- [LINQ to XML](#linq-to-xml)
  - [Example](#example-1)
- [LINQ to Entities](#linq-to-entities)
  - [Example](#example-2)
- [LINQ Joins in C#](#linq-joins-in-c)
  - [1. Inner Join](#1-inner-join)
  - [2. Left Outer Join](#2-left-outer-join)
  - [3. Right Outer Join](#3-right-outer-join)
  - [4. Full Outer Join](#4-full-outer-join)
  - [5. Cross Join](#5-cross-join)
  - [Summary of Joins in LINQ](#summary-of-joins-in-linq)
- [Advantages of LINQ](#advantages-of-linq)

----


## Key Concepts

1. **Unified Syntax**
   - LINQ allows querying different data sources with a unified syntax, regardless of whether the source is an array, list, database, or XML.

2. **Query Expressions**
   - LINQ uses query expressions, which are similar to SQL but are written directly in C# or VB.NET.

3. **Deferred Execution**
   - Queries are not executed until the results are enumerated, providing performance benefits.

4. **Extension Methods**
   - LINQ works through extension methods like `Where`, `Select`, and `OrderBy` that are defined in the `System.Linq` namespace.

5. **Types of LINQ**
   - LINQ can work with various data sources:
     - **LINQ to Objects**: Queries collections like arrays, lists, etc.
     - **LINQ to SQL**: Queries relational databases.
     - **LINQ to XML**: Queries XML documents.
     - **LINQ to Entities**: Works with Entity Framework for database querying.

---

## LINQ Query Syntax

LINQ provides two ways to write queries: **Query Syntax** and **Method Syntax**.

### 1. Query Syntax

```csharp
int[] numbers = { 1, 2, 3, 4, 5, 6 };

// LINQ query using query syntax
var evenNumbers = from num in numbers
                  where num % 2 == 0
                  select num;

// Iterating through the result
foreach (var num in evenNumbers)
{
    Console.WriteLine(num);
}
```

### 2. Method Syntax

```csharp
int[] numbers = { 1, 2, 3, 4, 5, 6 };

// LINQ query using method syntax
var evenNumbers = numbers.Where(num => num % 2 == 0);

// Iterating through the result
foreach (var num in evenNumbers)
{
    Console.WriteLine(num);
}
```

---

## Common LINQ Methods

| Method             | Description                                  |
|--------------------|----------------------------------------------|
| **Where**          | Filters elements based on a condition.      |
| **Select**         | Projects each element into a new form.      |
| **OrderBy**        | Sorts elements in ascending order.          |
| **OrderByDescending** | Sorts elements in descending order.       |
| **GroupBy**        | Groups elements that share a common key.    |
| **Join**           | Joins two sequences based on matching keys. |
| **First**          | Returns the first element of a sequence.    |
| **FirstOrDefault** | Returns the first element or a default value. |
| **Any**            | Checks if any element matches a condition.  |
| **All**            | Checks if all elements match a condition.   |
| **Count**          | Counts the number of elements.              |

---

## Example: LINQ with a List

```csharp
List<string> names = new List<string> { "Alice", "Bob", "Charlie", "David" };

// Query to find names that start with 'A'
var result = names.Where(name => name.StartsWith("A")).ToList();

foreach (var name in result)
{
    Console.WriteLine(name);
}
```

---

## LINQ to SQL

**Description**: LINQ to SQL is used to query relational databases directly using LINQ. It works with a database mapped to .NET classes through an Object Relational Mapper (ORM).

### Example

```csharp
using System;
using System.Linq;

class Program
{
    static void Main()
    {
        using (var db = new DataContext("YourConnectionString"))
        {
            // Assume there is a table named "Customers" in the database
            var customers = from customer in db.GetTable<Customer>()
                            where customer.City == "Ahmedabad"
                            select customer;

            foreach (var customer in customers)
            {
                Console.WriteLine($"{customer.Name} - {customer.City}");
            }
        }
    }
}

// Model for the Customer table
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string City { get; set; }
}
```

---

## LINQ to XML

**Description**: LINQ to XML allows querying and manipulating XML documents using LINQ.

### Example

```csharp
using System;
using System.Linq;
using System.Xml.Linq;

class Program
{
    static void Main()
    {
        // XML data
        string xmlData = @"
        <Customers>
            <Customer>
                <Name>Alice</Name>
                <City>Ahmedabad</City>
            </Customer>
            <Customer>
                <Name>Bob</Name>
                <City>Delhi</City>
            </Customer>
        </Customers>";

        // Load XML
        XDocument doc = XDocument.Parse(xmlData);

        // Query XML
        var customers = from customer in doc.Descendants("Customer")
                        where customer.Element("City").Value == "Ahmedabad"
                        select new
                        {
                            Name = customer.Element("Name").Value,
                            City = customer.Element("City").Value
                        };

        foreach (var customer in customers)
        {
            Console.WriteLine($"{customer.Name} - {customer.City}");
        }
    }
}
```

---

## LINQ to Entities

**Description**: LINQ to Entities works with the Entity Framework to query a database via strongly-typed entity classes.

### Example

```csharp
using System;
using System.Linq;
using System.Data.Entity;

class Program
{
    static void Main()
    {
        using (var context = new MyDbContext())
        {
            // Query to fetch customers from Ahmedabad
            var customers = from customer in context.Customers
                            where customer.City == "Ahmedabad"
                            select customer;

            foreach (var customer in customers)
            {
                Console.WriteLine($"{customer.Name} - {customer.City}");
            }
        }
    }
}

// DbContext class
public class MyDbContext : DbContext
{
    public DbSet<Customer> Customers { get; set; }
}

// Entity class
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string City { get; set; }
}
```

## LINQ Joins in C#

LINQ (Language Integrated Query) supports various types of joins, allowing you to combine data from multiple collections or data sources. This document provides examples and explanations for the following join types:

1. **Inner Join**
2. **Left Outer Join**
3. **Right Outer Join** (Simulated)
4. **Full Outer Join** (Simulated)
5. **Cross Join**

### 1. Inner Join
Combines records from two sequences where there is a matching key in both.

#### Example:
```csharp
var innerJoin = from customer in customers
                join order in orders on customer.Id equals order.CustomerId
                select new { customer.Name, order.OrderDate };

foreach (var item in innerJoin)
{
    Console.WriteLine($"{item.Name} placed an order on {item.OrderDate}");
}
```

### 2. Left Outer Join
Includes all records from the left sequence and matches from the right sequence. If there’s no match, the result includes `null` for the right-side elements.

#### Example:
```csharp
var leftJoin = from customer in customers
               join order in orders on customer.Id equals order.CustomerId into customerOrders
               from order in customerOrders.DefaultIfEmpty() // Ensures null when no match
               select new { customer.Name, OrderDate = order?.OrderDate };

foreach (var item in leftJoin)
{
    Console.WriteLine($"{item.Name} placed an order on {item.OrderDate ?? "No Orders"}");
}
```

### 3. Right Outer Join
LINQ does not natively support right joins, but you can simulate them by reversing the left join logic.

#### Example:
```csharp
var rightJoin = from order in orders
                join customer in customers on order.CustomerId equals customer.Id into orderCustomers
                from customer in orderCustomers.DefaultIfEmpty()
                select new { OrderDate = order.OrderDate, CustomerName = customer?.Name };

foreach (var item in rightJoin)
{
    Console.WriteLine($"Order on {item.OrderDate} by {item.CustomerName ?? "Unknown Customer"}");
}
```

### 4. Full Outer Join
Includes all records from both sequences. If no match is found, null values are included for non-matching elements. LINQ requires a combination of left and right joins to simulate a full outer join.

#### Example:
```csharp
var fullOuterJoin = 
    (from customer in customers
     join order in orders on customer.Id equals order.CustomerId into customerOrders
     from order in customerOrders.DefaultIfEmpty()
     select new { customer.Name, OrderDate = order?.OrderDate })
    .Union(
     from order in orders
     join customer in customers on order.CustomerId equals customer.Id into orderCustomers
     from customer in orderCustomers.DefaultIfEmpty()
     select new { Name = customer?.Name, OrderDate = order.OrderDate });

foreach (var item in fullOuterJoin)
{
    Console.WriteLine($"{item.Name ?? "Unknown Customer"} - {item.OrderDate ?? "No Order"}");
}
```

### 5. Cross Join
Returns the Cartesian product of two sequences. LINQ doesn't have a direct cross-join operator, but you can achieve it using `from ... in` syntax.

#### Example:
```csharp
var crossJoin = from customer in customers
                from order in orders
                select new { customer.Name, order.OrderDate };

foreach (var item in crossJoin)
{
    Console.WriteLine($"{item.Name} - {item.OrderDate}");
}
```

### Summary of Joins in LINQ

| Join Type       | LINQ Support | Special Notes                      |
|-----------------|--------------|------------------------------------|
| Inner Join      | Directly supported | `join ... on ... equals ...`      |
| Left Outer Join | Supported    | Use `DefaultIfEmpty()`             |
| Right Outer Join| Simulated    | Reverse the left join logic        |
| Full Outer Join | Simulated    | Combine left join and right join with `Union` |
| Cross Join      | Supported    | Use `from ... in` for Cartesian product |


---

## Advantages of LINQ

1. **Ease of Use**: Simplifies querying with a SQL-like syntax.
2. **Compile-Time Checking**: Ensures queries are type-safe.
3. **Readability**: Provides a consistent syntax across different data sources.
4. **Productivity**: Reduces the amount of code needed for data operations.
