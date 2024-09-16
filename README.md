# test-ts-xo-ava

Minimal repo to use latest versions of [TypeScript](https://www.typescriptlang.org/) + [XO](https://github.com/xojs/xo) + [AVA](https://github.com/avajs)

## Workflow

Following `npm install`, the script `prepublish` is executed to perform the following tasks

1. `format` source files using [Prettier](https://prettier.io/)
2. `lint` source code using [XO](https://github.com/xojs/xo)
3. run `test`s using [AVA](https://github.com/avajs)
4. `build` Javascript files by transpiling TypeScript source code

## Goals

- ESM only. Source code, tests and transpiled code use ESM syntax
- Tests are writen in `.spec.ts` TypeScript files alongside source code under `src/`
- Test files should not be built

## Original state

- All packages but `typescript` are the latest available version
- AVA relies  `ts-node` and the Node.js `--loader=ts-node/esm` option in order to load the `.spec.ts` test files

```sh
> NODE_OPTIONS='--loader=ts-node/esm' ava

(node:24577) ExperimentalWarning: `--experimental-loader` may be removed in the future; instead use `register()`:
--import 'data:text/javascript,import { register } from "node:module"; import { pathToFileURL } from "node:url"; register("ts-node/esm", pathToFileURL("./"));'
(Use `node --trace-warnings ...` to show where the warning was created)
(node:24577) [DEP0180] DeprecationWarning: fs.Stats constructor is deprecated.
(Use `node --trace-deprecation ...` to show where the warning was created)

(node:24577) ExperimentalWarning: `--experimental-loader` may be removed in the future; instead use `register()`:
--import 'data:text/javascript,import { register } from "node:module"; import { pathToFileURL } from "node:url"; register("ts-node/esm", pathToFileURL("./"));'
(Use `node --trace-warnings ...` to show where the warning was created)
(node:24577) [DEP0180] DeprecationWarning: fs.Stats constructor is deprecated.
(Use `node --trace-deprecation ...` to show where the warning was created)
  ✔ dummy exists
  ─

  1 test passed
```

## Solution

After upgrading `typescript`, AVA crashes. Official documentation [hinted at `tsimp`](https://github.com/avajs/ava/blob/main/docs/recipes/typescript.md#enabling-avas-support-for-typescript-test-files) as the solution, but when in place, AVA consitently times out without running any tests. The [tentative solution](https://github.com/avajs/ava/blob/main/docs/08-common-pitfalls.md#timeouts-because-a-file-failed-to-exit) to solve the timeout issues proved to be a dead-end.

I finally ran out [a discution introducing `ts-run`](https://github.com/avajs/ava/discussions/3303) which worked perfectly.
