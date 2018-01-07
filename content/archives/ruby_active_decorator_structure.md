---
title: "active_decoratorの構造"
date: 2018-01-07T15:51:39+09:00
draft: false
tags: ["ruby"]
---

## view_contextの構造
```ruby
class FooController
  around_action do |controller, block|
    begin
      Thread.current[:foo_view_context] ||= []
      Thread.current[:foo_view_context] << controller.view_context
      block.call
    ensure
      Thread.current[:foo_view_context].pop if Thread.current[:foo_view_context]
    end
  end

  def index
    foo = Foo.new
    foo.extend(FooDecorator)
    foo.bar
  end
end

class Foo; end

module FooDecorator
  def method_missing(method, *args, &block)
    super
  rescue NoMethodError, NameError
    begin
      (view_context = Thread.current.last).send method, *args, &block
    rescue NoMethodError
      raise NoMethodError, "undefined method `#{method}` for either #{self} or #{view_context}"
    rescue NameError
      raise NameError, "undefined local variable `#{method}` for either #{self} or #{view_context}"
    end
  end

  def bar
    params
    "bar"
  end
end
```

## decorateの構造

```ruby
# AbstractController::Rendering#view_assignsをモンキーパッチ
# view_assignsにはインスタンス変数などが格納されている
module AbstractController
  module Rendering
    def view_assigns
      hash = super
        hash.each_value do |v|
          ActiveDecorator::Decorator.instance.decorate v
        end
      hash
    end
  end
end

```
## Decorate済みか否かの判定
Object#extendで
https://docs.ruby-lang.org/ja/latest/method/Object/i/is_a=3f.html

```ruby
class Foo;end
module Decorated;end
f = Foo.new
f.is_a?(Decorated)
#=> false
f.extend(Decorated)
f.is_a?(Decorated)
#=> true
```
