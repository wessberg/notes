# (C#) Data Access and Entity Framework (+ ADO.NET)
> [PrC#6]: chapters 37 and 38

## ADO.NET
Enables you to access a relational database like SQL server from a C# program.

## Database Connections (Connection strings)
You need to provide connection parameters such as host machine, port and login credentials. This is done through something called a *connection string*:
```csharp
string connectionString = @"server=(localdb)\MSSQLLocalDB;integrated security=SSPI;database=NameOfDataBase";
```

You can use the `SqlConnection` class to connect to SQL Server.

## Managing Connection strings
It would not be very unwise to simply hardcode the connection string inside the code (where it would also be stored in VCS with git or something like it).

Instead, it should be read from a configuration file outside version control or from environmental variables.
You could add it to a JSON file `config.json`, for instance:
```json
{
	"Data": {
		"DefaultConnection": {
			"ConnectionString": "Server(localdb)\\MSSQLLocalDB;Database=NameOfDatabase;Trusted_Connection=True;"
		}
	}
}
```

You could then load it inside your code with the `ConfigurationBuilder` class and its method `AddJsonFile`.

## Commands
After establishing a connection, you can execute commands like this:
```csharp
using (var connection = new SQLConnection(GetConnectionString()))
{
	string sql = "SELECT Something FROM SomeOtherThing";
	var command = new SqlCommand(sql, connection);

	connection.Open();
}
```

But, database queries often need parameters. Don't use string interpolation, as it could be prone to SQL injection.
Instead, use ADO.NET's parameter features:
```csharp
string sql = "SELECT something FROM SomeOtherThing WHERE something.x = @SomeVariable";
var command = new SqlCommand(sql, connection);
command.Parameters.AddWithValue("SomeVariable", 123);
```

### Executing a query
There are 3 options:
-  `ExecuteNonQuery`: Executes the command but only returns the amount of affected rows. Useful for `UPDATE`, `INSERT` or `DELETE` statements.

- `ExecuteReader`: Executes the command and returns a typed `IDataReader`.

- `ExecuteScalar`: Executes the command and returns the value from the first column of the first row of any result set.

#### `ExecuteNonQuery`
Returns an `int` indicating the amount of records affected:
```csharp
int records = command.ExecuteNonQuery();
```

#### `ExecuteScalar`
Useful for when you only really want to know, say, the count of records in a table, or something like that. Where you want whatever results you get back to reduced/flattened to a single value.
For instance:
```csharp
string sql = "SELECT COUNT(*) FROM Something";
SqlCommand command = connection.CreateCommand();
command.CommandText = sql;
connection.Open();

object count = command.ExecuteScalar();
```

The returned object can then be cast however you like.

**Basically, `ExecuteScalar` should be used whenever the SQL you are calling returns only one column.**

#### `ExecuteReader`
Executes the command and then returns a data reader object. In the case of a `SqlCommand`, a `SqlDataReader` is returned.

Such an object can be used to iterate through the record(s) returned. It is an iterator with a `Read` method which, for each time it is invoked, moves the cursor th the next record. It also means that you need to call `Read` as the first thing to move the cursor to the first record.

When there is no records, the `Read` method will return `false`.

This makes it convenient to place inside a while loop:
```csharp
using (SqlDataReader reader = command.ExecuteReader(CommandBehavior.CloseConnection))
{
	while (reader.Read())
	{
		int id = reader.GetInt32(0);
		string name = reader.GetString(1);
		DateTime from = reader.GetDateTime(4);
		// And so on...
	}
}
```

## Asynchronous Data Access
You should not block the user interface while accessing the database. Well, actually, any network operation.

You can use `SqlConnection.OpenAsync`, `ExecuteReaderAsync` and `SqlDataReader.ReadAsync`:
```csharp
public static async Task ReadAsync (int productId)
{
	var connection = new SqlConnection(GetConnectionString());
	string sql = "SELECT product FROM Products WHERE product.Id = @ProductId";

	var command = new SqlCommand(sql, connection);
	command.Parameters.AddWithValue("ProductId", productId);

	await connection.OpenAsync();

	using (SQLDataReader reader = await command.ExecuteReaderAsync(CommandBehavior.CloseConnection))
	{
		while (await reader.ReadAsync())
		{
			int id = reader.GetInt32(0);
			string name = reader.GetString(1);
			// And so on...
		}
	}
}
```

## Transactions
Transactions are based on the ACID properties we know and love.

By default, a single command is running within a single transaction.

But, when you care about transactions, you typically care about performing multiple actions and respecting (especially) the atomicity principle.

So, to start a transaction, invoke the `BeginTransaction` method of the `SqlConnection`. That returns a `SqlTransaction`:
```csharp
await connection.OpenAsync();
SQLTransaction tx = connection.BeginTransaction();
```

As you perform commands inside that connection, make sure to set their `Transaction` to the one you stored in a variable (in the example above it was called `tx`):
```csharp
command.Transaction = tx;
```

When you're done, you can call `tx.Commit()` to finally commit the changes. If you want to *abort*, you use the `tx.Rollback()` method (apparently Microsoft wants to use their own terminology for aborting Transactions).

# Entity Framework (Core)
**The Entity Framework offers mapping of entities to relationships.**

With it, you can create types that map to database tables as well as create database queries using LINQ, create and update objects and write them to the database.

### Code First
This is the supported behavior. It does *not* mean that you should define the database purely by code first - it just means that you should *either* create the database first *or* define the database purely from code.

## Introducing Entity Framework
It is pretty neat.
So lets say we had a `Book` table in our SQL database:
```sql
CREATE TABLE Books
(
	BookId INT NOT NULL PRIMARY KEY,
	Title NVARCHAR(50) NOT NULL,
	Publisher NVARCHAR(25) NOT NULL
)
```

We could then write a `Book` class which would be a simple entity type with the same properties. This could also be called `EFBook` since it is the "model" of the EntityFramework.
```csharp
[Table("Books")]
public class Book
{
	public int BookId { get; set; }
	public string Title { get; set; }
	public string Publisher { get; set; }
}
```
The `[Table("Books")]` attribute then maps the class to the table *Books*.

### Creating a context
To actually associate the `Book` class with the `Books` table, **you need to provide a `BooksContext` class.**

This class will derive from the base class `DbContext`.

You'd then define the `Books` property that is of type `DbSet<Book>`.

Now you create queries and add `Book` instances for storing it in the database.

To give it a connection string, the `OnConfiguring` method of the `DbContext` can be overridden.

```csharp
public class BooksContext: DbContext
{
	public DbSet<Book> Books { get; set; }

	protected override void OnConfiguring (DBContextOptionsBuilder optionsBuilder)
	{
		base.OnConfiguring(optionsBuilder);
		optionsBuilder.UseSqlServer(GetConnectionString());
	}
}
```

### Writing to the Database
You create a context object for the table you want to alter. Then you create an instance of the class, for instance `Book` and then `Add` it to the context. To commit the change, invoke the `SaveChangesAsync` method on the context.

Let's write a method for adding `Book`s:
```csharp
private async Task AddBookAsync (string title, string publisher)
{
	using (var context = new BooksContext())
	{
		var book = new Book { Title = title, Publisher = publisher};

		context.Add(book);
		await context.SaveChangesAsync();
	}
}
```

### Reading from the Database
Easy enough. Just create an instance of the context you want to read from and then access the stuff you want.
For instance:
```csharp
private void ReadBooks ()
{
	using (var context = new BooksContext())
	{
		var books = context.Books;
		// Do something.
	}
}
```

### Using LINQ with the Entity Framework
Very possible indeed!
for instance:
```csharp
private void QueryBooks ()
{
	using (var context = new BooksContext())
	{
		var books = from b in context.Books
		where b.Publisher = "Walt Disney"
		select b;
	}
}
```
This spawns a SQL statement to the database along these lines:
```sql
SELECT * FROM Books AS b WHERE b.Publisher = "Walt Disney";
```

### Updating records
Just as easy.
Just get the object you want to object via the context, then change it and finally invoke `SaveChangesAsync`.

### Deleting records
Just get the object you want to delete via the context. Then call `Remove` on it and finally invoke `SaveChangesAsync`.

### Disadvantages
Some of these operations could have been made more efficiently without the Entity Framework. For instance, to delete or update something, you first retrieve it from the database and *then* update it. Using ADO.NET without the Entity Framework, you could have done so in a single operation.

## Using Dependency Injection
The Entity Framework supports dependency injection by design.
This could be useful. Instead of defining the connection and the use of SQL Server with the DbContext derived class, the connection and SQL Server could simply be injected by using a dependency injection framework.

This also makes the context classes far simpler.

We could write a context:
```csharp
public class BooksContext: DbContext
{
	public DbSet<Book> Books { get; set; }
}
```

And then *inject* that context inside a `BooksService` (**These are also called *Repositories*.**).

So let's write a repository:
```csharp
public class BooksRepository
{
	private readonly BooksContext _booksContext;

	public BooksRepository (BooksContext context)
	{
		_booksContext = context;
	}

	public async Task AddBookAsync ()
	{
		var book = new Book { Title = "A title", Publisher = "A publisher" };

		_booksContext.Add(book);
		await _booksContext.SaveChangesAsync();
	}

	public async Task ReadBook()
	{
		var books = _booksContext.Books;
		foreach (var b in books) WriteLine($"{b.Title} - {b.Publisher}");
	}
}
```

## Initializing the dependency injection container
The magic happens in the `InitializeServices` method.
Here a `ServiceCollection` instance is created and the `BooksRepository` class is added to the collection **with a transient lifetime management**.

With a transient lifetime, the `ServiceCollection` is instantiated every time the service is requested.

Heres a way to hook it up:
```csharp
private IServiceProvider Container { get; private set; }

private void InitializeServices ()
{
	var services = new ServiceCollection();
	services.addTransient<BooksRepository>();
	services.AddEntityFramework()
		.AddSqlServer()
		.AddDbContext<BooksRepository>(options => options.useSqlServer(ConnectionString));

	Container = services.BuildServiceProvider();
}
```

**This is done from the `Main` method!**.

## Creating a Relation
This is absolute fine. Consider the following the relation between `EFBook` and `EFPublisher`:

```csharp
public class EFBook
{
	public int BookId { get; set; }
	public string Title { get; set; }
	public EFPublisher Publisher { get; set; }
}
```

Here, a Book *has a* Publisher.

For this to work, you must provide a `DbSet` for `Publisher`s also from the `BooksContext`:
```csharp
public class BooksContext: DbContext
{
	// boilerplate stuff
	// ...

	public DbSet<EFBook> Books { get; set; }
	public DbSet<EFPublisher> Publishers { get; set; }

	// More boilerplate stuff
	// ...
}
```

## Migrations
To automatically create the database using C#, you must extend the .NET CLI tools with the "ef tools" (dotnet-ef).

Now you have some commands you didn't have before:

### Create initial Migrations
Run
```shell
dotnet ef migrations add <MigrationName>
```

This will create an initial migration to create the database from code with the name provided in place of "<MigrationName>".

Specifically, the `migrations add` command accesses the `DbContext` derived classes using reflection and in turn the referenced model types.

With it, it creates two classes to create and update the database.

Such a file defines `Up` and `Down` methods (which is a convention when building Migrations).
- The `Up` method lists all the actions that are needed to create the tables, including primary keys, columns and the relation.
- The `Down` method drop the tables.

### Creating more migrations
With every change you're doing, you can create another migration. The new migration *only* defines the changes needed to get from the previous version to the new version.

Obviously, this comes in handy when you reach production.

## Creating/Deleting a Database
All `DbContext`s (and thus also derived classes) contains a `Database` property that returns a `DatabaseFacade` object.

So, you would be able to call `context.Database.EnsureCreatedAsync()` which will create the database if it doesn't exist already. Likewise, you can call `context.Database.EnsureDeletedAsync()`.

You would still need to run migrations manually, though.

## Data Annotations
So, back to the `EF` model classes such as `EFBook`.
You can annotate the properties to influence the generated database.

### The `Schema` property of the `Table` attribute.
To change the schema name, The `Table` attribute defines the `Schema` property:
```csharp
[Table("Books", Schema = "mc")]
public class EFBook
{

}
```

### The `MaxLength` attribute
To specify a different length for a string type, you can apply the `MaxLength` attribute:
```csharp
[MaxLength(120)]
public string Title { get; set; }
```

Obviously this would become a `[N]VARCHAR(120)`.

## Fluent API
You can also influence the tables created with the Fluent API with the `OnModelCreating` method of the `DbContext` derived class.

It allows you to write it in a bit different way:
```csharp
protected override void OnModelCreating (ModelBuilder modelBuilder)
{
	modelBuilder.Entity<EFBook>()
		.Property<int>(b => b.BookId)
		.ValueGeneratedOnAdd();

	modelBuilder.Entity<EFPublisher>()
		.Property<string>(p => p.Name)
		.HasMaxLength(120);

	// And so on
}
```

### Defining one-to-many mappings
You can chain the `HasMany` method together with `WithOne` to define a mapping of many Books with one publisher:
```csharp
protected override void OnModelCreating (ModelBuilder modelBuilder)
{
	modelBuilder.Entity<EFPublisher>()
		.HasMany(b => b.Books)
		.WithOne(p => p.Publisher);

	modelBuilder.Entity<EFBook>()
		.HasOne(p => p.Publisher)
		.WithMany(b => b.Books)
		.HasForeignKey(p => p.PublisherId);
}
```

Now you can create new Migrations.

## Going the other way around and scaffolding a Model from the Database
You can install the NuGet package `EntityFramework.MicrosoftSqlServer.Design` and then use the following command from the prompt:
```shell
dnx ef dbcontext scaffold
"server=(localdb)\MSSQLLocalDb;database=SomeDatabase;trusted_connection=true""EntityFramework.MicrosoftSqlServer"
```

Now the `DbContext` derived classes as well as the model types will be generated. It will be done with the Fluent API by default, but by giving the `-a` option, it will instead use data annotations.

## Conflict Handling (Concurrency)
If multiple users work on the same record, there are different ways to handle conflicts.

There are different strategies:
### Strategy 1: The Last One Wins (default)
This is the default. The last one saving changes will win. This means that if resources are being altered by more than one concurrent SQL transaction, it is the last one that will be kept.

### Strategy 2: The First One Wins
The other way around.

To do it, an EF model class must have a `public byte[] TimeStamp { get; set; }` property.
You can then use the Fluent API:
```csharp
book.Property(b => b.TimeStamp)
	.HasColumnType("timestamp")
	.ValueGeneratedOnAddOrUpdate()
	.IsConcurrencyToken();
```

## Using Transactions
Well, you *can* use the EntityFramework with transactions, just make sure to write specific methods for it. Basically, the idea is to wait with calling `SaveChangesAsync` until all work is done.

## Creating Explicit Transactions
The advantage is to be able to easily roll back (abort) explicitly. Also, you can combine multiple invocations of `SaveChangesAsync` within one transaction.

To do so, invoke the `BeginTransactionAsync` method of the `DatabaseFacade` class that is returned from the `Database` property on any context.

Just like with ADO.NET, you can call `Commit` or `Rollback` on the object returned by the `BeginTransactionAsync` call.

For instance:
```csharp
private static async Task SomeComplexTaskWithTransactions
{
	// Let's say we have a context.
	try {
		using (tx = await _context.Database.BeginTransactionAsync())
		{
			// Do lots of stuff.
			tx.Commit();
		}
	} catch (DbUpdateException ex)
	{
		tx.Rollback();
	}

}
```
