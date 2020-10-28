# TIL

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
    "pretest": "npm link --force",
    "test": "cd test && name"
  }
}
```

The `prelink` step uses `npm link` to make the current module installed in the
global scope and it uses `--force` to replace any existing installation.

The `test` step then just `cd`s to the `test` folder and runs the command using
its binary name. Thanks to the `pretest` we know that's the latest version of
the command code as it gets put to the global scope just before we invoke it.

### 2020-10-27 JavaScript regex `.*` but not greedy

Ever needed to write a regex like this: `start.*end` and noticed that `end` will
get eaten up as a part of `.*`? Just use `.*?` and then `.*?` will only match
stuff up until `end` but will not eat `end`.

## To-Do

### Find a better way than `npm link --force` - unlink first?

2020-10-28

### Set up a TIL scanner and digest maker

Set up a GitHub Actions workflow which uses my `github-api` library to
enumerate all my public and private repositories and look through their
recent commits (since the last run - so the last day, in `master` only
or on all branches is to be decided) and pick out the ones where the
commit description starts with TIL. Collect all the TIL entries and
make a digest, which then gets appended to the existing TIL collection
in the readme of this repository which is categorized by the date.
