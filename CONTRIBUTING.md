# Contributing to SysAdminAutomation

Bug reports, fixes and new scripts are welcome. This page covers what a change
has to meet and how to send one. For what each script does, see the
[README](README.md).

## Start with an issue

Before you write much code, open an
[issue](https://git.coffeylabs.org/coffey-labs/SysAdminAutomation/issues) or
say hello on [community.coffeylabs.org](https://community.coffeylabs.org). Say
what you want to change, why, and which distributions you can test it on.

New accounts on git.coffeylabs.org can file issues but cannot push or fork.
Until you have repository access, attach your change to the issue as a patch:

```sh
git format-patch origin/main --stdout > my-change.patch
```

Repository access goes to volunteers we have worked with; a few good issues or
patches is the usual way in. After that you push a branch and open a pull
request. The GitHub copy is a read-only mirror, so pull requests opened there
are not merged.

## What a script has to do

Match the rest of the collection:

- `#!/bin/bash` and `set -euo pipefail`.
- The GPL header, then `# Usage:` and `# Description:` lines. The header is the
  script's documentation, so keep it accurate.
- Base utilities only. Nothing a minimal Debian, Fedora or Arch install lacks;
  where tools differ, detect and fall back, the way `network_info.sh` falls
  back from `iptables` to `nft` and `update_system.sh` picks a package manager.
- A root check on anything that changes system state. Read-only subcommands
  stay open to ordinary users.
- `--dry-run` wherever a run changes something.
- A confirmation before anything destructive, an unattended override
  (`--yes`, `ASSUME_YES=1` or `FORCE=1`), and a refusal when there is no
  terminal to ask on and no override was given.
- Output that tells "found nothing" apart from "could not look", as
  `log_inspect.sh` and `security_audit.sh` do.

A new script also needs an entry under the right heading in the README.

## Before you send it

Run ShellCheck the way CI does. The tree is clean at `warning` severity, so
anything it reports is new:

```sh
shellcheck --severity=warning *.sh
```

Then run the script. ShellCheck finds quoting mistakes, not wrong behavior.
Use a throwaway VM or container: `--dry-run` first, then for real. If the
script touches packages, services or users, try it on more than one
distribution family. Say in the issue or pull request what you ran it on.

## Pull requests

- One change per pull request, with a description of what and why.
- Keep the commit message about the change. No tooling attributions or
  generated-by footers.
- The project is licensed under the **GNU GPL v3.0**, and code you contribute
  is distributed under it.
