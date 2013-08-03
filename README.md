Zen blah blah blah
=========

principles, conventions, patterns, and techniques

## Quotes

* *"Programs must be written for people to read, and only incidentally for machines to execute."* - Harold Abelson and Gerald Jay Sussman
* *“Any fool can write code that a computer can understand. Good programmers write code that humans can understand.”* - Martin Fowler, "Refactoring: Improving the Design of Existing Code"


## General

1. Flat is better than nested.

  ```ruby
    # Bad
    if arg.nil?
      '' # empty string
    else
      if arg == 'hello world'
        'hi'
      else
        'wtf'
      end
    end
  
    
    # Good
    return '' if arg.nil?
    
    if arg == 'hello world'
      'hi'
    else
      'wtf'
    end
  ```

3. Always decide in favor of readability rather than shortness of code.
4. Be careful on using too much metaprogramming. It is cool but it raises the complexity to a much higher level.
5. Use logically named variables rather than long expressions on conditional statements.

