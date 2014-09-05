# Composition Over Inheritance

## Questions

- Why favor composition over inheritance?
- When to choose inheritance?

## Definitions

- **Composition** - *"has a"* or *"uses a"* relationship
  - `Car` has an `Engine`
  - `Person` has a `Name`

- **Inheritance** - *"is a"* relationship
  - `Car` is a `Vehicle`
  - `Person` is a `Mammal`


## What to use?

### Inheritance?

- Does `Type B` wants to expose the complete interface *(all public methods)* of `Type A`, such that `Type B` can be used anywhere `Type A` is expected?

A `Biplane` will expose the complete interface of an `Airplane`. So, it makes perfect sense to derive it from `Airplane`.

### Composition?
- Does `Type B` only wants some of the behavior *(public methods)* of `Type A`?

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

## Composition and Dependency Injection

Inheritance Approach:

```ruby
class Runnable 
  def run
    # Run 40km/h
  end
end

class Car < Runnable
end

myCar = Car.new
myCar.run
```

Composition Approach:

```ruby
class Engine
  def start
    # Run 40km/h
  end
end

class Car
  def initialize engine
    @engine = engine
  end
  
  
  def run
    @engine.start
  end
end
```

You can change the behavior of a class on run-time using composition and dependency injection.
This will be very much useful when doing unit test.




## Both Make Sense?

```ruby
class PhysicsObject
  def update
    # Update physics
  end
end


# Enemy is a PhysicsObject
class Enemy < PhysicsObject
  def initialize
    @health = 100
  end
end


# Enemy uses a PhysicsObject
class Enemy
  def initialize
    @physicsObject = PhysicsObject.new
    @health = 100
  end
  
  def update_physics
    @physicsObject.update
  end
end
```

Inheritance is more intuitive?

### The Twin Enemy

- Two blocks conected by an energy chain.
- Single health bar.
- Blocks can move separately and have their own Physics.

![Twin Enemy](http://www.proun-game.com/Oogst3D/BLOG/Inheritance%20versus%20composition%20-%20Enemy.gif)


```ruby
class Enemy

  def initialize
    @health = 100
    @physicsA = PhysicsObject.new
    @physicsB = PhysicsObject.new
  end
  
  
  def update_physics
    @physicsA.update
    @physicsB.update
  end

end

```




## Summary

Inheritance is NOT bad and Composition is not always the correct approach.
If you are going to use Inheritance -- think about Liskov Substitution Principle.

## Sources
- http://en.wikipedia.org/wiki/Liskov_substitution_principle
- http://stackoverflow.com/questions/49002/prefer-composition-over-inheritance
- http://en.wikipedia.org/wiki/Composition_over_inheritance
- http://en.wikipedia.org/wiki/Circle-ellipse_problem
- http://joostdevblog.blogspot.com/2014/07/why-composition-is-often-better-than.html
