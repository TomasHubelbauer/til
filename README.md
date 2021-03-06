# TIL

## 2020-11-07 `Remove-Item -Exclude` versus `Get-ChildItem -Exclude | Remove-Item`

```powershell
Remove-Item . -Exclude file,directory,directory/* -Recurse
```

This command will preserve `directory`, but it will also wipe its contents!
Even the wildcard won't stop that.

Use this instead:

```powershell
Get-ChildItem -Exclude file,directory | Remove-Item -Recurse
```

`Remove-Item` supposedly worked as expected until PowerShell 3.0, but does no longer.

Source: https://stackoverflow.com/a/52143053/2715716

## 2020-11-01 Using Git patch instead of find and replace

I needed to replace a part of a source file in CI and wanted to do it on Windows,
but not the ugly way PowerShell does it. This was for GitHub Actions, so PS is
the default runner for `run` steps.

I figured I could just use `git diff index.js > index.patch` and then run
`git apply index.patch` in CI to get my replacing done without using PowerShell
and downloading any non-built-in software to the agent.

## 2020-10-28 MarkDown escape fenced code block

Use `~~~` to escape a fenced code block, like so:

```markdown
~~~
I can use backticks all I want in here!
~~~
```

## 2020-10-28 NodeJS `package.json` `license` values

https://github.com/jslicense/spdx-license-ids/blob/master/index.json

## 2020-10-28 Install NPM binary globally before each test run

I am working on an NPM binary module and I would like to run the module by name
during testing in a subdirectory of my repository.

The module is set up like this: there is an `index.js` in the root of the repo
where all the magic happens and then there is a `test` folder and in it are
some test files. I want to run a test which `cd`s to the `test` folder and runs
the binary as a global command in there, instead of referencing it by relative
script path.

`package.json`
```json
{
  "name": "name",
  "bin": {
    "name": "index.js"
  },
  "scripts": {
    "pretest": "npm unlink && npm link",
    "test": "cd test && name"
  }
}
```

The `prelink` step uses `npm link` to make the current module installed in the
global scope. It may be faster to use `--force` to replace the existing one,
but it prints an ugly warning.

The `test` step then just `cd`s to the `test` folder and runs the command using
its binary name. Thanks to the `pretest` we know that's the latest version of
the command code as it gets put to the global scope just before we invoke it.

### 2020-10-27 JavaScript regex `.*` but not greedy

Ever needed to write a regex like this: `start.*end` and noticed that `end` will
get eaten up as a part of `.*`? Just use `.*?` and then `.*?` will only match
stuff up until `end` but will not eat `end`.

## To-Do

### Set up a TIL scanner and digest maker

Set up a GitHub Actions workflow which uses my `github-api` library to
enumerate all my public and private repositories and look through their
recent commits (since the last run - so the last day, in `master` only
or on all branches is to be decided) and pick out the ones where the
commit description starts with TIL. Collect all the TIL entries and
make a digest, which then gets appended to the existing TIL collection
in the readme of this repository which is categorized by the date.
