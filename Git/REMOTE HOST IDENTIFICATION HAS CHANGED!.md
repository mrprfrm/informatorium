---
tags:
 - git
 - issue
---

При попытке в очередной раз клонировать репозиторий с github.com можно столкнуться с сообщением гласящем о невозможности клонирования из-за смены идентификатора соответствующего ресурса.

```zsh
git clone git@github.com:<user_name>/<repository_name>.git
```

```zsh
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:uNiVztksCsDhcc0u9e8BujQXVUpKZIDTMczCvj3tD2s.
Please contact your system administrator.
Add correct host key in /Users/antonpetrov/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/antonpetrov/.ssh/known_hosts:8
Host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.
```

## Решение

После изменения `ssh-key` на стороне github.com эти изменения также должны быть учтены на локальной машине, в противном случае, ресурс с которого мы пытается клонировать репозиторий не может считаться надёжным, из-за чего и возникает вышеописанная ошибка.

Проверить `ssh-key` ресурса можно используя команду

```zsh
ssh-keyscan -t rsa github.com
```

Где ключ `-t` ожидает соответствующий тип алгоритма шифрования, в нашем случае `rsa`.

В качестве ответа мы получаем соответствующий ресурсу `ssh-key` и можем его сохранить в файл  `/Users/<user_name>/.ssh/known_hosts`

```zsh
# github.com:22 SSH-2.0-babeld-439edbdb
github.com ssh-rs <rsa-key>
```