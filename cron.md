# Cron

Cron is a way to run tasks on a schedule.

## crontab

With crontab you can define a cron schedule to execute your scripts. Let's say we want to make a new file in `~/temp-files` every two minutes.

First, let's make the script. In our home directory, create `make-new-file` and put this in there.

```sh
#!/usr/bin/env bash

mkdir --parents ~/temp-files
cd ~/temp-files
touch file-$(date +%s).txt
```

Then, make it executable.

```sh
chmod +x make-new-file
```

Now, let's add it to our crontab. Run `crontab -e` to edit our crontab. Add this line to the bottom.

```sh
*/2 * * * * ~/make-new-file
```

This will run the script every two minutes. The first `*` is for the minute, the second is for the hour, the third is for the day of the month, the fourth is for the month, and the fifth is for the day of the week. The `*` means "every". So, `*/2` means "every two". If we want to run it every day at 3:30 AM, we would do `30 3 * * *`. If we want to run it every Monday at 3:30 AM, we would do `30 3 * * 1`.

For easier editing, we can use [crontab.guru](https://crontab.guru/).
