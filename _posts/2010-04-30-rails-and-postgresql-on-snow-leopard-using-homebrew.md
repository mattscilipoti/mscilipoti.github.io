---
title: "Rails and PostgreSQL on Snow Leopard using homebrew"
date: 2010-04-30
categories: [Rails, PostgreSQL, Homebrew]
---

**Update: 12 May 2010.** Updated automatic load instructions.

As we have seen in the past, I couldn't find a definitive, accurate install and use guide for PostgreSQL with Rails on Snow Leopard, using Homebrew. In the end, it took less work than any of the existing instruction lists (that I found) indicated.

**Of Note:** I did NOT need to make any changes to my `$PATH` or `.profile`.

Most of this is based on info from:

- [http://blog.tquadrado.com/?p=215](http://blog.tquadrado.com/?p=215)
- [http://www.gregbenedict.com/2009/08/31/installing-postgresql-on-snow-leopard-10-6/](http://www.gregbenedict.com/2009/08/31/installing-postgresql-on-snow-leopard-10-6/)

Thank you all for the helpful info.

### Installation

We assume you have Homebrew installed.

**Note:** I did not allow incoming network connections. You may, but everything I discuss works without it.

```bash
$ brew install postgresql # postgres is alias
```

Follow the instructions for initializing (`initdb...`). Follow the instructions for automatic load, if applicable (`launchctl...`).

That's it. I didn't need any of the `mkdir` and `chown` commands some instructions listed.

Install the fast Postgres gem:

```bash
$ gem install pg
```

Installation complete.

**UPDATE:** If Postgres is not loading on boot OR you see:

```bash
launchctl: Dubious ownership on file (skipping): /usr/local/Cellar/postgresql/8.4.3/org.postgresql.postgres.plist
nothing found to load
```

Then this should fix it:

```bash
sudo chown root:wheel /usr/local/Cellar/postgresql/8.4.3/org.postgresql.postgres.plist
sudo launchctl load -w /usr/local/Cellar/postgresql/8.4.3/org.postgresql.postgres.plist
```

### Rails

The docs indicate that a superuser named `postgres` is created by default. Instead, my installation created a superuser named after the logged-in user: `$USER`.

To make sharing the `database.yml` easier, I created another user:

```bash
$ createuser --superuser your_company_name -U $USER
```

Now update your `database.yml`:

```yaml
development:
  adapter: postgresql
  database: health_crowd_dev
  username: your_company_name
  host: localhost
```

Some instructions list more settings. These worked for me.

You are ready:

```bash
$ rake db:create
```

Fini.