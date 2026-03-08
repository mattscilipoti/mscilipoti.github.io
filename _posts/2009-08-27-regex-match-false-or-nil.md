---
title: "regex match false? or nil?"
date: 2009-08-27
categories: [Ruby, Regex]
---

We have some scripts we run which are "instrumented" with messages and progress bars. Two issues:

1. These are muddying up our specs.
2. In production, the progress bars can raise "Broken Pipe" errors (stdout issues?).

We will probably create some abstraction for this concept, but we started with:

```ruby
puts "It's working." if show_instrumentation?
```

### The Specs

```ruby
describe '#show_instrumentation?' do
  it "should default to false" do
    @it.show_instrumentation?.should be_false
    # @it.should_not be_show_instrumentation
  end
end

describe '#show_instrumentation?' do
  it "should be true if ENV['INFO']=true" do
    ENV['INFO'] = true
    @it.show_instrumentation?.should be_true
  end
end
```

### The Code

```ruby
def show_instrumentation?
  @show_instrumentation ||= (ENV['info'] =~ /true/i)
end
```

This worked. But sometimes, `ENV['INFO'] == 'true'` when the first spec was run (`spec --reverse`). So... we added the `after`:

```ruby
after :each do
  ENV['info'] = ''
end
```

Fail. `nil` is not `false`.

Turns out:

```ruby
'' =~ /true/i    # -> nil
'false' =~ /true/i # -> nil
'TRUE' =~ /true/i  # -> 0
nil =~ /true/i     # -> false
```

So... how do you return a boolean from `=~`?

```ruby
def show_instrumentation?
  @show_instrumentation ||= (ENV['info'] =~ /true/i) >= 0
end
```