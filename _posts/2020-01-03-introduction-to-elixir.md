---
layout: post
title: "Learning elixir language - Part 1"
tags: ["elixir", "functional programming"]
---

Today I wanna start a post series with the topic Learning Elixir. This is the first post to introduce this wonderful technology.

## What is Elixir lang?

Quoting the [elixir documentation](https://elixir-lang.org/): 

>> *Elixir is a dynamic, functional language designed for building scalable and maintainable applications.*

>> *Elixir leverages the Erlang VM, known for running low-latency, distributed and fault-tolerant systems, while also being successfully used in web development and the embedded software domain.*


Elixir is a language that running over Erlang Virtual Machine. It's machine was designed to support the information's traffic in Ericsson telecommunication systems.

It's creator is a brazilian developer called [José Valim](https://twitter.com/josevalim), he also was a Rails Core Team Member having leader evolution from Rails 2 to Rails 3, doing the framework modular. Another curious is the fact of Rubyists joined to Elixir community, like Dave Thomas for example. Hence, Elixir has started using the best parts of Ruby way.

## Why did Elixir was created for?

In 2011 José Valim leaved Plataformatec to threat a repetitive strain injury, he decides use this free time to read some books in this books stack was [Seven Languages in Seven Weeks](https://www.amazon.com/Seven-Languages-Weeks-Programming-Programmers/dp/193435659X). Reading it, José learned about Erlang technologies, he realized that Erlang Virtual Machine was wonderful, but the language was hard to understand.

Then, he had an idea to design a language easy to use based in Ruby, therefore, Elixir was started to be built. Elixir was designed to take advantages from Erlang Machine, likes low-latency and fault tolerance. And also, to be easy to programming likes Ruby is.

## Elixir is a functional language, what it means?

Different from object oriented languages like Java, Ruby or Python, elixir follows the functional paradigm. Functional languages there are some differences from OO languages:

 * Don't keeps state
 * High order functions
 * Recursion

### Don't keeps state

It means that there isn't an internal state to be changed during code execution, different from objects in OOP paradigm. The objects has properties that's determines its state.

```ruby
class Lamp
  
  def initialize
    @is_on = false
  end

  def turn_on
    @is_on = true
  end

  def turn_off
    @is_on = false
  end
end
```

On the Lamp class described above we have an example of keeps state. Each lamp object should has a property called `is_on`, this property know if lamp is on or off. Functional languages doesn't keep this kind of informations, only handle the creation of new data based on received parameters and returns it.

Another interesting example to get it concept is the below. The first code was written in Ruby, the string "Hello World!" is an object instanced from [String class](https://ruby-doc.org/core-2.5.1/String.html), thus we could call the method `lenght` direct from it.

```ruby
"Hello World!".lenght
# => 12
```

On the second example we have an elixir code, the string "Hello World!" is only a primitive type. To know its size we need to call the module `String` and its method `length`.

```elixir
String.length("Hello World!")
12
```

### High order functions

This is an ability to treat functions as data, passing it like parameter, assigns to variable, returning and using it in a determined moment.

```elixir

# Function defined and immidiatly assigns to variable
sum = fn (a, b) -> a + b end
sum.(1, 2)


# Passing a function as parameter
def receive_function(function, first_number, second_number) do
  function(first_number, second_number)
end

receive_function(sum, 1, 2)

# Function returning another one
def returning_function(a, b) do
  fn (a, b) -> a + b end
end
```

There are many object oriented languages implementing this concept of high order functions, but remember it was born from functional paradigm.

## Recursion

It's the possibility to one function call itself one more time working in a similar way like a looping. The first example below written in PHP language shows a loop interacting over an array:

```php
$loop = [1, 2, 3];
$size = sizeof($loop);

for ($i = 0; $i < $size; $i++) {
    echo "{$loop[$i]}\n";
}
```
The PHP example above is interacting using the imperative programming. This approach is used mainly in languages like C for example, the main idea is control the code flux thought statements as `if`, `else` and `else if` and repetition controls as `while`, `do/while` and `for`.

```elixir
defmodule Recursive do

  def loop([head | tail]) when tail !== [] do
    IO.puts head
    loop(tail)
  end

  def loop([head]), do: IO.puts head

end

Recursive.loop([1, 2, 3])
```
The elixir code example above has same behaviour of PHP code, but the difference is in the paradigm. In elixir was used recursion a concept from functional languages. Rather than, control code flux using a repetition structure is used pattern matching and guard. I wanna explain this concepts in future blog posts.

## Conclusion

Elixir has been seems a good choose to starts with functional programming because is simple, robust and you gain all benefits of Erlang Virtual Machine.

Although, the concepts like high order function and recursion could be implemented using non functional languages, this approach was born from functional and works better in this family languages.

## References

* [Interview with José Valim](https://www.youtube.com/watch?v=Vmln9LvGbdo)
* [Difference between OOP and functional programming](https://www.educba.com/functional-programming-vs-oop/)
* [Recursion](https://elixir-lang.org/getting-started/recursion.html)
