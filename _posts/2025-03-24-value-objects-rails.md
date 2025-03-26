---
layout: post
title: "Value Objects in Rails"
tags: ["rails", "activerecord", "value object"]
---

This is the second blog post in a series explaining the topics of my talk Don’t Rewrite your framework at TropitalRails.

Have you heard of value objects? Today, we're going to see what they are and how to use them in Rails. First of all, let's take a look at example below:
```ruby
class Customer < ApplicationRecord
  validates :name, :email, :password, :address_street, :address_city, presence: true
end
```
In this example, we have two related fields: `address_street` and `address_city`. They are related because they describe the same thing: an address. We could compose an address with those fields and form an object without identity only a state. It's called **value object**.

To clarify, let's create a class to represent the address:

```ruby
class Address
  attr_reader :street, :city

  def initialize(street, city)
    @street = street
    @city = city
  end

  def ==(other)
    @street == other.street && @city == other.city
  end
end
```
Now we need to use this class in our `Customer` model. Active Record provides us with the method `composed_of`:

```ruby
class Customer < ApplicationRecord
  composed_of :address, mapping: { address_street: :street, address_city: :city }
end
```

Now we are able to use address like as an object:
```ruby
customer = Customer.new
customer.address_street = "123 Main St"
customer.address_city = "Anytown"
customer.address
#<Address:0x0000ffffb045c6c0 @city="Anytown", @street="123 Main St">


address = Address.new("123 Main St", "Anytown")
customer = Customer.new
customer.address = address


other_address = Address.new("456 Elm St", "Othertown")
customer.address == other_address
# false
```

We are also able to use the Address class in queries
```ruby
Customer.where(address: Address.new("123 Main St", "Anytown"))
```

## References

* [wiki.c2](https://wiki.c2.com/?ValueObject)
* [martinfowler.com](https://martinfowler.com/bliki/ValueObject.html)
* [Active Record Aggregations](https://api.rubyonrails.org/classes/ActiveRecord/Aggregations/ClassMethods.html)
