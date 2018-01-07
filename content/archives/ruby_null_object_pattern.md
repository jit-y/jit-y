---
title: "RubyでNullObjectパターン"
date: 2018-01-07T15:45:29+09:00
draft: false
tags: ["ruby"]
---

[airake/airbrake-ruby](https://github.com/airbrake/airbrake-ruby)を参考に

Hash.newを利用している。

```ruby
class NullPlugin
  def hello
  end
end

class FooPlugin
  def hello
    'hello'
  end
end

class App
  @plugins = Hash.new(NullPlugin.new)
  class << self
    attr_reader :plugins
    def [](plugin_name)
      plugins[plugin_name]
    end
    def add_plugin(plugin_name, plugin)
      plugins[plugin_name] = plugin
    end
  end
end

class Client
  def hello(plugin_name)
    App[plugin_name].hello
  end
end

App.add_plugin(:foo, FooPlugin.new)
c = Client.new
c.hello(:aaa)
#=> nil
c.hello(:foo)
#=> "hello"
```
