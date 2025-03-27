---
layout: post
title: "Rails Association Callbacks"
tags: ["rails", "activerecord", "association callback"]
---

This is the third blog post in a series explaining the topics of my talk Don’t Rewrite your framework at TropitalRails.

Let's imagine a common situation: you are developing the e-commerce of the previous blog posts and you have a `Cart` model and an `Item`.

```ruby
class Cart < ApplicationRecord
  has_many :items
end

class Item < ApplicationRecord
  belongs_to :cart
end
`````
Now, you need to calculate the total price of the cart and save it in a field called `total_price`:

```ruby
class Cart < ApplicationRecord
  has_many :items

  def update_total_price
    total_price = items.map(&:total_price).sum
    update(total_price: total_price)
  end
end
`````
Every time a new product is added or removed from the cart, you iterate over all items to recalculate the total price. But what if we could make this process more efficient? Active Record provides association callbacks to handle this automatically.
```ruby
class Cart < ApplicationRecord
  has_many :items,
    after_add: :increment_total_price,
    after_remove: :decrement_total_price

  def increment_total_price(item)
    self.total_price = item.total_price + total_price
  end

  def decrement_total_price(item)
    self.total_price =  total_price - item.total_price
  end
end
``````
This change in the code will make the total price calculation dynamic. Of course, some may argue that this approach introduces side effects. That’s true, but remember—everything in programming involves trade-offs.

## References

* [Active Record Callbacks](https://guides.rubyonrails.org/active_record_callbacks.html#association-callbacks)
