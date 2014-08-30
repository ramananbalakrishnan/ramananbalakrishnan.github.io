---
layout: post
title:  'Getting beyond those pesky iptables rules'
subtitle: 'aka SSH Local Port Forwarding'
categories: 
tags:
---

Most of the development at [Semantics3](https://semantics3.com) occurs on a shared development machine hosted at AWS. While this helps us control our dev environment with high precision, it causes some difficulties when trying to test features on a local computer. For example, connecting to a test database might be impossible, given that most ports from the dev machine are cutoff from the outside world.

This is where SSH comes to the rescue using its local port forwarding or "outbound" tunneling feature.

```
$ ssh -L [bind_address:]port:host:hostport [user@]hostname
    Specifies that the given port on the local (client) host is
    to be forwarded to the given host and port on the remote side.
```

As shown below, SSH can be used to setup a tunnel to access `private-db:5432` from your `localhost` via a `public-host`.

```
                                +-----------------------------------+
                                |                                   |
+-----------------+          +-------------+     +-----------------+|
| localhost:15432 |---SSH---\| public-host |     | private-db:5432 ||
|            ==============================|==============>        ||
|                 |---------/|             |     |                 ||
+-----------------+          +-------------+     +-----------------+|
                                |                                   |
                                |        private network            |
                                +-----------------------------------+
```


The command to setup the tunnel above is

```
$ ssh -L 15432:private-db:5432 user@public-host
```

Now, you can simply use your client of choice to connect to the database as if it were on your local computer.

```
$ psql -h localhost -p 15432 -d testdatabase
```

A few notes:

  - `AllowTCPForwarding` needs to be set to `yes` in your server's ssh config for this to work.
  - Additional flags `-nNT` can be used to suppress any output and avoid terminal allocation, as we just need port forwarding.
  - If this piqued your interest, I also highly recommend the talk  [The Black Magic of SSH / SSH Can Do That?](http://bjeanes.com/2012/11/talk-ssh-can-do-that) by [bjeanes](https://github.com/bjeanes).

> originally appeared in the [Engineering@Semantics3](https://engineering.semantics3.com/2016/05/22/ssh-local-port-forwarding/) blog
