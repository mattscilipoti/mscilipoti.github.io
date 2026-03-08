---
title: "script/runner and cron"
date: 2009-08-20
categories: [Rails, Cron, Automation]
---

### script/runner

[script/runner](http://api.rubyonrails.org/classes/Runner.html) is a Rails command-line tool for running scripts in the context of your Rails application. It is useful for running scripts that interact with your models, such as maintenance tasks or data migrations.

### Cron Jobs

Cron is a Unix utility for scheduling tasks to run at specific times or intervals. It is commonly used for automating repetitive tasks, such as backups, updates, or maintenance scripts.

### Combining script/runner and Cron

To run a Rails script using `script/runner` in a cron job, you can add an entry to your crontab file. For example:

```bash
0 0 * * * /path/to/your/app/script/runner -e production 'YourModel.your_method'
```

This example runs the `your_method` method of the `YourModel` class every day at midnight in the production environment.

### Gotchas

- **Environment:** Make sure to specify the correct Rails environment using the `-e` flag. The default is `development`.
- **Path:** Use absolute paths for both the `script/runner` command and any files or directories it depends on.
- **Output:** Redirect output to a log file to capture any errors or messages. For example:

```bash
0 0 * * * /path/to/your/app/script/runner -e production 'YourModel.your_method' >> /path/to/your/log/cron.log 2>&1
```

### Alternatives

If you are using Rails 3 or later, consider using the [Whenever gem](https://github.com/javan/whenever) for managing cron jobs. It provides a more Ruby-like syntax and integrates well with Rails.