---
aliases:
  - Yarn migration
author:
  - Anton Petrov
last updated: 2023-10-15
spell checked: true
tags:
  - yarn
version: 0.1.0
---
Yarn Berry (новая версия Yarn) позволяет производить сборку намного быстрее, чем npm и классическая версия Yarn.

Yarn Berry также предлагает качественно отличный процесс сборки. Он позволяет пропускать шаг сборки вендорных пакетов. Пакеты из зависимостей предполагается собирать в своеобразные архивы и хранить в репозитории с кодом. Таким образом, переключение между ветками репозитория с разными пакетами в зависимостях не будет приводить к полной сборке всего проекта каждый раз. Такой подход получил лаконичное название Plug and Play (PnP).

Несмотря на все преимущества, PnP не поддерживает некоторые из фреймворков, в частности React Native, что может стать существенным препятствием при переходе на новую версию Yarn.

Однако даже без PnP, более свежая версия Yarn может похвастаться завидным приростом производительности и отлично работает с концепцией `node_modules`. Для этого достаточно определить опцию `nodeLinker` в конфигурации `yarnrc`.

```yml
nodeLinker: "node_modules"
```

## Список источников

 - [Yarn v2 concerns (reddit.com)](https://www.reddit.com/r/reactnative/comments/eyrget/yarn_v2_concerns/)
 - [Discussion around release strategy of Yarn v2 (github.com)](https://github.com/yarnpkg/berry/issues/766)
 - [Migrating from 1.x / npm (yarnpkg.com)](https://yarnpkg.com/migration/overview)
 - [Yarn PnP migration (yarnpkg.com)](https://yarnpkg.com/migration/pnp)