
<!--
Generated by:
$ ./scripts/gen_doc.sh
-->

[![Build Status](https://travis-ci.org/AurelienLourot/github-commit-watcher.svg?branch=master)](https://travis-ci.org/AurelienLourot/github-commit-watcher)

Official documentation [here](http://lourot.com/gicowa).

# gicowa.py - GitHub Commit Watcher

GitHub's _Watch_ feature doesn't send notifications when commits are pushed.
This script aims to implement this feature and much more.

> **Call for maintainers:** I don't use this project myself anymore but IFTTT
instead (see below). If you're interested in taking over the maintenance of
this project, or just helping, please let me know (e.g. by opening an issue).

## Installation

    
    
    $ sudo apt-get install sendmail
    $ sudo pip install gicowa
    

## Quick setup

Add the following line to your `/etc/crontab`:

    
    
    0 * * * * root gicowa --persist --no-color --mailto myself@mydomain.com lastwatchedcommits MyGitHubUsername sincelast > /tmp/gicowa 2>&1
    

**That's it.** As long as your machine is running you'll get e-mails when something gets pushed on a repo you're watching.

> **NOTES:**

>

>   * The e-mails are likely to be considered as spam until you mark one as
non-spam in your e-mail client. Or use the `--mailfrom` option.

>   * If you're watching 15 repos or more, you probably want to use the
`--credentials` option to make sure you don't hit the GitHub API rate limit.

## Other/Advanced usage

`gicowa` is a generic command-line tool with which you can make much more that
just implementing the use case depicted in the introduction. This section
shows what it can.

### List repos watched by a user

    
    
    $ gicowa watchlist AurelienLourot
    watchlist AurelienLourot
    AurelienLourot/uncommitted
    AurelienLourot/crouton-emacs-conf
    brillout/FasterWeb
    AurelienLourot/github-commit-watcher
    

### List last commits on a repo

    
    
    $ gicowa lastrepocommits AurelienLourot/github-commit-watcher since 2015 07 05 09 12 00
    lastrepocommits AurelienLourot/github-commit-watcher since 2015-07-05 09:12:00
    Last commit pushed on 2015-07-05 10:48:58
    Committed on 2015-07-05 10:46:27 - Aurelien Lourot - Minor cleanup.
    Committed on 2015-07-05 09:39:01 - Aurelien Lourot - watchlist command implemented.
    Committed on 2015-07-05 09:12:00 - Aurelien Lourot - argparse added.
    

* * *

> **NOTES:**

>

>   * Keep in mind that a commit's _committer timestamp_ isn't the time at
which it gets pushed.

>   * The lines starting with `Committed on` list commits on the `master`
branch only. Their timestamps are the _committer timestamps_.

>   * The line starting with `Last commit pushed on` shows the time at which a
commit got pushed on the repository for the last time on any branch.

### List last commits on repos watched by a user

    
    
    $ gicowa lastwatchedcommits AurelienLourot since 2015 07 04 00 00 00
    lastwatchedcommits AurelienLourot since 2015-07-04 00:00:00
    AurelienLourot/crouton-emacs-conf - Last commit pushed on 2015-07-04 17:10:18
    AurelienLourot/crouton-emacs-conf - Committed on 2015-07-04 17:08:48 - Aurelien Lourot - Support for Del key.
    brillout/FasterWeb - Last commit pushed on 2015-07-04 16:40:54
    brillout/FasterWeb - Committed on 2015-07-04 16:38:55 - brillout - add README
    AurelienLourot/github-commit-watcher - Last commit pushed on 2015-07-05 10:48:58
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 10:46:27 - Aurelien Lourot - Minor cleanup.
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 09:39:01 - Aurelien Lourot - watchlist command implemented.
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 09:12:00 - Aurelien Lourot - argparse added.
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 09:07:14 - AurelienLourot - Initial commit
    

* * *

> **NOTE:** if you're watching 15 repos or more, you probably want to use the
`--credentials` option to make sure you don't hit the GitHub API rate limit.

### List last commits since last run

Any listing command taking a `since <timestamp>` argument takes also a
`sincelast` one. It will then use the time where that same command has been
run for the last time on that machine with the option `--persist`. This option
makes `gicowa` remember the last execution time of each command in
`~/.gicowa`.

    
    
    $ gicowa --persist lastwatchedcommits AurelienLourot sincelast
    lastwatchedcommits AurelienLourot since 2015-07-05 20:17:46
    $ gicowa --persist lastwatchedcommits AurelienLourot sincelast
    lastwatchedcommits AurelienLourot since 2015-07-05 20:25:33
    

### Send output by e-mail

You can send the output of any command to yourself by e-mail:

    
    
    $ gicowa --no-color --mailto myself@mydomain.com lastwatchedcommits AurelienLourot since 2015 07 04 00 00 00
    lastwatchedcommits AurelienLourot since 2015-07-04 00:00:00
    AurelienLourot/crouton-emacs-conf - Last commit pushed on 2015-07-04 17:10:18
    AurelienLourot/crouton-emacs-conf - Committed on 2015-07-04 17:08:48 - Aurelien Lourot - Support for Del key.
    brillout/FasterWeb - Last commit pushed on 2015-07-04 16:40:54
    brillout/FasterWeb - Committed on 2015-07-04 16:38:55 - brillout - add README
    AurelienLourot/github-commit-watcher - Last commit pushed on 2015-07-05 10:48:58
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 10:46:27 - Aurelien Lourot - Minor cleanup.
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 09:39:01 - Aurelien Lourot - watchlist command implemented.
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 09:12:00 - Aurelien Lourot - argparse added.
    AurelienLourot/github-commit-watcher - Committed on 2015-07-05 09:07:14 - AurelienLourot - Initial commit
    Sent by e-mail to myself@mydomain.com
    

* * *

> **NOTES:**

>

>   * You probably want to use `--no-color` because your e-mail client is
likely not to render the bash color escape sequences properly.

>   * The e-mails are likely to be considered as spam until you mark one as
non-spam in your e-mail client. Or use the `--mailfrom` option.

## Changelog

**1.2.3** (2015-10-17) to **1.2.5** (2015-10-19):

  * Exception on non-ASCII characters fixed.

**1.2.2** (2015-10-12):

  * Machine name appended to e-mail content.

**1.2.1** (2015-08-20):

  * Documentation improved.

**1.2.0** (2015-08-20):

  * `--version` option implemented.

**1.1.0** (2015-08-20):

  * `--errorto` option implemented.

**1.0.1** (2015-08-18) to **1.0.9** (2015-08-19):

  * Documentation improved.

## Contributors

  * [Aurelien Lourot](http://lourot.com/)
  * [cassiodoroVicinetti](https://github.com/cassiodoroVicinetti)

## Similar projects

The following projects provide similar functionalities:

  * [IFTTT](https://ifttt.com/), see [this post](http://www.warski.org/blog/2013/04/per-commit-e-mail-github-notifications/).
  * [Zapier](https://zapier.com/), however you have to create a "Zap" for each single project you want to watch. See [this thread](http://webapps.stackexchange.com/questions/62701/github-receive-updates-of-starred-projects-via-email-of-commits-on-master-branch).
  * [HubNotify](http://hubnotify.com/) and [Sibbell](https://sibbell.com/), however they will notify you only for new tags, not new commits.

