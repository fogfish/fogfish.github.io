---
layout: post
title:  Development Environment Checklist
description: |
  Checklist to setup development environment from scratch.
---

# Development Environment Checklist

Checklist to setup development environment from scratch 

* AppleID
* XCode
* [iTerm2](https://iterm2.com)
  - color palette [Solarized](https://ethanschoonover.com/solarized/)
* [oh my zsh](https://ohmyz.sh)
  - theme [agnoster](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes#agnoster)
  - patch `prompt_context` at agnoster theme with `prompt_segment black default "λ"`
* [Brew](https://brew.sh)
* [Visual Studio Code](https://code.visualstudio.com)
  - Code Spell Checker
  - [Monokai Operator](https://marketplace.visualstudio.com/items?itemName=markfknight.monokai-operator-theme) / [Identical Sublime Monokai](https://marketplace.visualstudio.com/items?itemName=maximetinu.identical-sublime-monokai-csharp-theme-colorizer)
  - Docker
  - Erlang
  - Go
* [Source Code Pro](https://github.com/adobe-fonts/source-code-pro)
* [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
* [Wireshark](https://www.wireshark.org/download.html)
* [Burp Suite](https://portswigger.net/burp)
* `brew install node`
* `npm install -g yarn`
* `npm install -g typescript ts-node`
* `npm install -g aws-cdk`
* `brew install python3`
* `python3 -m pip install virtualenv`
* `pip3 install awscli --upgrade`
* [Go](https://golang.org/dl/)
  - `export PATH=/usr/local/go/bin:$PATH`
  - `export GOPATH=$HOME/devel/go`
* `brew install dep`
* `brew install openssl`
* [Erlang](https://www.erlang.org/downloads)
  - `./configure --prefix=/usr/local/otp_22.0 --enable-threads --enable-smp-support --enable-kernel-poll --with-ssl=/usr/local/opt/openssl`
* [Scala](https://www.scala-lang.org)
  - `curl -s "https://get.sdkman.io" | bash` use [sdkman](https://sdkman.io/install)
  - `sdk install java 11.0.5.hs-adpt`
  - `brew install sbt`
  - Visual Studio Code plugins: Scala (Metals)

