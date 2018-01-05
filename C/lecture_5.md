# Lecture 5

> Slides

## Declarations and Definitions

### Definition

A *Definition* specifies what a function *does* or *where* a variable is stored.

### Declaration

A *Declaration* describes the type of a variable or a function as well as its name. **No space is allocated**.

**Variables and functions are *defined* exactly once, but may be declared several times**!

## Scope of variables

A Variable defined in a function is local to that function (e.g. it is a local variable).

### Automatic variables

An *automatic variable* is a local variable.

### Global variables

*External* variables are ones that are defined outside *any* function (e.g. in the global scope of the file).

It is also implicitly accessible from within other files/compilation units.

### Static variables

Global variables prefixed with the `static` prefix is *private*, e.g. only accessible from within the current file (or compilation unit, `.c` and `.h` file).

### External variables

This is global variables (e.g. defined in the global scope of a file) that is marked with an `extern` prefix.

Making a global variable external means that it is global across all files/compilation units and can be accessed from anywhere.

You have to *define* the variable somewhere and *then* mark it with `extern`

### Static automatic/local variables

For a local variable annotated with the `static` keyword, it is a bit different. Here it means that **it can retain its value across calls to a function**. So, no matter how often you invoke the function, whatever the value of the variable is/was, it will persist.

**The keywords `static` and `extern` are mutually exclusive**!

### External functions

If a function declaration is marked as `extern`, it has global scope and doesn't need to be marked later on as `extern`.

## Restrictions and resolutions

- A function ***cannot*** return a function.
  - e.g. `foo()()`
- A function ***cannot*** return an array
  - e.g. `foo()[]`
- An array ***cannot*** be invoked
  - e.g. `foo[]()`

BUT:

- A function ***can*** return a pointer to a function.
  - e.g. `(*fun())()`
- A function ***can*** return a pointer to an array
  - e.g. `(*fun())[]`
- A array ***can*** include function pointers
  - e.g. `(*foo[])()`

## Type specifier

- `char`, `int`, `short`, `long`, `float`, `double`, `struct`, `union`, `enum`
- `signed`, `unsigned`
- Pointer: `*`
- Array: `[]`

## Difference between `#define` and variables

The `#define` compiler hint allows for read-only compile-time declarations of things:

``#define PI 3.14`

This is practical because:

- You can even define expressions update:
  - e.g. `#define MIN(x,y) ((x) < (y)) ? (x) : (y)`
- They are just macros. Each time a reference to a `#define` appears within the file, it will be replaced by its' raw value at compile-time and thus doesn't take up any space in memory.
- Of this reason, they are also immutable. They don't live at run-time like variables do.

## `struct`

A `struct` is a bunch of data items grouped together in memory:

```c
struct tag {
	type_1 identifier_1;
	type_2 identifier_2;
	// ...
	type_N identifier N;
}

struct tag variable_name;
```

Data items in a `struct` are accessed through the `.` operator.

When using a pointer to a struct, the data items dereferenced through the pointer are accessed through the `->` operator.

### What a `struct` can contain

It can have bit fields, unnamed fields and word-aligned fields:

```c
struct pid_tag {
	unsigned int inactive: 1;
	unsigned int: 1; // 1 bit of padding
	unsigned int refcount: 6;
	unsigned int: 0; // pad to int length
	short pid_id;
	struct pid_tag *link;
};
```

For example:

```c
struct Age {
	unsigned int age: 3;
};

int main () {
	Age.age = 4;
	return 0;
}
```

## `union`

A `union` is similar in appearance to a `struct`, but has crucial difference in the memory layout!

Instead of each member being stored after the end of the previous one, **all the members have an offset of zero**.

This means that the storage for the individual members is overlaid: **only one member at a time can be stored there**.

## `enum`

An `enum` is simply a way of associating a series of names with a series of integer values:

```c
enum SizeKind {
	SMALL = 7,
	MEDIUM,
	LARGE = 10,
	HUMUNGOUS
};
```

## `const` type qualifier

The modifier `const` qualifies a read-only variable. This is one that cannot be a `lvalue` in an assignment following the variable declaration.

### Combining `const` and `*`

This is usually only used to simulate call-by-value for array parameters. It says *"I am giving you a pointer to this thing, but you may not change it"*

## `volatile` type qualifier

If a variable has a `volatile` qualifier, it may be modified outside the program.

For example, a *register* that can be modified by a device can be tested/read repeatedly by a program that never modifies it directly.

Assigning a volatile object to a pointer results in undefined behavior.

## `void *`

`void *` defines a pointer to **data of unspecified type**

## Type Conversions

Such conversions can be *explicit* or *implicit*

### Explicit Type Conversions

This is where a value of one type is explicitly cast to another type.

### Implicit Type Conversions

This happens when:

1. A value of one type is assigned to a variable of a different type.
2. An operator converts the type of its operands
3. A value is passed as argument to a function or when a value is returned from a function

## Unsigned and Signed

At the bit level, they have the same representation, but they are interpreted differently depending on the signed/unsigned declaration.

**If there is a mix of unsigned and signed in a single expression, signed values are implicitly cast to `unsigned`**.

## Pointer Conversions

A pointer to one type of value can be converted to a pointer to a different type.

Youy can convert a pointer to an object to a pointer to another object **whose type requires less or equally strict storage alignment**.

A pointer to `void` can be converted to or from a pointer to *any* type, without restriction or loss of information!

## Different meanings of `void`

As the return type of a function, it means that the function doesn't return a value.

In a pointer declaration, it means that the type of the pointer is generic (e.g. `any`).

In a parameter list, it means that something takes no parameters.