---
title: "Embracing RVM’s Global Gemset"
date: 2010-09-17
categories: [Ruby, RVM]
---

**[UPDATE]** Management is much simpler with Bundler. I just use the default gemset provided with each version of Ruby. All my gems go together. Bundler selects the appropriate gems for the current project. Nice.

In the brave new world of Bundler and RVM, I have been looking for a way to manage my global gems. The latest iteration is working pretty good. RVM provides `~/.rvm/gemsets/global.gems`, but this only runs when a Ruby is first installed. I need to manage my day-by-day changes. Enter `~/Gemfile.global`.

```bash
rvm ree@global bundle install --gemfile=~/Gemfile.global
```

Simple enough. But very helpful. Today I started playing with Ruby 1.9.2. Some of the gems in `Gemfile.global` aren't quite ready yet. I started editing a copy, named `Gemfile.global19`, but that seemed like folly. Enter `group "1.8"`.

For 1.8, use:

```bash
bundle install --without "1.9" --gemfile=~/Gemfile.global
```

For 1.9, use:

```bash
bundle install --without "1.8" --gemfile=~/Gemfile.global
```

Note: A symbol, which is a number (`:1.8`), causes problems. Use a string (`"1.8"`).

Seeing the Gemfile might help:

```ruby
source :rubygems
gem 'autotest'
gem 'autotest-fsevent'
gem 'autotest-notification'
gem 'autotest-rails'
gem 'cheat'
gem 'diff-lcs'
gem 'github'
gem 'hitch'
gem 'jeweler'
gem 'rake'
gem 'rdoc'
gem 'rdoc-data'
gem 'sqlite3-ruby'
gem 'thin'
gem 'thor'
gem 'watchr'
```