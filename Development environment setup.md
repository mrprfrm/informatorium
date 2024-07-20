---
authors:
  - Anton Petrov
status: Draft
tags:
  - python
  - node
  - neovim
  - zsh
  - git
---
## Homebrew setup

First, let's install Homebrew. We can simply run the script from [brew.sh](https://brew.sh/) right from the default terminal:

```zsh
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After Homebrew installation completed we can check it's available:

```zsh
$ brew -v
Homebrew 4.1.16
```

> We also may need to add the `brew` into `PATH` variable, if it's so, just follow the instructions from terminal.

> The version check is pretty common way to ensure that the tool is available from terminal, next we going to use it for each installation.

## zsh setup

Now let's install `zsh` command shell if we don't have it installed yet, if we have then this step can be skipped.

```zsh
$ brew install zsh
$ zsh --version
zsh 5.9 (x86_64-apple-darwin22.0)
```

Next, let's install OhMyZsh the extendable framework for the zsh shell:

```zsh
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Then let's install plugins we going to use with zsh:

```zsh
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
$ git clone https://github.com/jeffreytse/zsh-vi-mode $ZSH_CUSTOM/plugins/zsh-vi-mode
```

Now, when OhMyZsh is installed, let's add couple plugins and set the theme of the shell. First, we need to open the `~/.zshrc` config file with any preferable editor, and add the strings there:

```zshrc
# --snip--
ZSH_THEME="eastwood"
# --snip--
plugins=(git zsh-syntax-highlighting zsh-autosuggestions zsh-vi-mode direnv)
# --snip--
```

Next, after save the changes, we can reload Zsh session:

```zsh
$ source ~/.zshrc 
```

## Git setup

Now, let's install Git the version control tool:

```zsh
$ brew install git
$ git -v
git version 2.39.2 (Apple Git-143)
```

The next installation candidate is Lazygit the Git in-terminal interface.

```zsh
$ brew install lazygit
$ lazygit -v
commit=, build date=, build source=homebrew, version=0.40.2, os=darwin, arch=arm64, git version=2.42.0
```

> There is also the option install tap version of `lazygit`, it's more frequently updating version, but it also might be less stable.

Now, let's create ssh key to start using ssh connections, for instance for the github.com:

```zsh
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

> This command creates two files: public and private ssh key. The public key we can add to services, which supports ssh connection (for instance github.com), and the private key stays private on our side.

Next let's configure ssh-agent on the local machine. First let's run the agent in a background:

```zsh
$ eval "$(ssh-agent -s)"
Agent pid 1914
```

> Be aware of using third party ssh agents even installed by Homebrew or any others package managers, It might be unsafe. MacOS has built-in ssh-agent.

Next, let's set up ssh-agent configuration. First, let's check if agent's config already exists:

```zsh 
$ cat ~/.ssh/config
```

If it doesn't, let's create it manually:

```zsh
$ touch ~/.ssh/config
```

Now, let's set the ssh-agent's configuration for the github.com using any preferred editor:

```text
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

And finally we can add the new ssh key on github.com.

## ITerm2 setup

Now, let's install ITerm2:

```zsh
$ brew install --cask iterm2
```

Next, let's download additional configurations for terminal and tools:

```url
$ git clone git@github.com:mrprfrm/ws-setup.git
```

Now, from the ITerm2 settings we can import a colour scheme we just downloaded. 

Next, let's install Nerd Font:

```zsh
$ brew tap homebrew/cask-fonts
$ brew install font-hack-nerd-font
```

As well as colour

 scheme we can set new font from the ITerm2 settings.

## Node/JavaScript setup

Now, we going to install NodeJS, but first let's install node version manager NVM:

```zsh
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

After the installation, we need to restart terminal to start using nvm, or we can also run the instructions after installation completion.

Now, we can install any preferable version of node. Let's install the last stable version:

```zsh
$ nvm install --lts
$ node --version
v20.9.0
```

And then, let's make it default, if needed:

```zsh
$ nvm alias default lts/*
```

Now, let's install Prettier the JavaScript code formatter:

```zsh
$ npm install -g --save-exact prettier
$ npx prettier -v
3.0.3
```

## Python setup

Now, let's install python interpreter and related tools. Usually there is already should be installed some version of python out of the box in the operation system, but event if it's so, we may need to have different version of Python, so first, let's install Pyenv the python version manager:

```zsh
$ brew install pyenv
$ pyenv --version
pyenv 2.3.31
```

Then, let's set up shell environment to for Pyenv:

```zsh
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

Next, let's a new version of Python and make it default:

```zsh
$ pyenv install 3.10.13
ModuleNotFoundError: No module named '_lzma'
WARNING: The Python lzma extension was not compiled. Missing the lzma lib?
Installed Python-3.10.13 to /Users/antion/.pyenv/versions/3.10.13
```

Python installation also required `lzma lib`, so in case if we faced the error like this: `ModuleNotFoundError: No module named '_lzma'` we may try to install `xz` lib first and then reinstall needed version of Python using Pyenv.

```zsh
$ brew install xz
$ pyenv uninstall 3.10.13
$ pyenv install 3.10.13
Installed Python-3.10.13 to /Users/antion/.pyenv/versions/3.10.13
```

Now, let's make the new version of python default:

```zsh
$ pyenv global 3.10.13
$ pyenv versions
  system
* 3.10.13 (set by /Users/antion/.pyenv/version)
```

Then we can install Python linters and formatters using python package manager:

```zsh
$ pip3 install black flake8 isort
Successfully installed black-23.10.1 click-8.1.7 flake8-6.1.0 isort-5.12.0 mccabe-0.7.0 mypy-extensions-1.0.0 packaging-23.2 pathspec-0.11.2 platformdirs-3.11.0 pycodestyle-2.11.1 pyflakes-3.1.0 tomli-2.0.1 typing-extensions-4.8.0
```

## Rust setup

Here, we going to install Rust. It's required for couple Python's and Lua's package and tools. Also it's a great choice for performance critical parts of the systems. Let's install it first:

```zsh
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

> After installation we might need to restart terminal session to make `$PATH` variable adjustments work.

Now, we can check the `rustup` and `cargo` versions.

```zsh
$ rustup -V
rustup 1.26.0 (5af9b9484 2023-04-05)

$ cargo -V
cargo 1.73.0 (9c4383fb5 2023-08-26)
```

Then, let's install rust formatter:

```zsh
$ rustup component add rustfmt
$ rustfmt -V
rustfmt 1.6.0-stable (cc66ad46 2023-10-03)
```

Also let's install rust diagnostic tool:

```zsh
$ cargo install languagetool-rust --features full
```

## Lua setup

Now, we going to install Lua. This language is built in for nvim. The most nvim plugins and their configurations using this language. Let's install it:

```zsh
$ brew install lua
$ lua -v
Lua 5.4.6  Copyright (C) 1994-2023 Lua.org, PUC-Rio
```

After Lua installation completed, let's install LuaRocks the Lua package manager:

```zsh
$ brew install luarocks
$ luarocks --version
/opt/homebrew/bin/luarocks 3.9.2
```

Next, let's install Lua linter Luacheck:

```zsh
$ luarocks install luacheck
```

Then, let's install Lua formatter StyLua. We going to install it using Rust crates:

```zsh
$ cargo install stylua
$ stylua -V
stylua 0.18.2
```

By default Stylua crates can support different version of Lua than we have installed. In this case we can simply install Stylua for particular Lua version:

```zsh
$ cargo install stylua --features lua54
```

## NeoVim setup

Now we ready to install and configure Neovim. First let's install it using Homebrew:

```zsh
$ brew install neovim
$ nvim -v
NVIM v0.9.4
```

Next, configure our local instance of Neovim. In our case we going to use existing configuration of Neovim from [mrprfrm/nvim](https://github.com/mrprfrm/nvim). Let's figure out if the folder with nvim configuration exist:

```zsh
$ ls ~/.config
iterm2
```

In case if it doesn't let's simply clone the configuration form the mentioned repository:

```zsh
$ git clone git@github.com:mrprfrm/nvim.git
```

Before we going to install plugins we need to install the Packer the nvim package manager. To install it let's run the command in the shell:

```zsh
$ git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

Next, let's install the plugins for Neovim:

```zsh
$ nvim +PackerSync
```

> Before we'll be able to enjoy all advantages of Neovim we might need to install additional packages for plugins.

> Also there a chance the icons we using in plugins configuration are already deprecated and in this case we probably need to replace broken ones manually.

Now, let's install `ripgrep` for more performed search via telescope.

```zsh
$ brew install ripgrep
$ rg -V
ripgrep 13.0.0
```

> Without this tool the Telescope plugin might not work at all. 

Next, let's setup copilot:

```zsh
$ nvim +Copilot setup
```

Copilot plugin should offer us to pass the authentication using browser, here we just need to follow the instructions.

After the first start, LSPInstall should run installation for all LSP servers from configuration, but some of these installations may have need to be run manually:

```zsh
$ brew install lua-language-server
```

Also if we not sure that some of LSP or formatters are executable, we can check it right from Neovim command line:

```nvim
:echo executable("stylua")
1

:echo executable("lua-language-server")
1
```

Also we might need to set up `bat` the enhanced version of `cat`:

```zsh
$ brew install bat
```

> Out of the box `bat` supports couple color schemes and we can set one of them as a default theme by adding the environment valuable `BAT_THEME` to the `.zshrc`:
## Sources list

- [ohmyz.sh](https://ohmyz.sh/)
- [Generating a new SSH key and adding it to the ssh-agent (github.com)](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [jesseduffield/lazygit (github.com)](https://github.com/jesseduffield/lazygit)
- [15 Lazygit Features In Under 15 Minutes (youtu.be)](https://www.youtube.com/watch?v=CPLdltN7wgE)
- [nvm-sh/nvm (github.com)](https://github.com/nvm-sh/nvm)
- [ModuleNotFoundError: No module named _lzma (gist.github.com)](https://gist.github.com/iandanforth/f3ac42b0963bcbfdf56bb446e9f40a33)
- [pyenv/pyenv (github.com)](https://github.com/pyenv/pyenv)
- [rust-lang/rustfmt (github.com)](https://github.com/rust-lang/rustfmt)
- [jeertmans/languagetool-rust (github.com)](https://github.com/jeertmans/languagetool-rust)
- [luarocks.org](https://luarocks.org/)
- [lunarmodules/luacheck (github.com)](https://github.com/lunarmodules/luacheck)
- [JohnnyMorganz/StyLua (github.com)](https://github.com/JohnnyMorganz/StyLua)
- [wbthomason/packer.nvim (github)](https://github.com/wbthomason/packer.nvim)
- [github/copilot.vim (github.com)](https://github.com/github/copilot.vim)
- [jose-elias-alvarez/null-ls.nvim (github.com)](https://github.com/jose-elias-alvarez/null-ls.nvim)