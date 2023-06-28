---
tags:
 - Rust
 - Nvim
 - LSP
---

## lsp-config

В конфигурации серверов [[lsp-config | lsp-config]] имеется несколько опций для работы с [[Rust]]

### rust-analyzer

- [GitHub](https://github.com/rust-lang/rust-analyzer)
- [GitHub Pages](https://rust-analyzer.github.io/manual.html)

На самом деле `rust-analyzer` — это библиотека для семантического анализа кода на `Rust`, которая поддерживает специфический кейс в виде запуска сервера с поддержкой [[LSP]].

### Установка

Самый очевидный и низкоуровневый способ поставить `rust-analyzer` — это установить библиотеку на своей машине.

Чтобы добавить ресурс вручную, можно воспользоваться командой:

```
rustup component add rust-src
```

Также возможно просто добавить `rust-analyzer` в систему используя `nvim-lsp-installer`.

Для этого можно либо явно кликнуть на `rust_analyzer` открыв окно `nvim-lsp-installer`

```
:LspInstallInfo
```

> TODO: Здесь можно вставить картинку

Либо используя shell

```
:LspInstall rust_analyzer
```

> TODO: Здесь также можно вставить картинку