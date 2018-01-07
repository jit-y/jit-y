---
title: "Ruby_configure_pattern"
date: 2018-01-07T15:57:56+09:00
draft: false
tags: ["ruby"]
---

```ruby
module App
  self << class
    def configure
      yield config
    end

    def config
      @config ||= Configure.new
    end
  end

  class Configure
    attr_accessor :foo

    def initialize
      @foo = 'bar' # default
    end
  end
end

App.conigure do |config|
  config.foo = 'baz'
end
```
