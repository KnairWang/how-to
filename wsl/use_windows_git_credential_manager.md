# use windows git credential manager on wsl

## reference
[github](https://gist.github.com/evillgenius75/613a44aa407300a08d0e3faea4c9df6b)

## how to
0. make sure `git` and `git-credential-manager-core` works fine on windows. Then find your git installation folder, the `git-credential-manager-core.exe` usually locate in `{git-root}/mingw64/libexec/git-core/git-credential-manager-core.exe`
1. in wsl, download latest git, see [linux/install_latest_git](../linux/install_latest_git.md)
2. make a symbol link `/usr/local/bin/git-credential-manager-core -> {path to the git-credential-manager-core.exe}`
2. in wsl, create a file under `/usr/local/bin`, name is `git-credential-manager-core`, input content like:
    ```bash
    #!/bin/sh
    exec {full-path-to-your-git-credential-manager-core.exe} $@
    # the full path is usually starts with /mnt/c/
    ```
3. in wsl, make the `git-credential-manager-core` executable:
    ```bash
    sudo chmod +x /usr/local/bin/git-credential-manager-core
    ```
4. in wsl, update the .gitconfig in `~`
    ```.gitignore
    ...
    ...
    # other config content
    ...
    ...

    [credential]
        helper = manager-core
    ```
