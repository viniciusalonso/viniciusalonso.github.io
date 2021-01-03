---
layout: post
title: "Ruby tip: dig methods your best friends"
tags: ["ruby"]
---

It's probably that you needed to go through by some hash or array like the below:

```ruby
item = {
  id: "0001",
  type: "donut",
  name: "Cake",
  ppu: 0.55,
  batters: {
    batter: [
      {id: "1001", type: "Regular"},
      {id: "1002", type: "Chocolate"},
      {id: "1003", type: "Blueberry"},
      {id: "1004", type: "Devil's Food"}
    ]
  },
  topping: [
    {id: "5001", type: "None"},
    {id: "5002", type: "Glazed"},
    {id: "5005", type: "Sugar"},
    {id: "5007", type: "Powdered Sugar"},
    {id: "5006", type: "Chocolate with Sprinkles"},
    {id: "5003", type: "Chocolate"},
    {id: "5004", type: "Maple"}
  ]
}
```

Let's suppose you wanna get "Chocolate" value. At the first moment, you would think to go through like this:

```ruby
> item[:batters][:batter][1][:type]
# => Chocolate
```

And if `batter` does not exist? Or the array index does not exist? It would throw an exception doing your code breaks.

```ruby
> item[:batters][:batter][10][:type]
# => NoMethodError (undefined method `[]' for nil:NilClass)
```

## Hash#dig

A better approach could be to use `dig` method:

```ruby
> item.dig(:batters, :batter, 1, :type)
# => "Chocolate"
```

Dig does not throw exceptions, it just returns `nil` when a key is not found.

```ruby
> item.dig(:batters, :batter, 300, :type)
# => nil
```

This method is implemented by others classes like `Array`, `Struct`, `OpenStruct`, `CSV::Table` and `CSV::row`.

## Reference

 - [Dig Methods](https://docs.ruby-lang.org/en/3.0.0/doc/dig_methods_rdoc.html)
