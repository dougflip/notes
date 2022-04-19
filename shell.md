## HEREDOC

https://stackoverflow.com/questions/2953081/how-can-i-write-a-heredoc-to-a-file-in-bash-script

```sh
if true ; then
    cat <<- EOF > /tmp/yourfilehere
    The leading tab is ignored.
EOF
fi

cat << 'EOF' > /tmp/yourfilehere
The variable $FOO will not be interpreted.
EOF

cat <<'EOF' |  sed 's/a/b/'
foo
bar
baz
EOF

cat <<'EOF' |  sed 's/a/b/' | sudo tee /etc/config_file.conf
foo
bar
baz
EOF
```

## Suppress Output

https://stackoverflow.com/questions/46009283/how-to-suppress-stdout-and-stderr-in-bash

```sh
>/dev/null 2>&1

dpkg -s firefox >/dev/null 2>&1
```
