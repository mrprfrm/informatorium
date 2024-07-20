---
authors:
  - Anton Petrov
status: Draft
tags:
  - shell
---
## Установка

```zsh
$ brew install --cask alacritty
```

> Опция `cask` — означает установку с помощью `brew` приложения с графическим интерфейсом, для приложений командной строки или библиотек указание этой опции не требуется.

## Настройка

Начнём с настройки шрифтов. Мы будем использовать шрифт `Hack Nerd Font Mono`, поскольку он прекрасно работает с плагинами для `nvim`.

> В случае если данный шрифт отсутствует в системе, иконки в `nvim` банально не будут отображаться.
 
Определим семейство и размер шрифта в эмуляторе:

```toml
[font]
size = 16

[font.normal]
family =  "Hack Nerd Font Mono"
```

Далее, определим размер эмулятора на старте:

```toml
[window]
startup_mode = "maximized"
```

И несколько несколько опций специфичных

А также цветовую схему по аналогии с цветовой схемой `nvim`. Для этого скопируем подготовленные параметры цветовой схемы из репозитория автора nightfox, найти их можно по [ссылке](https://github.com/EdenEast/nightfox.nvim/blob/main/extra/nordfox/alacritty.toml).

Также внесём коррективы в настройки цветовой схемы для большей контрастности:

```toml
# --snip--
[colors.cursor]
text = "#4c556a"
cursor = "#d1b57f"
# --snip--

# --snip--
[colors.bright]
black = "#81899B"
# --snip--

# --snip--
[[colors.indexed_colors]]
index = 16
color = "#3e4a5b"
# --snip--
```

Опционально, можно определить переменные окружения эмулятора. В данном случае определим предпочтительный colorset:

```zsh
[env]
TERM = "xterm-256color"
```

> Это может поможет избежать проблем с отображением цветов в различных приложениях, например в `tmux`. 

Следует заметить, что в данном примере используется весьма ограниченный перечень настроек системы, полный перечень можно найти [по ссылке](https://alacritty.org/config-alacritty.html).

## Запуск и использование

Alacritty — это эмулятор терминала и запускается он как и любое другое приложение в операционной системе.

## Список источников

- [Alacritty configuration (alacritty.org)](https://alacritty.org/config-alacritty.html)
- [EdenEast/nightfox.nvim (github.com)](https://github.com/EdenEast/nightfox.nvim)