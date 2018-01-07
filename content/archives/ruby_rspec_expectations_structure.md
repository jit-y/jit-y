---
title: "rspec expectationsの構造"
date: 2018-01-07T15:52:57+09:00
draft: false
tags: ["ruby"]
---

```ruby
class Target
  attr_reader :target
  def initialize(value)
    @target = value
  end

  def to(matcher, message=nil, &block)
    PositiveHandler.handle_matcher(target, matcher, message, &block)
  end

  def not_to(matcher, message=nil, &block)
    NegativeHandler.handle_matcher(target, matcher, message, &block)
  end
end

class PositiveHandler
  def self.handle_matcher(actual, matcher, message=nil, &block)
    if actual == matcher
      'ok'
    else
      if block_given?
        yield
      else
        message || 'ng'
      end
    end
  end
end

class NegativeHandler
  def self.handle_matcher(actual, matcher, message=nil, &block)
    if actual != matcher
      'ok'
    else
      if block_given?
        yield
      else
        message || 'ng'
      end
    end
  end
end

def expect(value)
  Target.new(value)
end

def eq(value)
  value
end

expect("a").to eq "a"
expect("a").to eq("b"), "ちがうよ"
expect("a").to eq("b") do
  "チガウヨ"
end

expect("a").not_to eq "a"
expect("a").not_to eq "b"
```
