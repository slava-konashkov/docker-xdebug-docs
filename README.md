# Docker (Mac) De-facto Standard Host Address Alias

This launchd script will ensure that your Docker environment on your Mac will have `10.254.254.254` as an alias on your loopback device (127.0.0.1).  The command being run is `ifconfig lo0 alias 10.254.254.254`.

Once your machine has a well known IP address, your PHP container will then be able to connect to it, specifically XDebug can connect to it at the configured `xdebug.remote_host`.

### Installation Of IP Alias (This survives reboot)

Copy/Paste the following in terminal with sudo (must be root as the target directory is owned by root)...

```bash
sudo curl -o /Library/LaunchDaemons/com.ralphschindler.docker_10254_alias.plist https://gist.githubusercontent.com/ralphschindler/535dc5916ccbd06f53c1b0ee5a868c93/raw/com.ralphschindler.docker_10254_alias.plist
```

Or copy the above Plist file to /Library/LaunchDaemons/com.ralphschindler.docker_10254_alias.plist

Next and every successive reboot will ensure your lo0 will have the proper ip address.

Finally, make sure to configure your xdebug correctly.  However you get your `xdebug.remote_host` into the container, ensure it has similar settings:

```ini
zend_extension=xdebug.so
xdebug.remote_host=10.254.254.254
xdebug.remote_enable=1
xdebug.remote_autostart=1
```

It is also useful to pass the following environment variable into your container:

`PHP_IDE_CONFIG="serverName=localhost"`

PHPStorm will use `localhost` as the server name when you setup the `Preferences > PHP > Debugging` profile.

#### Why?

Because `docker.local` is gone. This seems to be the easiest way to setup xdebug to connect back to your IDE running on your host.  Similarly, this is a solution for any kind of situations where a container needs to connect back to the host container at a known ip address.

# PHPStorm IDE setup
## Setup DBGp Proxy
![DBGp Proxy](https://raw.githubusercontent.com/SunDrop/docker-xdebug-docs/master/phpstorm-dbg-proxy.png)
## Setup Servers
For each domain and posp setup configuration and map path from local to docker
![Servers](https://raw.githubusercontent.com/SunDrop/docker-xdebug-docs/master/phpstorm-servers.png)