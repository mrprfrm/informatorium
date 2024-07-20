---
authors:
  - Anton Petrov
status: Note
tags:
  - git
---
In case if we want to run pre-commit on already commited changes we can simply run it with the options `--from-ref` and `--to-ref`. For intance:

```zsh
$ pre-commit run --from-ref origin/dev --to-ref b4097540
```

- `--from-ref` is a target brach, where we actually want to merge changes (usually it is the same brach where we created new branch from).
- `--to-ref` is an actual reference of the brach we currently checked out. Here we can simply use short hash instead of reference name.

We can find any reference in current repository using `git show-ref`. By default it shows all references in `HEAD`.

```zsh
$ git show-ref
h213ghg23jk1h12jk3hj123j12h3kh2321j3kh21 refs/heads/master
12g3jk213jkh21k3gk21j3g1kj2g3k2hj2jh2j3h refs/heads/feature/123123/feature-name
m43b352mn77657b455bn43b453nm4b3nmb43gbn2 refs/heads/feature/185681313/tests-for-section
```