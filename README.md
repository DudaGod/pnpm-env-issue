# pnpm-env-issue

Example with incorrect using env variables in npmrc when run `pnpm i` using workspace

## Problem

When using `.npmrc` with `shared-workspace-lockfile = false` and use env variable in `store-dir` or `virtual-store-dir` paths. And moreover has file `pnpm-workspace.yaml` then when executing `pnpm i` store and virtual store will be created in the folder which has env variable name, but not a value.

### How to reproduce problem

1. Run `pnpm config get` and check `HOME` env variable:
```(bash)
❯❯❯ pnpm config get
// ...
; HOME = /Users/dudagod
```
2. Run `pnpm i`:
```
❯❯❯ pnpm i
Recreating /Users/dudagod/pnpm-env-issue/node_modules
Lockfile is up to date, resolution step is skipped
Packages: +1
+
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: ${HOME}/.pnpm-env-issue/pnpm-store
  Virtual store is at:             ${HOME}/.pnpm-env-issue/pnpm-virtual-store

dependencies:
+ nop 1.0.0

Progress: resolved 1, reused 1, downloaded 0, added 1, done
Done in 894ms
```

As a result, a folder with name `${HOME}` will be created. But if you remove `pnpm-workspace.yaml` or set `shared-workspace-lockfile = true` then `${HOME}` will be replaced to `/Users/dudagod`.
