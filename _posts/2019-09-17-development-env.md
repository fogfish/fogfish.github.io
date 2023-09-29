---
layout: post
title:  Development Environment Checklist
description: |
  Checklist to setup development environment from scratch.
---

# Development Environment Checklist

Checklist to setup development environment from scratch 

## Primary

* AppleID
* [sudo with touch id](https://apple.stackexchange.com/questions/259093/can-touch-id-on-mac-authenticate-sudo-in-terminal)
* XCode
* [iTerm2](https://iterm2.com)
  - color palette [Solarized](https://ethanschoonover.com/solarized/)
* [oh my zsh](https://ohmyz.sh)
  - theme [agnoster](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes#agnoster)
  - Install Powerline fonts https://github.com/powerline/fonts
  - patch `prompt_context` at agnoster theme with `prompt_segment black default "λ"`
* [Brew](https://brew.sh)
  - `brew analytics off`
  - `/opt/homebrew/bin/brew shellenv` add to `.zshrc`
* [Go](https://golang.org/dl/)
  - `export PATH=/usr/local/go/bin:$PATH`
  - `export GOPATH=$HOME/devel/go`
* [Visual Studio Code](https://code.visualstudio.com)
  - Code Spell Checker
  - [Monokai Operator](https://marketplace.visualstudio.com/items?itemName=markfknight.monokai-operator-theme) / [Identical Sublime Monokai](https://marketplace.visualstudio.com/items?itemName=maximetinu.identical-sublime-monokai-csharp-theme-colorizer)
  - Hex Editor (Microsoft)
  - Go
  - Flutter
  - bloc
  - Erlang
* [AWS Commandline Utility v2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html)
  - `brew install awscli`
* [Flutter](https://docs.flutter.dev/get-started/install/macos#downloading-straight-from-github-instead-of-using-an-archive)
  - `cd ~/Library`
  - `git clone https://github.com/flutter/flutter.git -b stable`
  - `export PATH=~/Library/flutter/bin:$PATH`
  - `flutter doctor`
  - `sudo gem install cocoapods`
* TypeScript
  - `brew install node@18`
  - `npm install -g yarn`
  - `npm install -g typescript ts-node`
  - `npm install -g aws-cdk`
* DaisyDisk
* Inkscape
  - `brew install inkscape`
* [Burp Suite](https://portswigger.net/burp)
* [Wireshark](https://www.wireshark.org/download.html)
* [Erlang](https://www.erlang.org/downloads)
  - `./configure --prefix=/usr/local/otp_22.0 --enable-threads --enable-smp-support --enable-kernel-poll --with-ssl=/usr/local/opt/openssl`
* [Scala](https://www.scala-lang.org)
  - `curl -s "https://get.sdkman.io" | bash` use [sdkman](https://sdkman.io/install)
  - `sdk install java 11.0.5.hs-adpt`
  - `brew install sbt`
  - Visual Studio Code plugins: Scala (Metals)

## Legacy checklist

* [Source Code Pro](https://github.com/adobe-fonts/source-code-pro)
* [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
* `brew install python3`
* `python3 -m pip install virtualenv`
* `brew install dep`
* `brew install openssl`
