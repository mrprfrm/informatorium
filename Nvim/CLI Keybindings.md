---
authors:
  - Anton Petrov
status: Note
tags:
  - nvim
---
## Neo Vim

```
:help events
```

Displays help for built-in events.

```lua
vim.api.create_au_group("<group_name>", { clear = true})
```

Creates `au_group`(???), clear is cleans all previously created groups with the same `group_name`.

### Mapping

- `<v-i-[>`: Selects all content of nearest `[...]` and sets cursor in the beginning of it;
- `<v-i-]>`: Selects all content of nearest `[...]` and sets cursor in the end of it;
- `<v-i-{>`: Selects all content of nearest `{...}` and sets cursor in the beginning of it;
- `<v-i-}>`: Selects all content of nearest `{...}` and sets cursor in the end of it;

- `<v-i-">`: Selects all content of between nearest quotes and sets cursor in the end of it (works the same for any quote type);

- `<C-o>`: Jumps backwards in the jump list
- `<C-i>`: Jumps forward in the jump list 

## tmux

- `<cmd-c>`: Copy selected text by mouse

## Alacrity

- `<cmd-r>`: Search for recent command in history