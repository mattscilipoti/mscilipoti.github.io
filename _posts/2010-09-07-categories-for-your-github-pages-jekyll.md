---
title: "Categories for your GitHub Pages (Jekyll)"
date: 2010-09-07
categories: [Jekyll, GitHub Pages]
---

You, like me, have a GitHub Pages powered blog. You, like me, are contemplating switching to WordPress for tags/categories. Fret no more. I added categories to my GitHub Pages powered blog today. You can too.

Just assign a YAML list to categories in your page meta:

```yaml
categories:
- ruby
- rvm
```

Listing and linking to them was harder than I expected. I was forced to create a page for each category that I wanted a list of posts for (`code.html`, `ruby.html`).

Update: I went to great pains to format this Liquid code (within Liquid). Even using the `--safe` argument on my Jekyll server. But, it still formatted differently on GitHub Pages. I hope to figure out a workaround shortly. Until then, check out the source [at GitHub](http://github.com/mattscilipoti/mattscilipoti.github.com).

For each post, I expected to get a nice list of assigned categories using:

```liquid
{{ " {{ post.categories | array_to_sentence_string " }} }}
```

Instead, I got:

```
nothing
```

Then I tried this (and variations):

```liquid
In this post:

{{ " {% for category in post.categories " }} %}
  <a href="/{{">{{ " {{ category[0] " }} }}</a>
{{ " {% endfor" }} %}
```

Which rendered:

```
In this post:
```

That's right. Nothing.

`site.categories` works nicely in that loop, but I wanted the categories for this post.

In this site:

```liquid
{% for category in site.categories  %}
  <a href="/{{ category[0] }}.html">{{ category[0] }}</a>
{% endfor %}
```

Armed with this knowledge, I set out to create a Tag Cloud. But, I could not find a way to get the count of posts for each category. I found an example on [litanyagainstfear.com](http://github.com/qrush/litanyagainstfear/blob/master/_layouts/default.html). Alas, it looks like you have to name each category explicitly to get the count of posts.

```liquid
{{ " {{ site.categories.code | size " }} }}
{{ " {{ site.categories.ruby | size " }} }}
```

Which led me to the (poor man's) Tag Cloud you see in the upper right corner.

The good news? I was able to use `_includes` to reuse the category page for each category.

See the source for this page [at GitHub](http://github.com/mattscilipoti/mattscilipoti.github.com).