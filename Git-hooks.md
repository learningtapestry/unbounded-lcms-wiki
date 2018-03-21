Bellow is the pre-commit hook we use when we work on the project. It uses the set of checkers and linters for us to
be sure that the codebase is well organized, clean and there are no known vulnerabilities in any of the dependencies.

### Pre commit hook

Used to perform required checks before committing changes to the repo. Should be placed into `.git/hooks` with permission on execution.

```bash
#!/usr/bin/env bash -l
#
# Pre-commit hook for a git repository

# Redirect output to stderr.
exec 1>&2

# checking shell scripts
# shellcheck disable=SC2044
for file in $(find . -name "*.sh"); do
  if [[ "${file}" != *node_modules* ]]; then
    if ! shellcheck "${file}"; then
      exit 1
    fi
  fi
done

# checking gems for vulnerabilities
if ! bundler audit check --update; then
  exit 1
fi

# checking gems using Snyk.io
if ! node_modules/snyk/cli/index.js test --file=Gemfile.lock; then
  exit 1
fi

 # checking NodeJS modules using Snyk.io
if ! node_modules/snyk/cli/index.js test; then
  exit 1
fi

# checking code
brakeman -zqA --summary --no-pager

exit
```
