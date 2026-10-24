# AGENTS.md

Ansible role that installs and configures Jitsi Meet with nginx and Prosody (XMPP) on Debian (bookworm, trixie).

## Layout

- `defaults/main.yml`: all role variables with defaults
- `tasks/`: `main.yml` includes `jitsi-meet.yml`, `nginx.yml`, `prosody.yml` and `ui_customization.yml`
- `templates/`, `files/`: configuration templates and static files
- `handlers/`, `meta/`: handlers and role metadata
- `molecule/default/`: integration test (`converge.yml`, `molecule.yml`)
- `requirements.yml`: role dependencies (`systemli.apt_repositories`)

## Checks

- `ansible-lint .` must pass (config in `.ansible-lint`; `molecule/` and `.github/` are excluded)
- `yamllint .` uses `.yamllint`
- `molecule test` runs the role in a container; CI runs it on Debian 12 and 13
- `ansible-lint` creates `.ansible/`; it is git ignored, never commit it

## Conventions

- Use fully qualified module names, quoted octal `mode` values and `true`/`false` booleans
- Give every task a `name`
- New or changed role variables need an entry in `defaults/main.yml` and the README

## Commits and PRs

- Write in English, use conventional commit titles (`feat:`, `fix:`, `chore:`, `ci:`; `!` for breaking changes)
- The PR title sets the release-drafter label (`enhancement`, `bug`, `chore`, `breaking`); a breaking change bumps the major version
- Open PRs as draft and fill in `.github/pull_request_template.md`: what changed and why, plus a breaking changes section if needed
- Do not list changed files or claim test results in the description; CI covers that
