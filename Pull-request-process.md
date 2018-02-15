* [Creating a Pull Request](#creating-a-pull-request)
  * [Brakeman](#brakeman)
  * [Bundler audit](#bundler-audit)
  * [Git hooks](#git-hooks)
* [Special Considerations for PRs from UnboundEd](#special-considerations)

## Creating a Pull Request

Thank you for deciding to contribute! Before you create a pull request, please [create an issue](https://github.com/learningtapestry/unbounded-lcms/issues) so that the maintainers have a chance to review and discuss your suggestion. Prefer to chat instead? Feel free to post your question in the [project gitter chat](https://gitter.im/learningtapestry/unbounded-lcms).

When you create a pull request, please note whether it is a bug or an enhancement and reference the issue id (use `fixes #` syntax) so that we can track your PR to the issue. Please do add tests for your proposed change, and ensure tests pass locally before submitting your PR.

In the Pull Request include a description of what you are changing, and any manual steps needed to verify the code. Include screenshots and animated GIFs in your pull request if you are able to do so. It will help us greatly when we review your PR.

### Brakeman

To check the source code for any security issues we use [Brakeman scanner](https://brakemanscanner.org). Scans are 
running in our CI server after each commit.

To scan project locally execute the following command:

```bash
brakeman -zqA --summary --no-pager
```

That will show the scan summary. 

![](https://user-images.githubusercontent.com/784577/33357588-96cd7682-d4cb-11e7-95a0-981bc3506760.png)

To generate full report with any issues found:

```bash
brakeman -zqA --no-pager -o report.html
```

Other options are listed [here](https://brakemanscanner.org/docs/options/)

There is `config/brakeman.ignore` file to store warnings we need to ignore. Such situation may occur when scanner finds the false positive. How to use it during development you can read [here](https://brakemanscanner.org/docs/ignoring_false_positives/)

### Bundler audit

To check the project's gems for vulnerabilities we use [bundler-audit](https://github.com/rubysec/bundler-audit) gem. 
It checks gems listed in Gemfile.lock against [ruby-advisory-db](https://github.com/rubysec/ruby-advisory-db).

To check the project locally run the following:

```shell
bundler audit check --update
```

![](https://user-images.githubusercontent.com/784577/35258126-20331156-0006-11e8-9662-5366b771c3f7.png)

### Git hooks 

To automate all the checks listed above one can use pre-commit git hook. Should be placed into `.git/hooks` with 
execute permissions.

```bash
#!/usr/bin/env bash -l
#
# Pre-commit hook for a git repository

# Redirect output to stderr.
exec 1>&2

# checking gems for vulnerabilities
if ! bundler audit check --update; then
  exit 1
fi

# checking code
brakeman -zqA --summary --no-pager

exit
```

## Special Considerations

*This section only applies to maintainers who are making PRs from the original UnboundEd project*

When submitting a PR to this repo based on code from the UnboundEd repo, please be sure to squash your commits and remove the issue IDs from the original commit messages, since they will not be relevant here.
