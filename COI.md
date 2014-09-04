# Composition Over Inheritance

## Argument
It is **easier to modify** a class through time if it favors composition over inheritance.

## Composition
- *"has a"* or *"uses a"* relationship

Example:
- `Car` has an `Engine`
- `Person` has a `Name`

## Inheritance
- *"is a"* relationship

Example:
- `Car` is a `Vehicle`
- `Person` is a `Mammal`


## Liskov Substitution Principle

  If `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` without altering
  any of the desirable properties of that program *(correctness, task performed, etc.)*
