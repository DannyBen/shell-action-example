Running GitHub hosted shell scripts in GitHub Actions
==================================================

This is a little proof of concept for running shell scripts hosted in any
public GitHub repository, inside Github Actions.

Usage
--------------------------------------------------

1. Create a new file `.github/remote-run` in your repo with this content:

```bash
#!/usr/bin/env bash
repo=$1
script=$2
shift 2
url="https://raw.githubusercontent.com/$repo/master/$script"
curl -s $url | bash -s $*
```

2. Commit the file and set its executable bit

```shell
$ git add . --all && git commit -am "add remote runner"
$ git update-index --chmod +x .github/remote-run
```

3. From your GitHub Action steps, run any github-hosted script by providing
   a repo as the first argument, and path to the script as the second. Any
   extra argument will be passed on to the script.

```yaml
steps:
- name: Checkout code
  uses: actions/checkout@v2

- name: Run some remote script
  run: .github/remote-run dannyben/shell-action-example say-hello Thor
```
