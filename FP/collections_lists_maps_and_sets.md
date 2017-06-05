# Collections: Lists, maps and sets

> HR Chapter 5

## Lists

### `List.map`

It applies the function *f* to each element in a list.

#### Signature of `List.map`

```fsharp
List.map : ('a -> 'b) -> 'a list -> 'b list
```

#### Example of `List.map`

Add the ".fs" extension to every string in a list:

```fsharp
List.map (fun s -> s + ".fs)
```

### `List.exists`

Returns true if a condition is true for one of the elements in the list.

#### Signature of `List.exists`

```fsharp
List.exists : ('a -> bool) -> 'a list -> bool
```

#### Example of `List.exists`

Returns true if any element in the (integer) list is equal to 2.

```fsharp
List.exists (fun s -> s = 2) [1; 3; 4] // False
```

### `List.forall`

Returns true if a condition is true for all of the elements in the list.

#### Signature of `List.forall`

```fsharp
List.forall : ('a -> bool) -> 'a list -> bool
```

#### Example of `List.forall`

Returns true if all elements in the (integer) list is equal to 2.

```fsharp
List.forall (fun s -> s = 2) [1; 3; 4] // False
```

### `List.tryFind`

Is like `List.exists`, but returns an `Option` type. In other words,
`None` if no match were found, `Some` otherwise.

#### Signature of `List.tryFind`

```fsharp
List.tryFind : ('a -> bool) -> 'a list -> 'a option
```

#### Example of `List.tryFind`

Returns `Some s` if any element in the (integer) list is equal to 2, `None` otherwise.

```fsharp
List.tryFind (fun s -> s = 2) [1; 3; 4] // None
```

### `List.filter`

Returns a list of elements for which the given condition is true

#### Signature of `List.filter`

```fsharp
List.filter : ('a -> bool) -> 'a list -> 'a list
```

#### Example of `List.filter`

Returns a new list of all the elements that are equal to 2:

```fsharp
List.filter (fun s -> s = 2) [1; 3; 4] // []
```

### `List.fold`

is a lot like *reduce*. The difference is that *fold* takes an explicit initial value whereas *reduce* always uses the first element of a list.

#### Signature of `List.fold`

```fsharp
List.fold : ('a -> 'b -> 'a) -> 'a -> 'b list -> 'a
```

#### Example of `List.fold`

Adds all the elements together.
*0* is the initival value. In this case, the offset of the sum.
So, if we were to take the sum of a list of numbers and add it to the sum,
we could just change *0* to the value of the sum.

```fsharp
List.fold (fun prev curr -> prev + curr) 0 [1; 2; 3;]
```

Here's the example with a different starting value:

```fsharp
let moneyInAccount = 1000
let transfersFromFriends = [100, 200, 100]

// Add the payments to the money in account
List.fold (fun prev curr -> prev + curr) moneyInAccount transfersFromFriends
```

### `List.foldBack`

Is like `List.fold`, **but the list elements are accumulated in the opposite order**.

## Sets

You construct sets with the `set` function and provide it with a list of starting values:

```fsharp
set [1; 2; 1;]
val it : Set<int> = set [1; 2]
```

You can of course also go from `Set` to `List` with the `Set.toList` function.

### Equality of sets

As expected, two sets are equal if their contents are the same.

Otherwise, `Set` has all the expected functions (`add`, `contains`, `remove`). I won't go into detail here since I understand these already.

What makes F# sets neat is that they have many of the same `List` functions such as *fold*, *forall*, etc.

## Maps

The easiest way to construct a `Map` is from a list with the `Map.ofList` function. Otherwise, like `Set`, most of what the `Map` type offers doesn't require much explanation for my own learning purposes.