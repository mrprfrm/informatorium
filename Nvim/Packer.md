---
authors:
  - Anton Petrov
status: Draft
tags:
  - Nvim
  - lua
---
Packer используется для управления плагинами для Neovim или Vim. Он упрощает процесс установки, обновления и удаления плагинов.

Конфигурация Packer написана на Lua, что делает его мощным и гибким. Пользователи могут писать код на Lua, чтобы определить, какие плагины установить, откуда их получить (GitHub, локальные каталоги и т. д.) и любые параметры конфигурации для каждого плагина.

Packer позволяет ленивую (lazy) загрузку плагинов, что означает, что они загружаются в память только тогда, когда они действительно нужны, что может улучшить время запуска Neovim.

Packer может устанавливать плагины параллельно, что ускоряет общий процесс, особенно при работе с большим количеством плагинов.

### Как использовать Packer:

Вот базовый пример настройки Packer в конфигурации Neovim:

Сперва следует установить Packer. В нашем случае просто клонируем репозиторий `wbthomason/packer.nvim` в директорию с конфигурацией nvim, чаще всего это директория `~/.config/nvim`.
  
```zsh
$ git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

Далее импортируем packer в корневой директории с плагинами, например `~/.config/nvim/lua/plugins/init.lua`:

```lua
local packer = require('packer')  

packer.startup(function()     
	use 'some-plugin/repository' -- Пример плагина 
end)
```

После определения списка плагинов, мы можем установить их Packer, запустив `:PackerInstall` в Neovim. Для обновления плагинов используется команда `:PackerUpdate`. А для удаления плагинов достаточно удалить или закомментировать строчку с плагином в `init.lua` и ввести команду `:PackerClean`.
### Ключевые команды:

- `:PackerSync`: Устанавливает, обновляет и очищает плагины сразу.
- `:PackerInstall`: Устанавливает определенные плагины.
- `:PackerUpdate`: Обновляет установленные плагины.
- `:PackerClean`: Удаляет плагины, которых нет в конфигурации.
- `:PackerCompile`: Компилирует конфигурацию плагинов.

### Пример конфигурации Packer:

Вот простой пример конфигурации Packer на Lua:

```lua
-- Lua
return require('packer').startup(function()
    -- Packer может управлять самим собой
    use 'wbthomason/packer.nvim'

    -- Другие плагины
    use 'tpope/vim-commentary'
    use 'neovim/nvim-lspconfig'
    use {'nvim-treesitter/nvim-treesitter', run = ':TSUpdate'}
    use 'preservim/nerdtree'
end)
```

Эта конфигуриция содержит сам Packer, а также несколько примеров плагинов: `vim-commentary`, `nvim-lspconfig`, `nvim-treesitter` и `nerdtree`.

## Список источников

-  [wbthomason/packer.nvim (github.com)](https://github.com/wbthomason/packer.nvim)
