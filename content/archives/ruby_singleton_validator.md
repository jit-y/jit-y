---
title: "singleton_validator"
date: 2018-01-07T15:55:00+09:00
draft: false
tags: ["ruby"]
---


```ruby
class Foo
  include ActiveModel::Model
  attr_accessor :a, :b
end

module FooValidations
  def self.extended(obj)
    obj.singleton_class.class_eval do
      validates :a, presence: true
      validates :b, length: { maximum: 255 }
    end
  end
end

foo1 = Foo.new(a: nil, b: 'a' * 256)
foo2 = Foo.new(a: nil, b: 'a' * 256)
foo1.extend(FooValidations)
foo1.valid? #=> false
foo2.valid? #=> true
```
