# Data Access & Entity Framework
> [PrC#6]: chapters 37 and 38.

## Lecture notes

#### project.json dependencies:
```json
"dependencies": {
	"System.Data.Common": "4.1.0",
	"System.Data.SqlClient": "4.1.0",
	"Microsoft.EntityFrameworkCore": "1.0.1",
	"Microsoft.EntityFrameworkCore.Sqlite": "1.0.1",
	"Microsoft.EntityFrameworkCore.SqlServer": "1.0.1",
	"Microsoft.EntityFrameworkCore.InMemory": "1.0.1"
}
```

#### `readonly` keyword
Property is only settable inline or from the constructor. You cannot assign a setter to it.

#### `??` operator
Take a look at the following code:
```csharp
return age ?? 12;
```

It says *Return age if it is not null, otherwise return 12*. This is nice! Its basically shorthand for:
```csharp
return age != null ? age : 12
```

### SQL in C#

### Connection string
A common way to connect to a database. Agnostic. Could be any server. For instance:
```json
{
	"ConnectionStrings": {
		"SqlServer": "Server=tcp:foo.bar.baz,1433;",
		"Sqlite": "Filename=./mydb.db"
	}
}
```

#### Import statements for SQL
```csharp
using System;
using System.Data;
using System.Data.SqlClient;
```

#### Fields
```csharp
SqlConnection connection;
```

#### Open connection.
Make sure to call `connection.Open()` before being able to execute any SQL.

### Parameterized SQL
Given a query string like the following:
```csharp
var query = "SELECT Id FROM Characters WHERE Id = @Id"
```
You can exchange the `@Id` part (like template) strings.
Say you have a `command` which you got from `connection.CreateCommand()`. You can now write `command.Parameters.AddWithValue("@Id", Id)` where `Id` is whatever you want to replace it with.

This is obviously to escape SQL Injection. The `AddWithValue` method takes care of properly escaping whatever input it is given and fails hard if it has unwanted side effects.

#### Executing SQL
Get a reader from the `command`: `command.ExecuteReader()`. You'll get an Iterator.
When you call `reader.Read()`, you move the internal cursor one position. So, even for reading a single entry, you still have to call `reader.Read()` at least once. The `Read()` method returns a `boolean`. So, you could wrap it in a conditional block:
```csharp
if (reader.Read()) {
	// Do stuff with the query results.
}
```

You could obviously wrap it in a `while` block instead for multiple results and then `yield` return the results.

The results will all be of type `any`, you have to cast them like this:
```csharp
var Id = reader.Id as int;
var Name = reader.Name as string;
var Age = reader.Age as int? // Nullable type.
```

#### Entity Framework
The Entity Framework makes working with databases easier by automating database related activities.

It is an Object/Relational mapping (ORM) framework that enables developers to work with relational data as domain-specific objects, eliminating the need for most of the data access plumbing code that developers usually need to write.
