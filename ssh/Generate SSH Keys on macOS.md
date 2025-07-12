---
authors:
  - Anton Petrov
status: note
tags:
  - ssh
---

SSH keys enable secure, passwordless authentication when connecting to remote servers or Git repositories. This guide demonstrates how to generate and use an SSH key on macOS for both server and Git-based authentication.

## Creating an SSH Key on macOS

Begin by generating a new SSH key using the `ed25519` algorithm, which offers strong cryptographic security and efficient performance:

```shell
ssh-keygen -t ed25519 -C "email@example.com"
```

By default, this creates the private key at `~/.ssh/id_ed25519`.

> It is strongly recommended to set a passphrase when prompted during key generation to enhance the security of your private key.

Next, copy the public portion of the key to the clipboard:

```shell
pbcopy < ~/.ssh/id_ed25519.pub
```

> `pbcopy` is the preferred utility for transferring output directly to the macOS clipboard. It accepts input from standard streams and seamlessly integrates into shell-based workflows without relying on third-party software.

### Add the SSH Key to the Keychain

To make the SSH key available for automatic use by macOS, add it to the system keychain:

> Note: The `ssh-agent` must be running for this command to work properly. On macOS, it is typically available by default.

```shell
ssh-add --apple-use-keychain ~/.ssh/id_ed25519  # Requires ssh-agent
```

> This ensures your key is stored securely and can be used by applications like Git or Terminal sessions without re-entering the passphrase each time.

### Add the Public Key to the Remote Server

Now we can establish an SSH session with the remote server:

```shell
ssh username@remote-server.com
```

> For this approach, we are assuming the remote server is already configured to accept password-based SSH authentication.

Then append the public key to the `authorized_keys` file on the remote server:

```shell
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@remote-server.com  # Requires ssh-agent
```

> Note: `ssh-copy-id` requires `ssh-agent` to be running and the key to be loaded with `ssh-add`. Alternatively, we can modify `~/.ssh/authorized_keys` manually.

Finally, we can test the SSH connection to the server using the SSH key:

```shell
ssh username@remote-server.com
```

> If the setup is correct, the connection should succeed without prompting for a password.

## SSH Key for Git

Another common use of SSH is with remote Git services such as GitHub, GitLab, or Gitea. To secure repository access, we can create a service-specific key or use an existing key and register it in the service’s SSH settings.

```shell
pbcopy < ~/.ssh/id_ed25519.pub
```

Then, paste the key into your Git server account’s SSH Keys section.

> These services often allow multiple SSH keys to be associated with an account, which is useful when working from multiple devices or roles.

### Configure SSH for Git Hosts

To streamline Git operations, configure host-specific rules in the `~/.ssh/config` file:

```config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
  UseKeychain yes
```

> This ensures the correct key is used automatically for GitHub without requiring additional flags or manual configuration. This is especially useful when working with multiple services or SSH identities.

Test the SSH connection to GitHub:

```shell
ssh -T git@github.com
```

> Note: This command uses your loaded SSH key from `ssh-agent`. Ensure the key has been added with `ssh-add`. For custom Git servers, replace `github.com` with the appropriate domain.

## TL;DR

- Generate your SSH key with `ssh-keygen -t ed25519` and set a passphrase.
- Use `pbcopy` to copy the public key and `ssh-add --apple-use-keychain` to store it in the keychain.
- Add the key to the remote server with `ssh-copy-id`.
- For Git services, register your public key and configure `~/.ssh/config` for streamlined access.

## References

- [Connecting to GitHub with SSH (github.com)](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [OpenSSH Manual (openbsd.org)](https://man.openbsd.org/ssh-keygen)
