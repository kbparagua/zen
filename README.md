Zen blah blah blah
=========

principles, conventions, patterns, and techniques

## Quotes

* *"Programs must be written for people to read, and only incidentally for machines to execute."* - Harold Abelson and Gerald Jay Sussman
* *“Any fool can write code that a computer can understand. Good programmers write code that humans can understand.”* - Martin Fowler, "Refactoring: Improving the Design of Existing Code"


## General

1. Try your best to prevent nested conditional statements.

  ```ruby
    # Bad
    def foo arg
      if arg.ni?
        '' # empty string
      else
        if arg == 'hello world'
          'hi'
        else
          'wtf'
        end
      end
    end
    
    
    # Good
    def foo arg
      return '' if arg.nil?
      
      if arg == 'hello world'
        'hi'
      else
        'wtf'
      end
    end
  ```


2. Use ternary operator `?:` for conditional statements with 1 liner bodies.
  
  ```ruby
    def foo arg
      return '' if arg.nil?
      
      arg == 'hello world' ? 'hi' : 'wtf'
    end
  ```

3. Always decide in favor of readability rather than shortness of code.
4. Be careful on using too much metaprogramming. It is cool but it raises the complexity to a much higher level.

