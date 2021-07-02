# Download

## Using Releases

{% hint style="success" %}
It is the **recommended** approach.
{% endhint %}

All you need to do is to download the latest version in the ****[**release section**](https://github.com/atorrescogollo/offensive-tor-toolkit/releases) ****of the repository.

```bash
export VERSION=<VERSION>

# Download the release
wget https://github.com/atorrescogollo/offensive-tor-toolkit/releases/download/${VERSION}/offensive-tor-toolkit-${VERSION}.tar.gz

# Uncompress
tar -xvzf offensive-tor-toolkit-${VERSION}.tar.gz

# Move to /opt/offensive-tor-toolkit/
sudo mv offensive-tor-toolkit-${VERSION}* /opt
sudo ln -sf offensive-tor-toolkit-${VERSION} /opt/offensive-tor-toolkit
```

## Using Docker Image

In order to download the toolkit inside a docker container, there is an [image published in Docker Hub](https://hub.docker.com/r/atorrescogollo/offensive-tor-toolkit).

### Download binaries from image

Create a temporary container from the image and copy the `/dist` folder to your host:

```bash
docker create --name ott \
    atorrescogollo/offensive-tor-toolkit:<VERSION>
docker cp ott:/dist ./dist
docker rm ott
```

{% hint style="info" %}
You can find the latest version by accessing the [release section](https://github.com/atorrescogollo/offensive-tor-toolkit/releases) of the repository.
{% endhint %}

Now, you will find the toolkit inside the folder you specified \(`/dist` in the example\):

```bash
$ du -hs ./dist/*
22M     dist/check-tor-connection
19M     dist/hidden-bind-shell
19M     dist/hidden-echo-server
19M     dist/hidden-portforwarding
19M     dist/hidden-socks5
19M     dist/reverse-shell-over-tor
19M     dist/reverse-shell-over-tor-simplehandler
19M     dist/tcp2tor-proxy
```

### Use the toolkit from the container

The toolkit is statically compiled so it can be run directly from the container:

```bash
docker run -it --rm -v "$(pwd)/work:/work" \
    atorrescogollo/offensive-tor-toolkit:<VERSION>
```

For example, to check if you can access Tor from the container, execute the following tool:

```bash
$ /dist/check-tor-connection
...
Title: Congratulations. This browser is configured to use Tor.
Closing Tor
Write line: SIGNAL HALT
Read line: 250 OK
Write line: QUIT
```

### Build your own Docker image

Probably is a good idea to have multiple tools inside a single Docker image. To include the Offensive Tor Toolkit inside your custom image you can use a multi-stage build:

```sql
# Dockerfile
FROM atorrescogollo/offensive-tor-toolkit:<VERSION> as ott

FROM kalilinux/kali-rolling:latest
COPY --from=ott /dist /opt/offensive-tor-toolkit
```

```bash
$ docker build -t mykali .
$ docker run -it --rm mykali
┌──(root💀ca67e068141f)-[/]
└─# du -hs /opt/offensive-tor-toolkit/*
22M     /opt/offensive-tor-toolkit/check-tor-connection
19M     /opt/offensive-tor-toolkit/hidden-bind-shell
19M     /opt/offensive-tor-toolkit/hidden-echo-server
19M     /opt/offensive-tor-toolkit/hidden-portforwarding
19M     /opt/offensive-tor-toolkit/hidden-socks5
19M     /opt/offensive-tor-toolkit/reverse-shell-over-tor
19M     /opt/offensive-tor-toolkit/reverse-shell-over-tor-simplehandler
19M     /opt/offensive-tor-toolkit/tcp2tor-proxy
```

