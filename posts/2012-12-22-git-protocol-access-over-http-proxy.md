---
layout: post
title: "Git Protocol Access Over Http Proxy"
date: 2012-12-22 00:00
comments: true
categories: corkscrew git
---

Git protocol(git://) is the fastest way to access to a git repository. However, firewall sometime prevent this protocol.

Recently we can use ssh protocol instead of git protocol to the Github repositories. However many other git repositories don’t have such alternatives.

In that case, ‘corkscrew’ enable you to use git protocol over http proxy.

Install corkscrew.

```sh
sudo apt-get install corkscrew
```

Create a script called ‘git-proxy.sh’.

```sh
#!/bin/sh
corkscrew <proxy host> <proxy port> $1 $2
```

Setup environment variable called GIT_PROXY_COMMAND.

```sh
chmod 755 /path/to/git-proxy.sh
export GIT_PROXY_COMMAND=/path/to/git-proxy.sh
```

Another approach is using ‘socat’. I havn’t yet tried that so refer to the following links, please.

- [How to use git over an HTTP proxy, with socat](https://gitolite.com/tips/git-over-proxy.html)
- [How to use the git protocol through a HTTP CONNECT proxy](https://www.emilsit.net/blog/archives/how-to-use-the-git-protocol-through-a-http-connect-proxy/)
