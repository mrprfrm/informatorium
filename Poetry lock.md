---
authors:
  - Anton Petrov
status: Note
tags:
  - python
  - package_manager
---
Также lock файл можно переписать без необходимости обновлять зависимости:

```zsh
$ poetry lock --no-update
```

> Это особенно полезно если например мы перенесли зависимость в другую группу или при разрешении конфликтов.