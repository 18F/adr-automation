# Achitectural Decision Record helpers

This repo has two GitHub Actions to help with creating architectural decision
records (ADRs). There is also an issue template to help teams draft consistent
ADRs.

By using the template together with the actions, you can manage the drafting,
acceptance, and recording of ADRs automatically.

### Issue template

The best way to use the issue template is to copy it from
[`adr-issue-template.yaml`](/adr-issue-template.yaml) into the
`.github/ISSUE_TEMPLATES/` of your repository. If you want to use a different
label to identify your ADRs, modify that file.

### Proposing an ADR

Beyond the issue template, this repo doesn't provide much for when an ADR is
first proposed. However, it can add a comment to your ADR issue letting
contributors know the process for accepting the ADR and kicking off the
automation to store it. To use this proposal action, your workflow file might
look something like this:


```yaml
name: ADR proposed
on:
  # When an issue is given a label...
  issues:
    types:
      - labeled

jobs:
  main:
    name: ADR proposed
    runs-on: ubuntu-latest

    steps:
      # ..post a comment if the issue is an ADR
      - name: post ADR comment
        uses: 18F/adr-automation/proposed@v1
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Inputs:**
|name|description|default|required|
|---|---|---|---|
|repo-token|A GitHub token with permission to write issue comments on your repository||true|
|label|The name of the issue label that indicates the issue is an ADR. This value is case sensitive!|`ADR`|false|

### Accepting an ADR

When you close an issue with the appropriate tag, the acceptance action can take
the body of your issue, turn it into an ADR document, note it with the correct
time and ADR number, and open a pull request to merge it into your repo. To use
this action, your workflow file will be something like this:

```yaml
name: ADR accepted
on:
  issues:
    types:
      - closed

jobs:
  main:
    name: ADR accepted
    runs-on: ubuntu-latest

    steps:
      - name: memorialize the ADR
        uses: 18F/adr-automation/accepted@v1
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

|name|description|default|required|
|---|---|---|---|
|repo-token|A GitHub token with permission to write issue comments on your repository||true|
|label|The name of the issue label that indicates that the ADR is accepted. This value is case sensitive!|`ADR: accepted`|false|
|path|The path in your repor where ADRs should be written.|`docs/architecture/decisions/`|false|
|pr-label|The label to apply to the created pull reqeust. Set to `true` to re-use the issue label, `false` to not label the pull request, or a string to set a custom label.|`true`|false|