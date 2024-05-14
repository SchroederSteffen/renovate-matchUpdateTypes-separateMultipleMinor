# renovate-matchUpdateTypes-separateMultipleMinor

Reproduction for using Renovate's `matchUpdateTypes` & `separateMultipleMinor` for the same package.

## Problem

In our org, we group all minor & patch updates (preset `group:allNonMajor`). However, we want to separate any minor TypeScript updates, because they may contain breaking changes and many tools restrict the supported minor versions.

Additionally, we want to use the option `separateMultipleMinor` to be able to perform version jumps one-by-one.

Unfortunately, Renovate doesn't allow using `matchUpdateTypes` & `separateMultipleMinor` in the same package rule because of <https://github.com/renovatebot/renovate/blob/8d78ca2ec8413739ac5c9247393ef8a147bfbd80/lib/config/validation.ts#L490>.

## Workaround

While it's not possible to use `matchUpdateTypes` & `separateMultipleMinor` in the same package rule, you can write two package rules as a workaround.

### Not Allowed

```json
{
  "packageRules": [
    {
      "groupName": "typescript",
      "matchPackageNames": ["typescript"],
      "matchUpdateTypes": ["major", "minor"],
      "separateMultipleMinor": true
    }
  ]
}
```

### Allowed

```json
{
  "packageRules": [
    {
      "groupName": "typescript",
      "matchPackageNames": ["typescript"],
      "matchUpdateTypes": ["major", "minor"]
    },
    {
      "matchPackageNames": ["typescript"],
      "separateMultipleMinor": true
    }
  ]
}
```
