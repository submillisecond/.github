# submillisecond/.github

Org-wide GitHub configuration for the [`submillisecond`](https://github.com/submillisecond)
organisation. Files in this repo apply across **all** repos under
`submillisecond/*` unless explicitly overridden in a repo's own `.github/`
directory.

## What's in here

| path | shown on | purpose |
|---|---|---|
| [`profile/README.md`](profile/README.md) | [github.com/submillisecond](https://github.com/submillisecond) | The org's landing page. Pitches the brand and lists the repos. |
| `README.md` (this file) | this repo's homepage | Explains what `submillisecond/.github` is for to maintainers. |
| `LICENSE` | this repo only | MIT - applies to the config in this repo, not to org content (each repo carries its own LICENSE). |

## How org-wide config works

A few file paths in this repo become defaults for every repo under
`submillisecond/*` that doesn't ship its own copy:

- `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`,
  `FUNDING.yml` are read from this repo unless a downstream repo defines
  its own.
- `ISSUE_TEMPLATE/`, `PULL_REQUEST_TEMPLATE.md`, `discussion-templates/`
  similarly cascade.
- `profile/README.md` is unique - it's only used here, to render the org's
  public landing page.

Today every action repo carries its own `SECURITY.md`, `CODE_OF_CONDUCT.md`,
and `CONTRIBUTING.md` (consistent enterprise-audit posture). This repo
keeps its scope narrow: just the org profile.

## Editing the profile

The org landing page renders from `profile/README.md`. Edit, commit, push -
GitHub picks up the new content within a few minutes.

```bash
git clone https://github.com/submillisecond/.github
cd .github
# edit profile/README.md
git commit -am "profile: refresh repo list"
git push
```

## Reference

- [GitHub docs - customizing your organization's profile](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile)
- [GitHub docs - default community files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
