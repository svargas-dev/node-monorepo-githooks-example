# Node Monorepo Githook Example

## What is this?

This a an example of how you could use [githooks](https://git-scm.com/docs/githooks) and shell scripts to run pre-commit hooks in a monorepo containing [TypeScript](https://www.typescriptlang.org/) sub-projects running on [Node.js](https://nodejs.org/).

## Why?

When you want to run tests against your staged files before you can commit the files. This example:

- Only checks modified workspaces
- Skips unnecessary checks
- Checks the workspace types
- Lints staged files
- Formats staged files

If any of the tests fail it will immediately exit.

### Similar projects

This offers similar functionality to these common packages:

- [Husky](https://typicode.github.io/husky/)
- [Simple Git Hooks](https://github.com/toplenboren/simple-git-hooks)

and

- [Lint-staged](https://github.com/okonet/lint-staged)

But with zero dependencies

## When might it be useful over those other packages?

- When you don't want extra dependencies in your codebase
- When you don't need all of their features
- When you can't install extra dependencies in your project
- When you want to use pre-commit githooks but your team doesn't

## How to use

### Prequisites

- Git v1.5.0+
- Unix or unix-like environment (tested with zsh on Mac 13 Ventura, Ubuntu 22.04 & Fedora 38)

### Assumptions

- Using [npm](https://github.com/npm/cli) (you can easily modify this for [yarn](https://yarnpkg.com/) or [pnpm](https://pnpm.io/))
- Workspace names match the name and case of the first level directory they are in<br>

Both can be easily edited

### Use locally without adding to the repo

1. Copy the pre-commit scripts
2. Edit `pre-commit` lines for your sub-projects e.g. 32 + 36
3. Rename each `pre-commit-*` file to match your workspace names
4. Edit each `pre-commit-*` file for your workspace name (line 3) and adjust tests for your needs
5. Make files executable e.g. `chmod +x pre-commit*`
6. Move files to `.git/hooks` in your project
7. Run `git config --local core.hooksPath .git/hooks/` to initialise the githooks

- Note: any time you edit `pre-commit` you must re-run this for changes to take effect

### Add to the repo and enable for everyone

1. Follow steps 1-5 above.
2. Choose the directory you'd like to store these scripts e.g. `.githooks/`
3. Update the workspace script paths in `pre-commit`
4. Edit your package.json adding a prepare script:

```
"scripts": {
  "prepare": "git config --local core.hooksPath .githooks/"
}
```

- Note: If using CI you will most likely not want to initialise these scripts. Take a look at the [Husky guide](https://typicode.github.io/husky/guide.html#disable-husky-in-ci-docker-prod) for further advice.

5. Run the prepare script e.g. `$ npm run prepare` or just run `$ npm i` again

### How do I opt out of the hooks?

Either:

- `git commit --no-verify`  
  or
- `git commit -n`

### Opt-in

Sometimes you don't want these checks to run by default

1. Edit `pre-commit` and uncomment lines 8-10
2. Default git commit behaviour will be unchanged and when you want to run the hooks

```
GIT_HOOKS=1 git commit
```

3. If you so choose, you could alias (git alias or shell alias) this to a shorter command

That's it!

DISCLAIMER:  
While every effort has been made that this is correct, you should understand any scripts you run from the internet.<br>
Use as your own risk.
