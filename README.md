# gicowa.py - GitHub Commit Watcher

GitHub's *Watch* feature doesn't send notifications when commits are pushed. This script aims to
implement this feature and much more.

## Installation

```
# sudo apt-get install sendmail
# sudo pip install PyGithub
$ git clone https://github.com/AurelienLourot/github-commit-watcher.git
```

## Quick setup

Add the following line to your `/etc/crontab`:

```
0 * * * * root /path/to/github-commit-watcher/gicowa.py --persist --no-color --mailto myself@mydomain.com lastwatchedcommits MyGitHubUsername sincelast > /tmp/gicowa 2>&1
```

**That's it.** As long as your machine is running you'll get e-mails when something gets pushed on
a repo you're watching.

## Other/Advanced usage

`gicowa` is a generic command-line tool with which you can make much more that just implementing
the use case depicted in the introduction. This section shows what it can.

First of all, add the following line to your `.bashrc`:

```bash
alias gicowa='/path/to/github-commit-watcher/gicowa.py'
```

### List repos watched by a user

```
$ gicowa watchlist AurelienLourot
watchlist AurelienLourot
AurelienLourot/uncommitted
AurelienLourot/crouton-emacs-conf
brillout/FasterWeb
AurelienLourot/github-commit-watcher
```

### List last commits on a repo

```
$ gicowa lastrepocommits AurelienLourot/github-commit-watcher since 2015 07 05 09 12 00
lastrepocommits AurelienLourot/github-commit-watcher since 2015-07-05 09:12:00
2015-07-05 10:46:27 - Aurelien Lourot - Minor cleanup.
2015-07-05 09:39:01 - Aurelien Lourot - watchlist command implemented.
2015-07-05 09:12:00 - Aurelien Lourot - argparse added.
```

### List last commits on repos watched by a user

```
$ gicowa lastwatchedcommits AurelienLourot since 2015 07 04 00 00 00
lastwatchedcommits AurelienLourot since 2015-07-04 00:00:00
AurelienLourot/crouton-emacs-conf - 2015-07-04 17:08:48 - Aurelien Lourot - Support for Del key.
brillout/FasterWeb - 2015-07-04 16:38:55 - brillout - add README
AurelienLourot/github-commit-watcher - 2015-07-05 10:46:27 - Aurelien Lourot - Minor cleanup.
AurelienLourot/github-commit-watcher - 2015-07-05 09:39:01 - Aurelien Lourot - watchlist command implemented.
AurelienLourot/github-commit-watcher - 2015-07-05 09:12:00 - Aurelien Lourot - argparse added.
AurelienLourot/github-commit-watcher - 2015-07-05 09:07:14 - AurelienLourot - Initial commit
```

### List last commits since last run

Any listing command taking a `since <timestamp>` argument takes also a `sincelast` one. It will then
use the time where that same command has been run for the last time on that machine with the option
`--persist`. This option makes `gicowa` remember the last execution time of each command in
`~/.gicowa`.

```
$ gicowa --persist lastwatchedcommits AurelienLourot sincelast
lastwatchedcommits AurelienLourot since 2015-07-05 20:17:46
$ gicowa --persist lastwatchedcommits AurelienLourot sincelast
lastwatchedcommits AurelienLourot since 2015-07-05 20:25:33
```

### Send output by e-mail

You can send the output of any command to yourself by e-mail:

```
$ gicowa --no-color --mailto myself@mydomain.com lastwatchedcommits AurelienLourot since 2015 07 04 00 00 00
lastwatchedcommits AurelienLourot since 2015-07-04 00:00:00
AurelienLourot/crouton-emacs-conf - 2015-07-04 17:08:48 - Aurelien Lourot - Support for Del key.
brillout/FasterWeb - 2015-07-04 16:38:55 - brillout - add README
AurelienLourot/github-commit-watcher - 2015-07-05 10:46:27 - Aurelien Lourot - Minor cleanup.
AurelienLourot/github-commit-watcher - 2015-07-05 09:39:01 - Aurelien Lourot - watchlist command implemented.
AurelienLourot/github-commit-watcher - 2015-07-05 09:12:00 - Aurelien Lourot - argparse added.
AurelienLourot/github-commit-watcher - 2015-07-05 09:07:14 - AurelienLourot - Initial commit
Sent by e-mail to myself@mydomain.com
```

> **NOTES:**
>
> * You probably want to use `--no-color` because your e-mail client is likely not to render the
>   bash color escape sequences properly.
> * The e-mails are likely to be considered as spam until you mark one as non-spam in your e-mail
>   client.

## Initial Author

[Aurelien Lourot](http://lourot.com/)
