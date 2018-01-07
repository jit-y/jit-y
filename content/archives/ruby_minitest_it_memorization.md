---
title: "[minitest] itのメモ化"
date: 2018-01-07T15:49:39+09:00
draft: false
tags: ["ruby"]
---

[minitest](https://github.com/seattlerb/minitest/blob/master/lib/minitest/spec.rb#L245-L248)

```ruby

define_method name do
  @_memoized ||= {}
  @_memoized.fetch(name) { |k| @_memoized[k] = instance_eval(&block) }
end

```

- [fetch](https://docs.ruby-lang.org/ja/latest/method/Hash/i/fetch.html)
