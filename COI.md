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
  attr_reader :width, :height
  
  def initialize width, height
    @width = width
    @height = height
  end
  
  def width= value
    @width = value
  end
  
  def height= value
    @height = value
  end
end


class Square < Rectangle
  def width= value
    @width = value
    @height = value
  end
  
  def height= value
    @height = value
    @width = value
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

**Inheritance**

```ruby
class Vehicle
  def run
    # Run 40km/h
  end
end

class Car < Vehicle
end

myCar = Car.new
myCar.run
```

**Composition**

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

You can **change** the behavior of a class on **run-time** using composition and dependency injection.
This will be very much useful when doing unit **tests**.

## Close Fight

**Inheritance**
```ruby
class PhysicsObject
  def update
    # update physics
  end
end

# Enemy is a PhysicsObject
class Enemy < PhysicsObject
  def initialize
    @health = 100
  end
end
```

**Composition**
```ruby
class PhysicsEngine
  def update
    # update physics
  end
end

# Enemy uses a PhysicsEngine
class Enemy
  def initialize
    @physicsEngine = PhysicsEngine.new
    @health = 100
  end
  
  def update_physics
    @physicsEngine.update
  end
end
```

1. Inheritance makes sense? Yes.
2. Composition makes sense? Yes.
3. What to choose? WTF.



### The Twin Enemy

- Two blocks conected by an energy chain.
- Single health bar.
- Blocks can move separately and have their own Physics.

![Twin Enemy](http://www.proun-game.com/Oogst3D/BLOG/Inheritance%20versus%20composition%20-%20Enemy.gif)


```ruby
class Enemy

  def initialize
    @health = 100
    @physicsA = PhysicsEngine.new
    @physicsB = PhysicsEngine.new
  end
  
  
  def update_physics
    @physicsA.update
    @physicsB.update
  end

end

```

## Rails STI (Single Table Inheritance)

```ruby
#
# name        :string
# email       :string
# password    :string
# industry    :string
# type        :string
#
class User < ActiveRecord::Base
end


class Business < User
end
```

- `name`, `email`, and `password` are common between `User` and `Business` models.
- `industry` attribute is for `Business` instances only.
- `type` is used by Rails to evaluate in which model the record belongs.


### Weird Side-effects

- Normal users will have `nil` `industry` column.
- `User` objects will have an `industry` and `industry=` methods.


### Composition Approach

```ruby
#
# name                  :string
# email                 :string
# password              :string
# account_holder_id     :integer
# account_holder_type   :string
#
class Account < ActiveRecord::Base
  belongs_to :account_holder, :polymorhpic => true
end


class User < ActiveRecord::Base
  has_one :account, :as => :account_holder
end


#
# industry              :string
#
class Business < ActiveRecord::Base
  has_one :account, :as => :account_holder
end
```


### Composition Drawback


```ruby

class User < ActiveRecord::Base
  
  has_one :account, :as => :account_holder
  
  # Delegate
  delegate :name, :to => :account


  # Wrapper method
  def email
    self.account.email
  end
end


user = User.first

# Directly access using the `Account` object
user.account.password
```


## Answers

1. Why favor Composition over Inheritance?
  - Composition gives more flexibility on a class.
  - You can change the behavior of a class during run-time using composition and dependency injection.
  - Single Table Inheritance in Rails have weird side-effects (on some situations).

2. When to choose Inheritance?
  - A class wants to use/expose all public methods of another class.
  - A sub-class doesn't change the behavior and definitions of the super class.
  - If inherting will pass the Liskov Substitution Principle.


## Final Words

- Inheritance is NOT bad and Composition is not always the correct approach.
- Inheritance is a tighter relationship compared to Composition, so think twice before using it.

## Sources
- http://en.wikipedia.org/wiki/Liskov_substitution_principle
- http://stackoverflow.com/questions/49002/prefer-composition-over-inheritance
- http://en.wikipedia.org/wiki/Composition_over_inheritance
- http://en.wikipedia.org/wiki/Circle-ellipse_problem
- http://joostdevblog.blogspot.com/2014/07/why-composition-is-often-better-than.html
