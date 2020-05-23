---
layout: post
title: "How Elixir's pattern matching works"
tags: ["elixir", "functional programming"]
description: >
  In the previous post, we’ve started to learn about Elixir’s history and functional programming. Today I’m gonna explain one of the most important concepts: pattern matching.

---

In the previous post, we’ve started to learn about Elixir’s history and functional programming. Today I’m gonna explain one of the most important concepts: pattern matching.

## What is pattern matching?

It’s a computer science concept where a bit sequence or part of them is searched following a specific pattern. Among its usages, one highlight is its use in functional programming languages. Many functional languages apply it, for a very good reason, pattern matching became programming more declarative.

Let’s start by = operator, in languages like PHP, Java or Ruby this operator has a unique function: assignment. But, in Elixir instead of only assigns a value to a variable, it compares the pattern into sides before assigns. Let’s check out the examples below:

```elixir
# The Elixir compare left side to right side before assign
iex> variable = 10
10

iex> colors = ["red", "green"]
["red", "green"]
```

These two simple examples above should seem like other languages, but let’s see how to get the elements inside the list.

```elixir
# To get each item in the list
iex> [first, second] = ["red", "green"]
["red", "green"]

iex> first
"red"

iex> second
"green"
```

This approach may be used in other structs.

```elixir
# Gets information from a map

iex> %{email: email} = %{email: "vba321@hotmail.com"}
%{email: "vba321@hotmail.com"}

iex> email
"vba321@hotmail.com"
```

## Decomposing lists

It's possible to handle lists in an easy way:

```elixir

iex> [head | tail] = [1, 2, 3, 4, 5]
iex> head
1

iex> tail
[2, 3, 4, 5]
```


## Pattern matching does Elixir's recursion beautiful

If you are familiar with languages likes Java you have to see methods overload. Overloaded methods tend to repeat the same name using different signs.

In Elixir we have two ways to does it, the first is using the arity The arity is about how many parameters your function should receive. The other way is using pattern matching.

```elixir
defmodule Factorial do
  def calculate(0), do: 1

  def calculate(n), do: n * calculate(n - 1)
end
```

Let's looks to the next example where the same problem is solved using PHP:

```php
class Factorial
{
    public function calculate(int $n) : int
    {
        if ($n === 0)
        {
            return 1;
        }

        return $n * $this->calculate($n - 1);
    }
}
```

The recursion doesn't happen in a natural way in PHP, we need to use an `if` statement to control the code flux direction. Instead, Elixir implements recursion in a beautiful form keeping the code more readable and clear.


## Conclusion

The main goal of this post is to show a bit of how pattern matching acts in the Elixir language. Different from OO or imperative languages, functional programming applies the recursion concept in the right way.

This kind of comparative it’s good to understand how the languages fit in your problem.

## References

* [Pattern matching definition](https://www.techopedia.com/definition/8801/pattern-matching)
* [Elixir pattern matching](https://elixir-lang.org/getting-started/pattern-matching.html)
