## History based on files and line numbers

- https://stackoverflow.com/questions/9935379/git-show-all-of-the-various-changes-to-a-single-line-in-a-specified-file-over-t

```sh
git ld -L110,120:package.json
```

## Which commit introduced a string?

- https://stackoverflow.com/questions/5816134/how-to-find-the-git-commit-that-introduced-a-string-in-any-branch

```sh
git ld -S utf-8-validate --all --source
```
