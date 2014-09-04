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

## Composition or Inheritance

### Inheritance
- Does `Type B` wants to expose the complete interface *(all public methods)* of `Type A`, such that `Type B` can be used anywhere `Type A` is expected?

A `Biplane` will expose the complete interface of an `Airplane`. So, it makes perfect sense to derive it from `Airplane`.

### Composition
- Does `Type B` wants only some of the behavior *(public methods)* of `Type A`?

A `Bird` may only need the `fly` behavior of an `Airplane`. In this case, it makes sense to extract it out and make it a member of both classes.

## Liskov Substitution Principle

  If `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` without altering
  any of the desirable properties of that program *(correctness, task performed, etc.)*

### Square-Rectangle Problem

A `Square` class that derives from a `Rectangle` class, assuming getter and setter methods exist for both `width` and `height`. The `Square` class **always** assumes that the `width` is equal with the `height`. If a `Square` object is used in a context where a `Rectangle` is expected, unexpected behavior may occur because the dimensions of a `Square` cannot (or rather should not) be modified independently.

```ruby
class Rectangle
  attr_accessor :width, :height
  
  def initialize width, height
    self.width = width
    self.height = height
  end
  
  def doubleWidth
    self.width *= 2 
  end
end


class Square < Rectangle
  def doubleWidth
    self.width *= 2
    self.height = self.width # also manipulates height
  end
end

```


### Prisoner-Person Problem

```ruby
class Person

  def walkNorth
    # walks north
  end
  
  
  def walkSouth
    # walks south
  end
end


class Prisoner < Person
  def walkNorth
    # not allowed
  end
  
  def walkSouth
    # not allowed
  end
end
```

`Prisoner` is not allowed to walk in any direction, but the contract of the `Person` class states that a `Person` can.

This strongly suggests that inheritance should never be used when the **sub-class restricts the freedom implicit in the base class**, but should only be used when the **sub-class adds extra detail to the concept represented by the base class** as in `Monkey` is an `Animal`.

## Mixin is an Inheritance Hack


## Sources
- http://en.wikipedia.org/wiki/Liskov_substitution_principle
- http://stackoverflow.com/questions/49002/prefer-composition-over-inheritance
- http://en.wikipedia.org/wiki/Composition_over_inheritance
- http://en.wikipedia.org/wiki/Circle-ellipse_problem
