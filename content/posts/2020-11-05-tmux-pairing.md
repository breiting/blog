---
title: "Pair programming with tmux"
date: 2020-11-05
type: post
tags: ['tmux', 'programming' ]
---

[tmux](https://github.com/tmux/tmux) is a terminal multiplexer and is a perfect
tool for organizing sessions in your terminal.

However, it can also be used to setup a pair programming session between two
developers. Assuming that both developers are logged into the same machine, a
new shared tmux session can be spawned as follows

```sh
$ tmux -S /tmp/tmux-shared new -s shared
```

This opens up a new tmux session with the name `shared` and a socket on the
local machine `/tmp/tmux-shared`. If you take a look at the file with `ls  -la`
you will see that the file is marked with `s`.

```sh
srwxrwx--- 1 breiting breiting Nov  5 09:22 /tmp/tmux-shared
```

The socket is only accessible by the owner and by the group. So you have to
make sure that the group is properly set, and the other user is also part of
this group (e.g. `users`).

```sh
chgrp users /tmp/tmux-shared
```

Once this is done, the other developer is good to join with the following
command.

```sh
tmux -S /tmp/tmux-shared attach -t shared
```

This allows both developers to work in the same terminal. However, if the second
developer just wants to join read-only, just add the `-r` flag in the command
above.

