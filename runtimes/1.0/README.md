# Runtime 1.0

## Stack
* [PHP 7.4](https://www.litespeedtech.com/open-source/litespeed-sapi/php)
* [NodeJS 14](https://nodejs.org/en/)
* [Caddy 2](https://caddyserver.com/v2)
* [Openlitespeed](https://openlitespeed.org/)
* [Supervisor](http://supervisord.org/)
* [Composer](https://getcomposer.org/)
* [WPCLI](https://wp-cli.org/)

## Volumes

The default vhost is serving files from `/opt/runtime/app/public` and logs are written to `/opt/runtime/app/logs`.

## Ports

* Caddy Proxy: `80` (http) and `443` (https)
* Openlitespeed: `8080` (http)

## Environment

* `RUNTIME_USER_ID`: the user's ID which is used to run the application (default: `10000`).
* `RUNTIME_GROUP_ID`: the user's group ID which is used to run the application (default: `10000`).

## Configuration 

You can override the default runtime configuration by mounting configuration files to these locations:

### Litespeed
* `/opt/runtime/config/litespeed.conf`: change the default Openlitespeed server configuration.
* `/opt/runtime/config/vhost.d/default.conf`: the default Litespeed vhost configuration.
* `/opt/runtime/config/vhost.d/<filename>.conf`: additional Litespeed vhost configurations (autoloaded).

### Caddy
* `/opt/runtime/config/caddy.conf`: the default Caddy server configuration.
* `/opt/runtime/config/caddy.d/default.conf`: the default Caddy vhost  configuration.
* `/opt/runtime/config/caddy.d/<filename>.conf`: additional Caddy vhost configurations (autoloaded).

### PHP
* `/opt/runtime/config/php.d/99-runtime.ini`: the default PHP configuration.
* `/opt/runtime/config/php.d/<filename>.ini`: additional PHP configruations (autoloaded).

### Supervisor
* `/opt/runtime/config/supervisor.conf`: the default Supervisor configuration.
* `/opt/runtime/config/supervisor.d/litespeed.conf`: the default Litespeed Supervisor configuration.
* `/opt/runtime/config/supervisor.d/caddy.conf`: the default Caddy Supervisor configuration.
* `/opt/runtime/config/supervisor.d/<filename>.conf`: additional Supervisor configurations (autoloaded).
