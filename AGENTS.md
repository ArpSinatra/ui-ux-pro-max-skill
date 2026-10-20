# Future commit identity

For new commits in this repository, use ArpSinatra
<341168454+ArpSinatra@users.noreply.github.com> as both author and committer.
Preserve historical contributors and commits; do not amend or rewrite history
to change attribution. Do not use a personal email fallback or global Git config.

In each fresh checkout/cloud session, configure repository-local identity:

```sh
for role in user author committer; do
  git config --local "$role.name" ArpSinatra
  git config --local "$role.email" 341168454+ArpSinatra@users.noreply.github.com
done
```

Before any authorized commit, run this check in the same shell/environment:

```sh
(
  expected='ArpSinatra <341168454+ArpSinatra@users.noreply.github.com>'
  for role in AUTHOR COMMITTER; do
    actual=$(git var "GIT_${role}_IDENT") || exit 1
    test "${actual%>*}>" = "$expected" || {
      printf '%s\n' "Unexpected $role identity; stop and inspect overrides." >&2
      exit 1
    }
  done
)
```

Environment variables, command-line Git config, and explicit author options can
override local settings. Do not bypass a failed check or supply conflicting
overrides. Local config stays with this checkout and is not cloned; these
instructions travel only once committed. This check creates no commit and does
not authorize committing or pushing.
