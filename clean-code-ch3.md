# Clean Code: Chapter 3 - Functions

- How can we make a function communicate its intent?
- What attributes can we give our functions that will allow a casual reader to inuit the kind of program they live inside?

## Small!
- First rule of functions is that they should be small.
- The second rule of functions is that **they should be smaller than that**.

## How small?
- Ideally, 2, 3, or 4 lines long.
- Should not ever be 20 lines long.
- Indent level should not be greater than one or two.

### Blocks
- `if`-`else`, `while`, `each`, etc...
- Should be 1 line long.
- Probably a function call.
  - Function call adds documentary value because it can have a descriptive name.

Code 1.0
```ruby
def open?
  now = DateTime.now
  
  active_shifts = 
    self.shifts.select do |shift|
      template_date = DateTime.new 2000, 01, 01

      start_time = template_date.change :hour => shift.start_time.hour, :min => shift.start_time.min
      
      end_time = template_date.change :hour => shift.end_time.hour, :min => shift.end_time.min
      end_time += 1.day if end_time < start_time

      time_now = template_date.change :hour => now.hour, :min => now.min
      time_now += 1.day if time_now < start_time
  
      time_now >= start_time && time_now <= end_time
    end
  
  active_shifts.any?
end
```

Code 1.1
```ruby
def open?
  active_shifts =
    self.shifts.select do |shift|
      self.now_is_within? shift.start_time, shift.end_time
    end
    
  active_shifts.any?
end
```

Code 1.2
```ruby
def open?
  now = DateTime.now

  active_shifts = self.shifts.select { |shift| shift.active_on_time? now }
  active_shifts.any?
end
```

Code 2.0
```ruby
def compute_total_cost
  total_cost = 0
  
  @order_items.each do |order_item|
    total_cost += order_item.price * order_item.quantity
  end

  total_cost
end
```

Code 2.1
```ruby
def compute_total_cost
  total_cost = 0
  
  @order_items.each do |order_item|
    total_cost += self.compute_order_item_cost order_item
  end
  
  total_cost
end
```

Code 2.2
```ruby
def compute_total_cost
  total_cost = 0
  
  @order_items.each do |order_item|
    total_cost += order_item.compute_cost
  end
  
  total_cost
end
```



## Do One Thing

**Functions should do one thing.** 
**They should do it well.**
**They should do it only.**

What is *One Thing*?

## TO Statement

Template: TO `<function name>`, `<statements>` ...

- LOGO language used the keyword `TO` the same way Ruby used `def` to define a function.
- One level of *abstraction* below the name of the function.
- We write functions to decompose a larger concept (name of the function) into a set of steps at the next level of abstraction.
- We can tell if a function is doing more than "one thing" if you can extract another function from it with a name that is not merely a restatement of its implementation.
- Functions that do one thing cannot be reasonably divided into sections.

### Abstraction?

A technique for managing complexity of computer systems.

*For example, a programmer writing code that involves numerical operations may not be interested in the way numbers are represented in the underlying hardware (e.g. whether they're 16 bit or 32 bit integers), and where those details have been suppressed it can be said that they were abstracted away, leaving simply numbers with which the programmer can work.* - [Wikipedia](https://en.wikipedia.org/wiki/Abstraction_%28computer_science%29)

One level of *abstraction* below the name of the function?

Boil an egg:

1. Buy a hen.
1. Wait for it to lay an egg.
1. Get one egg.
1. Place the egg in a saucepan.
1. Add water to saucepan just enough to cover the egg by 1 inch.
1. Heat the pan over high heat just to boiling.
1. Remove from stove.
1. Let the egg stand in hot water for 10 minutes.
1. Drain.


*The essence of abstractions is preserving information that is relevant in a given context, and forgetting information that is irrelevant in that context.* – John V. Guttag

Code 2.0

To compute total cost, we set total cost to 0, then loop through each order items, **get the product of its price and quantity**, and add it to the current total cost, and finally return the total cost.

```ruby
def compute_total_cost
  total_cost = 0
  
  @order_items.each do |order_item|
    total_cost += order_item.price * order_item.quantity
  end

  total_cost
end
```


Code 2.1

To compute total cost, we set total cost to 0, then loop through each order items and add each cost to the current total cost, and finally return the total cost.

```ruby
def compute_total_cost
  total_cost = 0
  
  @order_items.each do |order_item|
    total_cost += self.compute_order_item_cost order_item
  end
  
  total_cost
end
```

## Switch Statement and If-else
- It's hard to make a `switch` statement that does only one thing.
- By their nature, `switch` statement always do *N* things.
- Make sure that each `switch` statement is buried in a low-level class and is never repeated.
- Use Polymorphism.

Code 3.0
```ruby
def validate_order order
  if order.pending?
    validatePendingOrder order
  elsif order.paid?
    validatePaidOrder order
  elsif order.completed?
    validateCompletedOrder order
  end
end

def delete_order order
  if order.pending?
    deletePendingOrder order
  elseif order.paid?
    deletePaidOrder order
  elseif order.completed?
    deleteCompletedOrder order
  end
end
```

Code 3.1

```ruby
class PendingOrderRecord
  
  def validate
    // validate
  end
  
  def delete
    // delete
  end
  
end

class OrderRecordFactory
  
  def self.build order
    if order.pending?
      PendingOrderRecord.new order
    elsif order.paid?
      PaidOrderRecord.new order
    elsif order.completed?
      CompletedOrderRecord.new order
    end
  end

end

def validate_order order
  OrderRecordFactory.build(order).validate
end

def delete_order order
  OrderRecordFactory.build(order).delete
end
```

## Use descriptive name
- “You know you are working on clean code when each routine turns out to be pretty much what you expected.”
- The smaller and more focused a function is, the easier it is to choose a descriptive name.
- Don’t be afraid to make a name long.
- A long descriptive name is better than a short enigmatic name.
- A long descriptive name is better than a long descriptive comment.
- Don’t be afraid to spend time choosing a name.
- Choosing descriptive names will clarify the design of the module in your mind and help you to improve it.

![naming](http://i.imgur.com/ksIVsui.jpg)

## Function Arguments

### Monadic
- Two main forms:
  - Asking question about the argument.
  - Transforming the argument into something else and returning it.

### Dyadic
- Harder to understand than monadic.
- Arguments should have a natural choesion or natural ordering. 

Code 4.0: Good Dyadic function
```ruby
plot x, y
```

Code 4.1: Bad Dyadic function
```ruby
writeField output, name
```

Code 4.1.1 Refactored
```ruby
output.writeField name
```

### Triads
- Significantly harder to understand than dyads.
- Issues
  - Ordering
  - Pausing
  - Ignoring
- Think very carefully before creating a triad.

#### Flag Arguments
- Ugly and terrible practice.
- Complicates the function signature, indicating that the function does more than one thing.
- It does one thing if the flag is true and another if the flag is false!


Code 11.0
```ruby
server.start true
```

Code 11.1
```ruby
class Server
  def start logging = false
    # do something
  end
end
```

Code 11.2
```ruby
# Just use a different method
server.startWithLogging
```

### Argument Objects
- When function needs more than two or three arguments, it is likely that some of those arguments needs to be wrapped into a class of their own.

Example:

Code 5.0
```ruby
makeCircle x, y, radius
```

Code 5.1
```ruby
center = Point.new x, y
makeCircle center, radius
```

- Reducing the number of arguments by creating objects seem like cheating, but it's not.
- They are part of a concept that deserves a name of its own.

### Verbs and Keywords

- *verb-noun* pair.

  Code 6.0
  ```ruby
  # name will be written
  write name
  ```
  
  Code 6.1
  ```ruby
  # name, which is a field, will be written
  writeField name
  ```

- *keyword* form.
  - Encode the name of the arguments into the function name.

  Code 7.0  
  ```ruby
  assertEquals expeted, actual
  ```
  
  Code 7.1
  ```ruby
  assertExpectedEqualsActual expected, actual
  ```
  
## No Side Effects
- Side effects are lies.
- Function promises to do one thing, but it also does other *hidden* things.
- Side effects create Temporal coupling.
  - When two actions are bundled together into one module just because they happen to occur at the same time. [coupling](https://en.wikipedia.org/wiki/Coupling_%28computer_programming%29)

Code 8.0
```ruby
class UserValidator

  def correct_password username, password
    user = User.find_by_username username;
    
    if user.present?
      coded_phrase = user.get_phrase_encoded_by_password
      phrase = self.cryptographer.decrypt coded_phrase, password
      
      if "Valid Password" == phrase
        Session.initialize
        return true
      end
    end
    
    return false
  end
  
end
```

## Command Query Separation
- Functions should either do something or answer something, but not both.
- Change the state of an object or should return some information about that object.

Code 9.0: Bad method design
```ruby
if person.set(:name, 'unclebob')
  # do something
end
```

Code 9.1: Separate command and query
```ruby
def person.has_attribute? :name
  person.set_attribute :name, 'unclebob'
end
```

## Prefer Exceptions to Returning Error Codes
- Subtle violation of *Command Query Separation*.

Code 10.0

```ruby
def delete page
  if delete_page(page) == OK
    if registry.delete_reference(page.name) == OK
      if config_keys.delete_key(page.name.key) == OK
        puts 'Page deleted'
      else
        puts 'config key not deleted'
      end
    else
      puts 'reference from registry not deleted'
    end
  else
    puts 'delete failed'
  end
end
```

Code 10.1
```ruby
def delete page
  delete_page page
  registry.delete_reference page.name
  config_keys.delete_key page.name.key
rescue Exception => e
  puts e
end
```

## Error handling is One Thing

Code 10.2
```ruby
def delete page
  delete_page_and_all_references page
rescue Exception => e
  log_error e
end

def delete_page_and_all_references page
  delete_page page
  registry.delete_reference page.name
  config_keys.delete_key page.name.key
end

def log_error error
  # log error
end
```
