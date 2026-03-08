---
title: "blog_engines.detect {|engine| engine.not_engine?}"
date: 2009-08-18
categories: [Rails, Engines, Plugins]
---

### Blog Engines

Rails engines are miniature Rails applications that can be embedded within another application. They are useful for sharing functionality between applications or for modularizing large applications.

### Detecting Engines

In Rails, you can use the `detect` method to find the first engine that meets a certain condition. For example:

```ruby
blog_engines.detect { |engine| engine.not_engine? }
```

This code iterates over the `blog_engines` collection and returns the first engine for which the `not_engine?` method returns `true`.

### Gotchas

- **Lazy Evaluation:** The `detect` method stops iterating as soon as it finds a match, making it more efficient than filtering the entire collection.
- **Nil Return:** If no match is found, `detect` returns `nil`. Be sure to handle this case in your code.

### Example

Here is a more complete example:

```ruby
blog_engines = [Engine.new("WordPress"), Engine.new("Jekyll"), Engine.new("Ghost")]

# Define the not_engine? method for demonstration purposes
class Engine
  attr_reader :name

  def initialize(name)
    @name = name
  end

  def not_engine?
    name == "WordPress"
  end
end

# Detect the first non-engine
non_engine = blog_engines.detect { |engine| engine.not_engine? }

if non_engine
  puts "Found a non-engine: #{non_engine.name}"
else
  puts "No non-engines found."
end
```

### Conclusion

The `detect` method is a powerful tool for finding the first matching element in a collection. Use it wisely to improve the readability and efficiency of your code.