---
title: "moonshine, rmagick, and jaunty"
date: 2009-08-22
categories: [Rails, Deployment, Moonshine]
---

### Moonshine

[Moonshine](http://github.com/railsmachine/moonshine/tree/master) is "Rails deployment and configuration management done right. ShadowPuppet + Capistrano == crazy delicious."

We are going through a phase of server 'provisioning'. We need a demo server, one for load testing, and an updated CI server. We are also moving some production servers from Windows to Ubuntu. Yay!

Moonshine embraces convention and combines Rails provisioning and deployment in a nice package. We enjoy it.

### Gem Library Dependencies

Some gems have system library dependencies (e.g., `rmagick` requires `imagemagick`, etc.). Moonshine already has "recipes" for a few (see `apt_gems.yml`). When Moonshine is installing a gem, it also installs any libraries listed for that gem. You can easily add your own in your `moonshine.yml`.

**Gotcha:** Unfortunately (as we just discovered), your `moonshine.yml` entries do not override existing entries in `apt_gems.yml`. There are two options for overriding:

1. Replace the entry in the plugin's `apt_gems.yml`.
2. Comment out the entry in the plugin's `apt_gems.yml`, then add your own entry in `moonshine.yml`.

We chose #2.

### Jaunty & rmagick

While [Moonshine](http://github.com/railsmachine/moonshine/tree/master) is, currently, only supported on Ubuntu 8.10 (Intrepid Ibex, really? Not LTS or the latest release? Interesting choice.), we have only found one issue with Jaunty (9.04): new library dependencies for `rmagick/imagemagick`.

Here are the `rmagick` dependencies in Jaunty:

```yaml
:apt_gems:
  rmagick:
    - imagemagick
    - libmagickcore-dev
    - libmagickwand-dev
```

**Note:**

- We are using the string version, not the symbol (for `rmagick`). Either may work; we haven't tested the symbol yet.
- We are using `rmagick -v 2.9.2`.