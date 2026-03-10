---
title: "I18n for Rails 3 and common gems"
date: 2010-09-14
categories: [Rails, I18n]
---

### Part One: Discussion

You're building a Rails 3 app. It needs to support internationalization (I18n). No problem. Rails 3 supports internationalization.

You can use `translate(key)` or `t(key)`:

```haml
%h2= t('hello')
```

It looks up the key in `config/locales/en.yml`:

```yaml
en:
  hello:
    Hey y'all
```

Yielding:

```
Hey y'all
```

(Aside: I will be using `en.yml` throughout this post, any language file could be used.)

But, we're using Rails. We expect conventions. And smart defaults. I shouldn't need to use `t(key)` wherever I need translations.

### My Expectations

1. Items that can be internationalized should have smart defaults. I should not have to make an entry like:

```yaml
labels:
  name: Name
```

2. Similarly, there should be fallback/common/generic entries. If an error message doesn't exist for this validation, on this specific model, use the generic message for this validation. If a generic message doesn't exist in the localization file, fall back to the smart default.

The generic:

```yaml
create_success: "%{model} created"
create_fail: "The %{model} could not be created:"
```

The specific:

```yaml
tour_versions:
  create:
    success: Your tour was successfully published.
```

3. Where possible, libraries should follow the Rails conventions instead of inventing their own. This allows me to switch (or simply remove) libraries and I18n still works.

### What I Found

Platformatec has a [nice listing of Rails I18n conventions](http://blog.plataformatec.com.br/2010/02/rails-3-i18n-changes/).

On our project, we use `simple_form`, `inherited_resources`, and `cancan`. Each one has a convention for I18n. Each one different.

Luckily, the functionality doesn't overlap... much.

- Rails covers error message defaults, attributes, submit buttons, and labels.
- `InheritedResources` covers flash (using Responders).
- `SimpleForm` deals with label, hint, and error.
- `Cancan` presents a 'not authorized' message.

So... expectations #1 & #2 are generally covered. But, sadly, #3 is not.