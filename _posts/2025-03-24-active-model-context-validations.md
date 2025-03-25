---
layout: post
title: "Multistep Validations with Active Model"
tags: ["ruby", "activemodel", "validations"]
---

This is the first blog post in a series explaining the topics of my talk Don't Rewrite your framework at TropitalRails.

Imagine the following scenario: You have an e-commerce. Customers should create an account.
```ruby
class Customer < ApplicationRecord
  validates :name, :email, :password, :address_street, :address_city, presence: true
end
``````

Then, you receive a request to make the address information optional at first, but only when the customer buys something. Basically, you need to have different validation steps in the same model. How would you do that? For these cases, ActiveModel provides context validations.

```ruby
class Customer < ApplicationRecord
  validates :name, :email, :password, presence: true, on: :create
  validates :address_street, :address_city, presence: true, on: :account_finish
end
````
Now we have two different contexts `create` and `account_finish`. In the example below we try to create a customer without the address informations. The validation will pass because we are using the `create` context.
```ruby
customer = Customer.new(name: 'John', email: 'john@google.com', password: '123')
customer.valid?
# => true

customer.save
# => true
```
In the next example, we try to finish the account without the address informations. The validation will fail, because we are using the `account_finish` context.

```ruby
customer.valid?(:account_finish)
# => false

customer.save(context: :account_finish)
# => false
```

We can fill the address informations and try again.

```ruby
customer.address_street = "1105 West Peachtree St"
customer.address_city = "Atlanta"
customer.valid?(:account_finish)
# => true

customer.save(context: :account_finish)
# => true
```
As we saw in this short post, context validations can be very useful. They allow us to have different validation steps in the same model. Rails also provides a context for update, similar to create.

## References

* [ActiveModel Validations](https://guides.rubyonrails.org/active_record_validations.html#on)
* [Boring Rails](https://boringrails.com/tips/activerecord-validation-context)
