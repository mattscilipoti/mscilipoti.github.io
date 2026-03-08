---
title: "Cancan, Inherited Resources, and Nested Routes"
date: 2010-10-07
categories: [Rails]
---

**Problem:**

My subscription does not have a subscriber. InheritedResources is not populating the `belongs_to` item.

```ruby
SubscriptionsController << InheritedResources::Base
  belongs_to :subscriber
  load_and_authorize_resource
  def create
    @subscription = build_resource
    # do something with subscription
    create!
  end
end
```

In `def create`, `@subscription.subscriber` is nil. This should be populated by InheritedResources during `#build_resource`.

```ruby
def build_resource
  get_resource_ivar || set_resource_ivar(end_of_association_chain.send(method_for_build, params[resource_instance_name] || {}))
end
```

Unfortunately, Cancan’s `load_resource` is populating the `@subscription` instance variable, so `build_resource` simply returns the value of the instance variable (`get_resource_var`).

**Solution:**

Don’t ask Cancan to load the resource.

```ruby
SubscriptionsController << InheritedResources::Base
  belongs_to :subscriber
  authorize_resource
  def create
    @subscription = build_resource
    # do something with subscription
    create!
  end
end
```

Or…

```ruby
SubscriptionsController << InheritedResources::Base
  belongs_to :subscriber
  def create
    @subscription = build_resource
    authorize! :create, Subscription
    # do something with subscription
    create!
  end
end
```

Note: This appears to only be an issue if using nested routes.

```ruby
resources :subscriber do
  resources :subscription
end
```

This was identified on Rails 3. May affect Rails 2.