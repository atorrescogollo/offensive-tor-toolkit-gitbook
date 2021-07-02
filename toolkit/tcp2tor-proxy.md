---
description: >-
  Victim allows another victim to access a Hidden Service through it. Useful to
  gaining access to victims in isolated networks.
---

# tcp2tor-proxy

This tool allows the victim to set up a **service that forwards TCP traffic to a Hidden Service**. Then, an victim inside an internal network can access a Hidden Service pivoting over the victim that executes this tool.

Some parameters need to be set:

```text
$ ./tcp2tor-proxy -h
Usage of tcp2tor-proxy:

  -listen string
        TCP Socket to listen on. Format: [<IP>]:<PORT> (default "127.0.0.1:60101")
  -onion-forward string
        Hidden service to proxy. Format: <ONION>:<PORT>. This parameter is required
  -timeout int
        Timeout in seconds for Tor setup (default 180)
```

