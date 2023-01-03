# dockerize

A simple script which runs a docker container using
more locked down defaults.

Setup will automatically create a configuration directory
as well as a special user and group to run the docker container as.

## What

`dockerize` is a simple script which sets up a Docker environment on the host
machine, and attempts to run Docker containers in a slightly more restrictive
default environment.

In order to use `dockerize`, you must first symlink it to a name:

```bash
 $ ln -s dockerize adguardhome
 $ adguardhome
```

The default environment is as follows:

* `--security-opt no-new-privileges:true` is used
* `--cap-drop ALL` is used
* `--name` is used and will always match the name of the symlink. In the example above
  it is `--name=adguardhome`
* `--hostname`, which is the same as the `--name`

A custom unprivileged user `docker-{name}:dockerusers` is used to run the container.
In the example above, it is `docker-adguardhome:dockerusers`.

## Usage


In order to use `dockerize`, you must first symlink it to a name. In the example
above, we used `adguardhome`. Upon symlinking, you can then run `adguardhome`,
which will run `dockerize` and perform the following.

1. Look for a folder in `/usr/local/etc/docker/adguardhome`
  * If it does not exist, we create the folder and generate a default
    `container` configuration, and then exit.
  * If it does exist, we continue to step 2
2. The `container` configuration is sourced from
   `/usr/local/etc/docker/adguardhome/container`
3. The `container` configuration is set up, and a `docker-*` user is
   created for the given container (unless overridden), as well as a
   `dockerusers` group.
4. Any local volume paths specified in the `container` configuration are created,
   and owned by the `docker-*` user (unless overridden).
5. The Docker container is called using `docker run --init` and passed various arguments.

The systemd service `dockerize@.service` can be used to run `dockerize` scripts
via the `systemctl` command for easier management.

## License

GPLv2

```
  The GPLv2 License

    Copyright (C) 2022  Peter Kenji Yamanaka

    This program is free software; you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation; either version 2 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License along
    with this program; if not, write to the Free Software Foundation, Inc.,
    51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
```
