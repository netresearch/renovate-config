# renovate-config

Org-wide Renovate shared configuration preset for Netresearch.

## What this provides

- **Stability delay**: New releases must be published for at least 3 days before Renovate will create update PRs (`stabilityDays: 3`). This reduces exposure to compromised versions that are typically detected and yanked within hours.
- **Vulnerability alerts**: Renovate will create PRs for known vulnerabilities and label them with `security` for easy triage.
- **Deny-listed versions**: Known compromised package versions are blocked via `allowedVersions` rules, preventing Renovate from ever proposing an update to those versions.

## Usage

In each repository's `renovate.json`, replace the default preset:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>netresearch/renovate-config"
  ]
}
```

This replaces `"config:recommended"` (which is already included in this preset).

## Adding a deny-listed version

When a supply chain attack is discovered in a package version:

1. Edit `default.json` in this repository.
2. Add a new entry to `packageRules`, or extend an existing entry's `allowedVersions` string. For example, to block `example-pkg@2.0.0`:

   ```json
   {
     "description": "Deny-list compromised package versions (supply chain attacks)",
     "matchPackageNames": ["example-pkg"],
     "allowedVersions": "!=2.0.0"
   }
   ```

   If the package already has an entry, append the new version constraint:

   ```json
   "allowedVersions": "!=1.14.1, !=0.30.4, !=1.15.0"
   ```

3. Commit and push to `main`. All repositories extending this preset will pick up the change on their next Renovate run.
