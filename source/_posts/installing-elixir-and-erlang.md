title: Installing Elixir and Erlang
date: 2017-06-29 20:45:25
tags:
  - elixir
  - erlang
  - asdf
  - version management
---

## Installing asdf
[asdf](https://github.com/asdf-vm/asdf) is a **version manager** with support for different languages, what is a really interesting feature.

We will use it to manage the installation of Erlang and Elixir and to stay up to date with such technologies.

> **Disclaimer**: since I'm using the Linux Mint OS, all installing instructions will be based on such OS. After each section I'll point you to the official installation instructions so you can see how to do it for your OS.

Let's install asdf!

```sh
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.3.0

echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.zshrc # if you use zsh shell
# OR echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.bashrc

echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.zshrc
# OR echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.bashrc
```

For most plugins, it is good if you have installed the following packages OR their equivalent on your OS:

```sh
sudo apt-get install automake autoconf libreadline-dev libncurses-dev libssl-dev libyaml-dev libxslt-dev libffi-dev libtool unixodbc-dev
```

For a different OS, follow the setup [here](https://github.com/asdf-vm/asdf#setup).

## Installing Erlang
Now that we have asdf installed, we need to install a *asdf plugin* for each language we want to have available on our machine.

Let's install the [asdf-erlang](https://github.com/asdf-vm/asdf-erlang) plugin and Erlang itself!

```sh
# Install asdf-erlang
asdf plugin-add erlang https://github.com/asdf-vm/asdf-erlang.git

# Install Erlang
asdf install erlang 20.0

# Set current version
asdf global erlang 20.0

# Open other terminal tab
# Verify if everything went well by
# Checking the current Erlang installed version
erl -v
# Erlang/OTP 20 ...
```

For more instructions about asdf usage check [this](https://github.com/asdf-vm/asdf#usage) section.

## Installing Elixir
Let's install the [asdf-elixir](https://github.com/asdf-vm/asdf-elixir) plugin and Elixir.

> :warning: :warning: :warning:
>
> If you would like to use precompiled binaries built with a more recent OTP, you should append `-otp-${OTP_VERSION}` to any installable version that can be given to `asdf-elixir`.
>
> See more about it [here](https://github.com/asdf-vm/asdf-elixir#elixir-precompiled-versions).
>
> :warning: :warning: :warning:

```sh
# Install asdf-elixir
asdf plugin-add elixir https://github.com/asdf-vm/asdf-elixir.git

# Install Elixir
asdf install elixir 1.5.0-otp-20

# Set current version
asdf global elixir 1.5.0-otp-20

# Open other terminal tab
# Verify if everything went well by
# Checking the current Elixir installed version
elixir -v
# Elixir 1.5.0-otp-20
```
